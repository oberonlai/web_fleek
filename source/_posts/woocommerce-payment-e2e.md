---
title: WooCommerce 金流串接實戰（十二）- 端對端測試
date: 2021-09-27 09:30:00
categories:
- WooCommerce
tags:
- WooCommerce 金流
- WordPress 外掛
- WooCommerce 外掛
- 自動化測試
- e2e
etoc: false
---

曾經做過一個專案，顧客把商品加入購物車後，可以同時選擇要加入幾筆商品，然後在結帳頁的時候需要根據商品數量來增加報名資料的欄位，也就是說如果購物車裡面有三筆資料，一樣的報名表單就需要填寫三份不同的客戶資料，有點類似旅行社在報名前需要填寫所有成員的資料一樣。

然後某天接到客戶反應說當只有一位報名時功能正常，但只要是多位報名會有問題，我就依照慣例開始手動測試，每一個報名要填的欄位有 8 個，所以每測試一次我至少要填 8 x 2 = 16 個欄位，用瀏覽器記憶的功能輸入或許不會花太多時間，但萬一客戶反應同時報名五個人以上會有問題，我就要填 40 個以上的欄位了，在找尋有沒有更快的測試方法時，才知道有端對端測試工具這種好物。

<!--more-->

所謂的端對端測試就是透過 JS 來自動操縱瀏覽器來依序執行頁面跳轉、按鈕點擊、填寫表單等真實使用者的瀏覽行為，它可以帶來以下好處：

- 確保我們寫好的程式碼是可以被使用者正常操作的
- 測試程式碼寫好一次後可以重複利用
- 減少手動測試的人為錯誤
- 減少手動測試的時間
- 安排固定時間自動執行測試
- 提升程式碼品質

現在市面上有超多廠商在提供這樣端對端測試的 SaaS 服務，像是 Ghost Inspector、Lambadtest、Usertrace 等等，只要搜尋 auto test tool 就可以找到一大堆，而本文要介紹的是 WooCommerce 官方提供的 wc-e2e-page-object。

它是一個 JS 的套件，整合斷言庫 Chai.js 以及 WooCommerce 常見的操作行為，像是前台加入購物車、點擊結帳按鈕，或是後台的新增商品、編輯訂單資料等等，都可以透過這個套件來進行操作。

## 安裝套件

首先為了跟 PHPUnit 的資料夾區分，我們新建一個 e2e 資料夾，裡面會放 JS 的測試```place-order.js```，接著新增 ```.babelrc``` 貼入並以下內容：

```
{
    "presets": [
        "es2015",
        "stage-2"
    ],
    "plugins": [
        "add-module-exports"
    ]
}
```

wc-e2e 套件需經由 NPM 安裝，須確保電腦可執行 Node.js，切換到外掛根目錄下，新增 package.json 檔，並貼入以下內容：

```json
{
  "name": "iron-pay",
  "version": "1.0.0",
  "description": "",
  "scripts": {
    "test": "cross-env NODE_CONFIG_DIR='./config' BABEL_ENV=commonjs mocha e2e/place-order.js --compilers js:babel-register --recursive"
  },
  "dependencies": {},
  "devDependencies": {
    "babel": "^6.5.2",
    "babel-cli": "^6.14.0",
    "babel-eslint": "^7.0.0",
    "babel-plugin-add-module-exports": "^0.2.1",
    "babel-preset-es2015": "^6.14.0",
    "babel-preset-stage-2": "^6.13.0",
    "chai": "^3.5.0",
    "chai-as-promised": "^6.0.0",
    "config": "^1.24.0",
    "cross-env": "^3.0.0",
    "istanbul": "^1.0.0-alpha",
    "mocha": "^3.0.2",
    "wc-e2e-page-objects": "0.2.2",
    "chromedriver": "^93.0.1"
  }
}
```

需要注意的地方是 scripts 的地方，要把 test 路徑設定為我們的 e2e 資料夾裡面的測試程式碼 ```place-order.js```：

```
"test": "cross-env NODE_CONFIG_DIR='./config' BABEL_ENV=commonjs mocha e2e/place-order.js --compilers js:babel-register --recursive"
```

最後是新增一個 config 資料夾裡面放跑端對端測試時的設定檔 ```default.json```：

