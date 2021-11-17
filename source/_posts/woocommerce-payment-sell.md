---
title: WooCommerce 金流串接實戰（十五）- 販售外掛
date: 2021-09-30 09:30:00
categories:
- WooCommerce
tags:
- WooCommerce 金流
- WordPress 外掛
- WooCommerce 外掛
- GPL
- Appsero
etoc: false
---

在完成特定客戶的需求之後，也許會有其他商家也希望可以在他們的 WooCommerce 購物網站上增加鐵人付的金流功能，這時候我們有以下幾種選擇：

1. 上架 wordpress.org 讓客戶可以在後台直接使用搜尋功能找到鐵人付外掛後安裝
2. 提供壓縮好的檔案傳送給潛在客戶請他們自行從後台安裝
3. 將外掛檔案上傳到 Github 並發行版本供客戶下載

如果是要以免費外掛來增加曝光，上架 wordpress.org 是比較好的選擇，對於客戶來說操作上最直覺，上架的步驟也很簡單，基本上就是把檔案打包好之後使用表單提交出去即可，具體的步驟可以參考[這篇文章](https://huanyichuang.com/blog/wordpress-plugin-submission/)。

<!--more-->

但有時候做好的外掛我們不一定選擇上架，有可能是等待審核的時間過長，或是這是需要收費的商業外掛，這時候我們就需要用其他方法來提供給客戶安裝。讓客戶安裝相對單純，比較麻煩的是後續的更新維護，如果是上架 wordpress.org，只要把更新檔透過 SVN 版本控管提交後，在客戶的後台就會收到更新提醒，我們完全不用擔心更新伺服器是如何跟客戶網站做通知的。

但如果我們今天要發行的是付費外掛，這個環節就需要我們自行處理，在 Github 裡面有不少的存放庫可以解決這問題，然後除了更新外，發行付費外掛還需要有序號機制、網域授權、帳號管理等流程，所以我們需要一套解決方案來開始販售外掛。

在準備展開販售外掛的事業之前，有件非常重要的事情我們必須要先了解，那就是 WordPress 的開源授權規範是採用很嚴格的 GPL，不只是核心程式，在 WordPress 裡面使用的佈景主題、外掛也直接套用 GPL 授權。這代表任何人都可以自由的修改並且散播程式碼。

也因為這樣的授權規範，販售外掛根據「程式碼」來進行收費就沒意義了，因為所有 WordPress 的程式碼都是開源的，而你客製過的程式也必須遵守開源的規範，必須以相同的授權進行免費散播，同時讓人有自由修改的權利。

那麼販售 WordPress 外掛真正賣的是什麼呢？

因為這樣的授權方式，現在可以找到一堆 GPL Download 的服務，這些服務從各種管道取得付費外掛的原始碼，然後用低於市價或是訂閱制的方式銷售這些程式，然後聲稱他們收的是「維護費」，並強調他們提供的程式 100% 正版，絕對沒有後門，而且還會定期更新。

之前看過一篇訪談，受訪者是幾間知名外掛大廠的負責人，討論對於 GPL 授權以及 GPL Download 的想法，多數老闆對於 GPL Download 斥之以鼻，但也有老闆不在意，因為他們賣的不是程式碼，而是「服務」，協助使用者解決問題或是開發更多新功能才是他們的核心價值。

而「服務」就是我們販售外掛的核心，縱使有人不知道從哪裡拿到你寫好的付費外掛，而這版本萬一被埋了後門或是套件過舊有漏洞，以及他們在使用上有任何問題，都得不到更新與維護，因此賣的是這些後續服務費用。

所以要販售外掛首先就不能在意有人「破解」你的外掛，畢竟全都是開放原始碼，要拔掉序號使用限制對於懂一點 PHP 的人來說易如反掌，與其要去防範如何讓有心人士無法破解，不如把時間放在提升產品的功能上。

但難道我們就這樣做白工便宜那些有心人士嗎？如果今天外掛已經賣翻天了我覺得再來擔心這件事比較合理，對個人開發者來說要在 WordPress 生態圈裡面找到還沒有被滿足的需求，這件事的難度絕對比防破解要高上個千萬倍。

有了這樣的心理準備之後，我們從以下幾個環節來探討該如何展開外掛販售事業：

1. 調查市場需求
2. 申請 Appsero
3. 整合 Appsero 套件
4. 建立外掛販售網站與定價


## 1. 調查市場需求

該如何知道這支外掛有沒有付費的市場需求，我從兩個角度下去切入，第一個是既有客戶請我客製化開發的第三方串接功能，像金流就是一種，當我的客戶需要，也許就會有其他客戶需要，把這樣的外掛將比較核心功能抽離出來，並把之前寫死的參數變成可以從後台設定的功能，讓客戶可以在後台直接操作。

第二個是觀察社群討論內容、近期熱門的服務，思考如果轉換成 WordPress 外掛能滿足誰的需求，同時也可以研究是否有同性質的外掛，有的話是否可以把介面改良或是增加潛在客戶可能會需要的功能，多問多聽多看自然就會有想法。

但不管是哪一種方式，沒有真正放在市場上永遠不會知道是否有人願意購買，因此寫一篇商品的介紹文就非常重要，介紹文描述這個商品可以解決什麼問題、提供哪些功能、該如何設定以及聯繫方式，就跟真正的外掛介紹頁一樣，只是還沒有開發出來而已XD

這樣做的好處是首先就能讓有需求的客戶透過關鍵字找到這篇介紹文，二來在寫這篇介紹文的同時，自己會把開發規格詳列出來，而這些規格都是要解決一個特定的問題，不然常常埋首下去開發的時候，很容易包山包海，東加西加的結果會讓商品定位不夠聚焦，自然就無法讓客戶理解這外掛可以解決他們的什麼問題。

透過介紹文可以聚焦開發規格，具體的想像要怎麼使用這個外掛，然後最好附上一些設定頁面的截圖，這些畫面不需要真的實際寫程式開發出來，我的作法是拿類似操作邏輯的外掛改圖做出來的，或是用一些做自訂欄位的外掛快速拉一下，只要記住一個原則，在還沒有確定真的會有人付費之前，半行程式都不要寫。

至於何時可以開始寫呢？我習慣是收到數封以上的來信詢問，詢問價格以及授權方式時，我就會知道這東西有市場，還有不少人會三番兩次來信催問，就能知道這個需求非常迫切，等到這個時候就能開始實作了。

## 2. 申請 Appsero

Appsero 是一套專門用來提供 WordPress 佈景主題與外掛販售的解決方案，它整合了上面提到的需求，像是更新、序號、授權控管，另外還有客戶管理、圖表分析以及自動部署的功能，同時也有 WooCommerce 外掛，基本上用了這個服務可以很快速的把銷售環節都搭建起來，不用自己寫半行程式碼。

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-01.jpg)