```json
{
    "url": "https://woocommerce.test",
    "users": {
        "admin": {
            "username": "woocommerce",
            "password": "password"
        },
        "customer": {
            "username": "",
            "password": ""
        }
    },
    "startBrowserTimeoutMs": 30000,
    "mochaTimeoutMs": 120000
}
```

url 是本機 WordPress 的路徑，並提供管理者的登入帳密，完成以上資料準備後，就能進行套件的安裝：

```sh
iron-pay$ npm install
```

安裝完成後外掛資料夾結構如下：

```
iron-pay
├── composer.json
├── composer.lock
├── config
│   └── default.json
├── e2e
│   └── place-order.js
├── iron-pay.php
├── node_modules
├── package-lock.json
├── package.json
├── phpunit.xml
├── src
│   ├── Gateways
│   ├── Options.php
│   ├── Posts
│   └── Utility.php
├── tests
│   ├── Gateways
│   ├── OptionsTest.php
│   ├── Posts
│   └── UtilityTest.php
└── vendor
```


## 撰寫測試程式

接著我們要撰寫測試程式碼，我們的測試流程為將商品加入購物車 -> 確保購物車內有該商品 -> 進入結帳頁 -> 選擇鐵人付信用卡金流 -> 按下結帳按鈕成立訂單

開啟 ```place-order.js```，貼入以下測試的架構：

```js
import config from 'config';
import chai from 'chai';
import chaiAsPromised from 'chai-as-promised';
import test from 'selenium-webdriver/testing';

// Helper objects for performing actions.
import { WebDriverManager, WebDriverHelper as helper } from 'wp-e2e-webdriver';

// We're going to use the ShopPage and CartPage objects for this tutorial.
import { ShopPage, CartPage, CheckoutPage } from 'wc-e2e-page-objects';

chai.use( chaiAsPromised );
const assert = chai.assert;

let manager;
let driver;

test.describe( '鐵人付信用卡測試', function() {

    // Set up the driver and manager before testing starts.
    test.before( 'open browser', function() {
        this.timeout( config.get( 'startBrowserTimeoutMs' ) );

        manager = new WebDriverManager( 'chrome', { baseUrl: config.get( 'url' ) } );
        driver = manager.getDriver();

        helper.clearCookiesAndDeleteLocalStorage( driver );
    } );

    this.timeout( config.get( 'mochaTimeoutMs' ) );

    // 測試

    test.after( 'quit browser', () => {
        manager.quitBrowser();
    } );

} );
```

基本架構為先引入測試要用的套件，分別匯入 ShopPage、CartPage、CheckoutPage 三個由 wc-e2e-page-objects 提供的模組，這些模組都有各自的方法來進行介面的互動。

測試的任務從 ```test.describe()``` 開始，先定義這個任務的名稱，然後開啟瀏覽器、檢查是否逾時，到最後關閉瀏覽器，介於 ```test.before()``` 跟 ```test.after()``` 中間就是我們要執行的動作。

首先是將商品加入購物車：

```js
// 略
test.describe( '鐵人付信用卡測試', function() {

    // Set up the driver and manager before testing starts.
    test.before( 'open browser', function() {
      // 略
    } );

    this.timeout( config.get( 'mochaTimeoutMs' ) );

    // 測試加入購物車
	test.it( '將商品加入購物車', () => {
        const shopPage = new ShopPage( driver, { url: manager.getPageUrl( '/shop' ) } );
        const added_product = shopPage.addProductToCart( 'Flying Ninja' );
        assert.eventually.ok( added_product );
    } );

    test.after( 'quit browser', () => {
      // 略
    } );

} );
```

宣告 ShopPage 的實例，並且將本機頁面跳轉至 shop 頁面，接下來使用 ShopPage 物件的 ```addProductToCart()``` 方法，將商品 Flying Ninja 加入購物車，該方法會回傳布林值，所以用斷言 ```assert.eventually.ok( added_product )``` 檢查是否回傳為 true，完成後執行 ```npm run test```，就會看到 Chrome 瀏覽器被啟用，並且自動執行加入購物車的動作，並在命令列裡面看到執行成功的訊息：

```sh
iron-pay$ npm run test

> iron-pay@1.0.0 test /Users/oberonlai/Sites/woocommerce/wp-content/plugins/iron-pay
> cross-env NODE_CONFIG_DIR='./config' BABEL_ENV=commonjs mocha e2e/place-order.js --compilers js:babel-register --recursive



  鐵人付信用卡測試
    ✓ 將商品加入購物車 (1936ms)


  1 passing (4s)
```