但既然是完整的解決方案就勢必要付出一些成本，Appsero 雖然有免費方案，但那是提供給有上架 wordpress.org 的外掛使用的，如果不上架要自己託管，就必須年繳一定的金額，它的計費方案算滿好入門的，一個月 25 鎂可以提供 500 個序號，所以最好商品的定價可以負擔這筆開銷，等到真的賣到超出這個量，可以選擇進階方案或是建置自己的解決方案。

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-02.jpg)

它的管理後台有許多實用的管理功能，首先是分析工具，透過圖表可以看到已啟用與停用的數量，並且還能看到有裝該外掛的客戶環境，像是 PHP、WordPress 版本，這樣就能根據較多人使用的環境來進行測試：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-03.jpg)

其次是客戶管理，可以看到該客戶的總購買金額以及基本資料，由於這些資料都是從 WooCommerce 送過來的，可以省去前往網站後台查看的時間：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-04.jpg)

至於序號管理可以直接在 Appsero 做停用與啟用，並且得知該序號使用在哪個網域上，或是根本沒有被啟用，同時也能知道序號到期的時間點，這樣就能在到期前提早通知客戶進行續約：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-05.jpg)

另外它還可以自訂當客戶停用外掛時的原因，方便得知客戶在使用上遇到的問題：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-06.jpg)

在實際運作之後發現 Appsero 最大的問題就是沒有中文語系，不管是他們提供的外掛或是整合套件都需要自己手動處理翻譯，但還好目前沒有遇過客戶看不懂的問題，所以綜合以上好處，我很推薦使用 Appsero 來進行事業初期的外掛販售管理。


## 3.整合 Appsero 套件

要如何讓 Appsero 認得我們的外掛並確認序號是正確可用的呢？很簡單，直接使用 composer 安裝他們的套件即可，然後用該套件提供的方法來判斷序號是否正確啟用，具體步驟如下：

### 新增外掛

申請好帳號後可以看到主畫面，點選左側選單的 Plugins，進入頁面後再點選右上角的 
Add Plugin：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-07.jpg)

選擇新增付費外掛：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-08.jpg)

輸入外掛基本資料：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-09.jpg)

將序號交由 Appero 管理：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-10.jpg)

Appsero 支援多種軟體販售平台，目前我們先選擇最後一個即可：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-11.jpg)

下一步會問你想要用哪一種方式進行銷售，可以使用 WooCommerce 或是另外一套在 WordPress 上面販售虛擬商品的 Easy Digital Download 外掛，但以目前台灣的金流收款考量，選擇 WooCommerce 能獲得比較大的支援度，因此這邊先選擇 WooCommerce：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-12.jpg)

最後選擇 Appsero 來管理序號：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-13.jpg)

以及在販售的網站上面安裝 Appsero 的外掛並將 Key 貼入後進行驗證：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-14.jpg)

上述步驟都完成後，會提示套件的安裝說明：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-15.jpg)

把 ```composer require appsero/client``` 複製下來，稍後會回到我們外掛的目錄來進行安裝，點選下一步，Appsero 提供了一個函式來初始化套件，包含建立實 例、資料蒐集、更新以及輸入序號的選單註冊，另外它還貼心了提供隱私權政策範本，告知本外掛會收集哪些資料。


### 套件安裝

接下來開啟編輯器，在我們的外掛資料夾裡面執行安裝：

```sh
iron-pay$ composer require appsero/client
```

安裝好之後開啟 ```iron-pay.php```引入 appsero 的套件，

```php
// 略
/**
 * Initialize the plugin tracker
 *
 * @return void
 */
if ( ! class_exists( 'Appsero\Client' ) ) {
		require_once __DIR__ . '/appsero/src/Client.php';
}

$client = new Appsero\Client( 'xxxx-xxx-xxx-xxxxxxx', '鐵人付金流', __FILE__ );

// Active insights.
$client->insights()->init();

// Active automatic updater.
$client->updater();

if ( $client->license()->is_valid() ) {
	\A7\autoload( plugin_dir_path( __FILE__ ) . 'src' );
	// 略
}
```

關鍵是 ```$client->license()->is_valid()``` 這個判斷式，這代表只有當客戶的序號通過驗證時才會執行的內容，我把載入核心檔案的方法寫在這裡面，也就是序號通過才會載入主程式，所以要「破解」也非常容易，把載入程式移出判斷式就好，因此要謹記，我們賣的是服務不是程式碼。

### 外掛更新

Appero 是使用 Release 來控管更新的推播，版本更新有兩種方式，一種是從 Appsero 後台直接上傳壓縮檔，另外一種是用 Github 的 tag 來取得最新版本，使用後者會比較方便，因此本文以 Github 來示範如何做到自動部署新版本。

首先我們要先把我們的存放庫跟 Appsero 做綁定，在 Appsero 後台找到 Intergration 的選單，進入後可以看到 Github 的區塊，選擇 iron-pay 的存放庫即可完成綁定：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-17.jpg)

接下來回到編輯器，在外掛根目錄新增一個 Readme.txt 文字檔，裡面紀錄了外掛的更新資訊、相容版本以及已測試過的 WordPress 版本，範例如下：

```
=== Iron Pay ===
Requires at least: 4.9.8
Tested up to: 5.8
Requires PHP: 7.0
Stable Tag: 1.0.0
WC requires at least: 5.0
WC tested up to: 5.5.2
License: GPLv3
License URI: https://www.gnu.org/licenses/gpl-3.0.html

== Description ==

鐵人付外掛為鐵人賽的 WooCommerce 金流外掛

== Installation ==

請參考安裝教學:https://oberonlai.blog/tw/wordpress-plugin-sell

== Changelog ==
<p>v1.0.0 - 新增版本資訊</p>
```