接下來加入第二個任務，讓頁面自動跳轉到購物車並且確認裡面有剛剛加入的商品名稱：

```js
// 略
test.describe( '鐵人付信用卡測試', function() {

    // Set up the driver and manager before testing starts.
    test.before( 'open browser', function() {
      // 略
    } );

    this.timeout( config.get( 'mochaTimeoutMs' ) );

    // 測試加入購物車
	test.it( '將商品加入購物車', () => {
      // 略
    } );
	
	// 測試購物車有正確加入商品
	test.it( '前往購物車', () => {
        const cartPage = new CartPage( driver, { url: manager.getPageUrl( '/cart' ) } );
        const has_product = cartPage.hasItem( 'Flying Ninja' );
        assert.eventually.ok( has_product );
    } );

    test.after( 'quit browser', () => {
      // 略
    } );

} );
```

這邊我們使用的是 CartPage 物件，並且用它的 ```hasItem()``` 方法來檢查購物車裡面是否有 Flying Ninja 這個商品，如果有的話就能通過斷言：

```sh
iron-pay$ npm run test

iron-pay@1.0.0 test /Users/oberonlai/Sites/woocommerce/wp-content/plugins/iron-pay
> cross-env NODE_CONFIG_DIR='./config' BABEL_ENV=commonjs mocha e2e/place-order.js --compilers js:babel-register --recursive



  鐵人付信用卡測試
    ✓ 將商品加入購物車 (2302ms)
    ✓ 前往購物車 (831ms)


  2 passing (5s)
```

最後是測試鐵人付信用卡結帳，測試流程是選擇信用卡並點擊前往付款按鈕：

```js
// 略
test.describe( '鐵人付信用卡測試', function() {

    // Set up the driver and manager before testing starts.
    test.before( 'open browser', function() {
      // 略
    } );

    this.timeout( config.get( 'mochaTimeoutMs' ) );

    // 測試加入購物車
	test.it( '將商品加入購物車', () => {
      // 略
    } );
	
	// 測試購物車有正確加入商品
	test.it( '前往購物車', () => {
       // 略
    } )
	
	// 測試能夠使用鐵人付前往結帳
	test.it( '前往結帳頁並下訂單', () => {
        const checkoutPage = new CheckoutPage( driver, { url: manager.getPageUrl( '/checkout' ) } );
        const select_payment = checkoutPage.selectPaymentMethod( '信用卡' );
        const place_order = checkoutPage.placeOrder();
        
        assert.eventually.ok( select_payment );
        assert.eventually.ok( place_order );
    } );

    test.after( 'quit browser', () => {
      // 略
    } );

} );
```

這邊使用 CheckoutPage 物件，並利用 ```selectPaymentMethod()``` 與 ```placeOrder()``` 這兩個方法來執行選擇付款方式與結帳的動作，要注意的地方是不管是哪種方法，要傳送的參數都是頁面上看得到的名稱，而非欄位的 id 或 value 值，畫面上看到什麼就傳什麼，執行結果如下：

```sh
iron-pay$ npm run test

> iron-pay@1.0.0 test /Users/oberonlai/Sites/woocommerce/wp-content/plugins/iron-pay
> cross-env NODE_CONFIG_DIR='./config' BABEL_ENV=commonjs mocha e2e/place-order.js --compilers js:babel-register --recursive



  鐵人付信用卡測試
    ✓ 將商品加入購物車 (1972ms)
    ✓ 前往購物車 (487ms)
    ✓ 前往結帳頁並下訂單 (786ms)


  3 passing (5s)
```

這樣就能確保使用鐵人付信用卡是可以正常建立訂單的。


當我們寫了測試來檢查自己的程式碼時，目的不是消除所有錯誤，也並非是寫了測試功能就完全不會有臭蟲，而是讓我們在修改程式的當下，萬一引發某些錯誤時可以立即透過測試來得知。

像上面的這個測試我們就無法測到金流商 API 發生錯誤時的情境，但因為有了這些測試，我們可以快速定位到是因為回傳資料有誤進而追查，而非又要翻原始碼去逐一定位問題所在，有了測試我們可以把除錯範圍縮小，減少除錯的時間。

當我們在本機測試完成後，接下來就要進入測試機的階段，下一篇會介紹外掛的部署流程，以及如何使用 VPS 建置測試站。