這個資訊會顯在 WordPress 後台的外掛資訊內，需要注意的是 Stable Tag，這邊的版本號是 Appsero 判斷是否有新版本提供的資訊，如果今天版更為 1.0.1 的話，這邊也記得要修改。

接下來當我們增加了新功能或是除完錯之後，可以先使用前一篇提到的 Buddy 來做自動化測試，確認無誤後就能用 ```git tag``` 來新增發行版本並推到主線上：

```sh
iron-pay$ git tag -a v1.0.1 -m '新增功能'
iron-pay$ git push origin v1.0.1
```

透過這個動作 Appsero 就能知道有新版本推送了，它就會主動去推播給有安裝該外掛的 WordPress 網站，同時也能在 Appsero 後台的 Release 看到發行版本紀錄：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-sell/wordpress-plugin-sell-18.jpg)

## 4. 建立外掛販售網站與定價

接下來我們就可以用 WooCommerce 建立一個購物網站來販售這支外掛，記得要先安裝 [Appsero Helper](https://wordpress.org/plugins/appsero-helper/)，這樣當訂單付款完成後顧客會收到下載連結，以及可以在網站這邊登入帳號讓顧客查看序號以及購買紀錄。

另外我還推薦以下幾支賣虛擬商品會用到的免費外掛：

- **[Autocomplete WooCommerce Orders](https://wordpress.org/plugins/autocomplete-woocommerce-orders/)** - 自動將虛擬商品的已付費訂單變更為已完成，這樣顧客才能收到下載連結
- **[Change Quantity on Checkout for WooCommerce](https://wordpress.org/plugins/change-quantity-on-checkout-for-woocommerce/)** - 讓顧客可以在結帳頁修改商品數量，避免讓顧客重複購買虛擬商品
- **[Checkout Field Editor for WooCommerce](https://wordpress.org/plugins/woo-checkout-field-editor-pro/)** - 調整結帳欄位把不需要的地址、電話拔掉，販售虛擬商品只要填寫電子郵件即可
- **[Direct Checkout for WooCommerce](https://wordpress.org/plugins/woocommerce-direct-checkout/)** - WooCommerce 預設的購物流程是先進購物車再進資料填寫頁，如果只有販售一個商品就能用這支外掛簡化這個步驟
- **[My Custom Functions](https://wordpress.org/plugins/my-custom-functions/)** - 不用開編輯器就能夠直接在後台寫 PHP
- **[RY WooCommerce Tools](https://tw.wordpress.org/plugins/ry-woocommerce-tools/)** - 功能很強大的金流外掛，可以串接多家台灣第三方金流
- **[WPForms Lite](https://wordpress.org/plugins/wpforms-lite/)** - 表單外掛，作為顧客問題回報的管道

以上最麻煩的部分是收款，現在台灣第三方金流控管比較嚴格，要販售虛擬商品需要進行審核，如果真的申請不過，那就只能先提供匯款帳號的方式來處理，但就必須要人工對帳以及手動開通外掛下載權限。

至於一套外掛該賣多少錢呢？我個人是覺得至少要能打平做這件事的後續服務成本，因此定價最好不要太低，常看到國外的付費外掛功能強大到不行但卻只賣十幾二十塊美金，我是覺得不能用國外的付費外掛價格來當作對照組，而是要根據本地市場來測試合適的價格區間，現在台灣也有很多廠商專門在販售 WordPress 付費外掛，以他們的價格當參考會比較準確。

萬一想提高單價但又沒有比其他廠商多出強大的功能，這時候我們可以從方案下手，像是授權一個網域是一個價格，授權十個或是無限制是另一個價，也可以把多個外掛做組合銷售，都是可以提高銷售額卻又不會增加成本的作法，總之就是多方嘗試，找到市場可以接受的價格區間。

## 結語

WordPress 除了提供給不會寫程式的人架設功能豐富的網站外，也讓開發者有了很多機會來拓展自己的事業，不管是靠它接案還是販售付費外掛，甚至是最後被大廠商併購，這在 WordPress 生態圈都已經不是新聞了，因此重點是不停止學習就可以發現更多的機會，持續學習才能不被既有的知識所侷限！