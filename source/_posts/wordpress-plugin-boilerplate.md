---
title: 【 心得 】WordPress 外掛開發日記 (一) - 架構
date: 2021-06-15 18:00:00
categories:
- 外掛開發
tags:
- boilerplate
- composer
- phpcs
- git
etoc: false
---

一年前的這個時候，從早到晚都在跟 WordPress 的佈景主題打交道，除了忙案子以外，空檔時就在研究哪些前端框架可以更快速的與 WordPress 做整合，想不到十二個月過去了，現在每天都改成在寫外掛，離前端程式開發越來越遠，停下腳步回頭來看，感覺是該來好好整理一下這陣子的開發心得了。

為了方便閱讀我拆成以下幾個章節來紀錄：

[一、WordPress 外掛架構 - 紀錄曾經使用過的架構以及目前作法](https://oberonlai.blog/tw/wordpress-plugin-boilerplate)
[二、WordPress 開發實用 Composer 套件 - 介紹在寫外掛的時候可以加速開發的套件](https://oberonlai.blog/tw/wordpress-and-composer)
三、載入佈景主題範本 - 如何在外掛裡面使用範本檔修改前端介面並整合前端開發流程
四、整合 PHPUnit 單元測試 - 避免改東牆壞西牆的問題再次發生
五、自動化部署 - 讓 WordPress 搭上 DevOps 的列車🚄
六、WordPress 外掛更新 - 自行管理未上架 wordpress.org 的外掛版本維護

如果是對 WordPress 外掛開發完全沒概念的朋友，我建議先閱讀 [Plugin Developer Handbook](https://developer.wordpress.org/plugins/) 會比較完整，或是買[這本書](https://oberonlai.blog/tw/building-web-apps-with-wordpress/)來看在學習上會比較有脈絡與系統性。而這系列文是我的實務經驗，所以很多地方可能還是會有不足之處，希望能遇到大大再幫我指點迷津了 🙏

第一篇文章主要紀錄了我在組織外掛結構的過程，並透過不斷的打掉重練來找到自己最舒服的方式，讓我們開始吧～

<!--more-->

雖然不知道出來混為何一定要還，但還是手癢開始把這一年多來改寫外掛的日常紀錄一下XD，從開始入坑 Composer 到學著寫下第一行單元測試，再到外掛的自管更新以及自動化部署，寫到一半發現餅華太大，只好拆成多篇來寫 > < 第一篇介紹目前我慣用的 Starter Plugin ( 好像沒這種說法XD ) 與工作流程，以及隨著時間演進的組織方法，所以萬一明天點開來看發現跟今天看到的內容不一樣還請見諒，文章也是需要重構的XD 文中如有不足之處還煩請大大指點迷津了 🙏

## WordPress 外掛架構

以佈景主題來說，有很多公司推出初始佈景主題 ( Starter Theme ) 可以入門，相較之下外掛就少很多，原因可能是外掛的組織方式太自由了，基本上我可以把所有的程式碼全都塞進到帶有標頭的 PHP 檔案即可，不需要拆分任何檔案與資料夾，但如果是做功能複雜或是大型外掛的擴充，這樣做會被未來的自己罵到臭頭XD

剛開始實作的時候我是採用「 [WordPress Plugin Boilerplate Generator](https://wppb.me)」，不管要做的功能是大還是小，我覺得遵循這套架構可以讓檔案很有邏輯與系統，它的特色是在於把前後台的資料夾切分開來，然後透過一層又一層的類別來做載入。

```
── LICENSE.txt
├── README.txt
├── admin
│   ├── class-my-plugin-admin.php
│   ├── css
│   │   └── my-plugin-admin.css
│   ├── index.php
│   ├── js
│   │   └── my-plugin-admin.js
│   └── partials
│       └── my-plugin-admin-display.php
├── includes
│   ├── class-my-plugin-activator.php
│   ├── class-my-plugin-deactivator.php
│   ├── class-my-plugin-i18n.php
│   ├── class-my-plugin-loader.php
│   ├── class-my-plugin.php
│   └── index.php
├── index.php
├── languages
│   └── my-plugin.pot
├── my-plugin.php
├── public
│   ├── class-my-plugin-public.php
│   ├── css
│   │   └── my-plugin-public.css
│   ├── index.php
│   ├── js
│   │   └── my-plugin-public.js
│   └── partials
│       └── my-plugin-public-display.php
└── uninstall.php
```

但經過幾個專案後發現不太順手，問題在於它把勾點 ( Hook ) 跟要跑在勾點上的函式切開來，常常在找這支函式是跑在哪個勾點上時會花上一些時間，更不用說在寫勾點的時候還需要切換檔案很容易混亂。

其次是很多功能很難界定它到底是屬於前台還後台，像是在做 REST API 我有時候會放在 public，有時候忘記又會放在 admin，要找的時候自己都會搞混。

然後最麻煩的地方是檔案的載入，WPPB 是使用 require 的方式來引入，所以如果新增檔案或是修改檔名的時候都會需要手動修改，最常發生的情況就是類別寫完後一直跑不動，追查後才發現根本忘記引入了 😅

所以一直在找尋著有沒有更好的架構可以來開發外掛，找了老半天都沒有適合自己的，索性就來發展一套自己的邏輯吧～

剛好當時在看「[現代 PHP](https://www.books.com.tw/products/0010688181) 」這本書，對於 Composer 很有興趣，就開始試著把 Composer 整合進 WordPress 的開發流程之中。

Composer 可以簡單的理解為是一套可以安裝 PHP 外掛的工具，可以把別人寫好的 PHP 類別透過 Composer 安裝後進行使用。

除此之外，它還可以幫助開發者做自動載入、套件更新管理，甚至還可以安裝 WP 的外掛，透過它來管理 WP 網站的外掛，你也可以上傳自己寫的套件，把一些常用的類別打包好後上傳，下次要用時直接透過 Composer 進行安裝。

更多 Composer 的實作細節會隨著下文逐一帶到，回到 WordPress 外掛開發架構的需求，我想要的是可以依照專案規模自行增加資料夾結構以及檔案命名，尤其是外掛開發的初期功能還很陽春，可能幾個檔案就搞定，等到外掛發展的越來越大，我可以隨時調整檔案結構以符合商業邏輯的變動。

參考了很多免費與付費外掛的結構後，目前已發展出一套自己的 SOP，大概的順序如下

### 1.建立基本架構

建立外掛的第一步就是在 wp-content/plugins 底下建立資料夾，然後新增主檔案與 src (source) 資料夾放主程式，這樣就完成最基本的結構了。

```
my-plugin
├── my-plugin.php
└── src
```

如果專案有前端的部分需要處理，我會開 assets 資料夾，裡面會放圖檔、CSS 與 JavaScript 等靜態檔案：

```
my-plugin
└── assets
    ├── css
    │   ├── admin.css
    │   └── public.css
    ├── img
    │   └── img1.jpg
    └── js
        ├── admin.js
        ├── ajax.js
        └── public.js
```

前後台的靜態檔就用檔名來區分，前台就是 public 後台是 admin，如果有需要做 Ajax 的 JS 檔也是會放在這邊。如果要進一步的整合 Node.js 來做前端的檔案編譯，assets 下面會再區分 src 跟 dist，然後才放 img、css 與 js 資料夾，之前有實作過整合最近非常紅的 CSS 框架 Tailwindcss，這部分會在之後的文章提到。

再來如果這個外掛有需要處理範本的話，我會再另外開一個 views 資料夾，這資料夾就像是佈景主題一樣，專門放範本檔：

```
my-plugin
└── views
    ├── loops
    │   └── post.php
    ├── page-about.php
    ├── page-contact.php
    ├── partials
    │   └── pagination.php
	├── woocommerce
    │   └── checkout
    ├── template-2col.php
    └── template-blank.php
```

views 除了放對應 slug 的範本檔之外，也放了頁面範本，loops 資料夾放的是需要跑在 WP_Query 裡面的列表類內容，而 partials 放的是所有頁面會共用到的元件，像是分頁導覽 pagination。如果是做 WooCommerce 的 Extension 我會再多開一個資料放 WC 的範本檔，這樣就能從外掛來修改 WC 相關頁面的內容。

再來是 src 裡面的結構，嘗試了很多命名方法，最喜歡的應該是 MVC 模式：

```
my-plugin
└── src
    ├── controllers
    │   └── cpt.php
    └── models
        └── cpt.php
```

View 的部分我把它獨立在 src 的資料夾外面，原因是 src 的資料夾會透過 PSR-4 的 Autoload 載入，但 WP 的範本檔並不支援這樣的機制，所以把 views 獨立在 src 以外避免被自動載入。

Models 資料夾我放的是需要新增或修改資料庫結構的類別，最常見的就是新增 Custom Post Type，或是需要修改預設的文章或頁面 post type、新增 options、widget，我都會放在 Models 資料夾下面。

Controllers 則是負責處理 Models 裡面各種 post type 或是 option 的 CRUD，以及各種 WP 內建機制的處理，像是 REST API、WP Cron、Shortcode 等等，基本上這個資料夾會放所有的邏輯處理類別，需要呼叫類別的程式也是放在這邊。

如果有多個 CPT 需要處理多種操作，我會再新增新的資料夾來做區分：

```
my-plugin
└── src
    ├── controllers
    │   ├── cpt.php
    │   ├── cpt1
    │   │   ├── Api.php
    │   │   ├── Router.php
    │   │   ├── Post.php	
    │   │   └── Shortcode.php
    │   └── cpt2
    │       ├── Post.php
    │       └── Widget.php
    └── models
        └── cpt.php
```

雖然從 MVC 的角度下去分類看似很清楚，但實作後常發現自己會混淆 M 跟 C 的部分，我曾經在一個專案中把 Shortcode 放在 Controllers，另一個專案卻放在 Models 裡面，或是要註冊 Rest API 我就常會舉棋不定，這到底該放在哪裡好？

於是索性就不分 Models 跟 Controllers 了，直接以 WordPress 資料表結構來區分資料夾：

```
my-plugin
└── src
    ├── posts
    │   ├── cpt1
    │   │   ├── Ajax.php
    │   │   ├── Field.php
    │   │   └── Cpt1.php
    │   └── cpt2
    │       ├── Api.php
    │       ├── Cpt2.php
    │       └── Shortcode.php
    ├── terms
    │   ├── taxonomy1
    │   │   └── Field.php
    │   └── taxonomy2
    │       └── Api.php
    ├── options
    │   ├── Field.php
    │   └── Menu.php
    └── user
        └── Field.php
```

最常處理的是 wp_posts，所以 posts 資料夾下面會放各個 CPT 的資料夾，options 是做設定頁面的，對應的資料表是 wp_options，user 是使用者相關，對應的資料表是 wp_users，至於 Taxonomy 我也會跟 Post 拆開獨立放在 terms 的資料夾底下，在結構上會更清楚些。

然後每個 cpt 的資料夾底下，會有一支跟 CPT slug 同名的檔案，裡面放的會是註冊 CPT 的功能，下一篇文章會示範如何使用 Composer 套件來註冊 CPT，其他的 Field.php 、Ajax.php、Api.php 就是管理該 CPT 的相對應功能。

所以整個資料夾的邏輯就是以資料類型 > 功能來區分，對我來說可以再也不用判斷這功能到底是屬於 Models 還是 Controllers。

最後則是單元測試的資料夾：

```
my-plugin
└── tests
    └── cpt
        └── cptTest.php
```

測試的部分比較複雜，之後會再以獨立篇幅來進行說明。

另外如果是要做 i18n 本地化，就需要再新增一個 languages 資料夾，翻譯用的 .pot 檔可以透過 WP CLI 產生。

```
$ wp i18n make-pot . languages/my-plugin.pot
```

資料夾結構會是這樣：
```
my-plugin
└── languages
    └── my-plugin.pot
```

最後視專案的需求，資料夾結構可以是這樣：

```
my-plugin
├── my-plugin.php
└── src
    └── cpt1
        └── Cpt1.php
```

也可以逐漸發展成這樣：

```
my-plugin
├── assets
│   ├── css
│   │   ├── admin.css
│   │   └── public.css
│   ├── img
│   │   └── img1.jpg
│   └── js
│       ├── admin.js
│       ├── ajax.js
│       └── public.js
├── my-plugin.php
├── src
│   ├── posts
│   │   ├── cpt1
│   │   │   ├── Ajax.php
│   │   │   ├── Field.php
│   │   │   └── Cpt1.php
│   │   └── cpt2
│   │       ├── Api.php
│   │       ├── Cpt2.php
│   │       └── Shortcode.php
│   ├── terms
│   │   ├── taxonomy1
│   │   │   └── Field.php
│   │   └── taxonomy2
│   │       └── Api.php
│   ├── options
│   │   ├── Field.php
│   │   └── Menu.php
│   └── user
│       └── Field.php
├── tests
│   └── cpt
│       └── cptTest.php
└── views
    ├── loops
    │   └── post.php
    ├── page-about.php
    ├── page-contact.php
    ├── partials
    │   └── pagination.php
    ├── template-2col.php
    ├── template-blank.php
    └── woocommerce
```

在實務中我最常變動的部分是 src 資料夾，除了需求的新增外，做程式碼重構也都會常常修改這邊，以往只要資料夾名稱或是檔案有新增刪除，透過手動引入的方式就必須要全部修改，有沒有更方便且更彈性的作法呢？有！答案就是 Composer。

### 2.設定 Composer 環境

安裝 Composer 的教學網路非常多，這邊主要示範給 WordPress 外掛開發者的環境設定。第一步先開啟終端機然後把目錄切到正在開發的外掛底下：

```
$ cd wp-content\plugins\my-plugin
```

然後執行 composer init

```
$ composer init
```

composer 會用對話式的介面來協助我們進行設定，依序要回答六個問題：

```
This command will guide you through creating your composer.json config.

Package name (<vendor>/<name>) [oberonlai/my-plugin]: 
Description []: 
Author [Oberon Lai <m615926@gmail.com>, n to skip]: 
Minimum Stability []: stable
Package Type (e.g. library, project, metapackage, composer-plugin) []: project
License []: propreitary
```

基本上因為這不是要上傳到 Composer 套件庫 Packagist 的套件，我習慣都直接採用預設值，如果有特殊需求可以再個別調整，第七題是關鍵，它會問你是否要安裝相依套件：

```
Define your dependencies.

Would you like to define your dependencies (require) interactively [yes]? 
Search for a package: a7/autoload
Enter the version constraint to require (or leave blank to use the latest version): 
```

輸入套件名稱 a7/autoload 按下 Enter 後 Composer 就會開始自動搜尋 Packagist 上面的所有 PHP 套件並且指定安裝的版本，如果要安裝最新版就直接 Enter 下一步即可，[a7/autoload](https://github.com/a7/autoload) 這個套件可以把指定資料夾裡面的檔案全部自動載入，如果沒有它就需要一個一個手動指定要自動載入的資料夾。

完成後接下來又會再問一次是否要搜尋其他套件，這邊我們什麼都不輸入直接下 Enter 即可。

接下來它會詢問是否要安裝只有在開發時才需要的套件，像是做單元測試 PHPUnit 這一類只會在開發環境中需要的套件就可以在這邊安裝，但目前我們先跳過，之後講解單元測試的文章再另行說明：

```
Would you like to define your dev dependencies (require-dev) interactively [yes]? no
```

接下來就會看到整個 Composer 設定檔的草稿，它會請你確認是否正確，輸入 Enter 即可，這樣就完成 composer.json 設定檔了。

```
{
    "name": "oberonlai/my-plugin",
    "type": "project",
    "require": {
        "a7/autoload": "^2.1"
    },
    "license": "propreitary",
    "authors": [
        {
            "name": "Oberon Lai",
            "email": "m615926@gmail.com"
        }
    ],
    "minimum-stability": "stable"
}

Do you confirm generation [yes]? yes
```

最後它會問你是否要安裝設定檔中的套件，按下 Enter 即可

```
Would you like to install dependencies now [yes]? yes
```

最後就會出現安裝完成的訊息，這就代表我們已經把 Composer 已經所需套件準備完成：

```
No lock file found. Updating dependencies instead of installing from lock file. Use composer update over composer install if you do not have a lock file.
Loading composer repositories with package information
Updating dependencies
Lock file operations: 1 install, 0 updates, 0 removals
  - Locking a7/autoload (2.1)
Writing lock file
Installing dependencies from lock file (including require-dev)
Package operations: 1 install, 0 updates, 0 removals
  - Installing a7/autoload (2.1): Extracting archive
Generating autoload files
```

觀察我們外掛的目錄，會發現多了 composer.json、composer.lock 以及 vendor 資料夾，未來我們任何從 Composer 安裝的套件都會放在 vendor 這個資料夾底下，至於該如何引入這些套件以及讓 src 資料夾的檔案可以自動載入呢？讓我們開始寫 code 吧～

### 3.加入 Autoload 機制

打開外掛的主檔案，我把一些常用的寫法存成了 VSCode 的 Snippet，內容如下：

```php
<?php

 /**
 * @link              https://oberonlai.blog
 * @since             1.0.0
 * @package           ODS
 *
 * @wordpress-plugin
 * Plugin Name:       Oberon Plugin
 * Plugin URI:        https://oberonlai.blog
 * Description:       Oberon Lai Blog Demo Plugins
 * Version:           1.0.0
 * Author:            Oberon Lai
 * Author URI:        https://oberonlai.blog
 * License:           GPL-2.0+
 * License URI:       http://www.gnu.org/licenses/gpl-2.0.txt
 * Text Domain:       ods
 * Domain Path:       /languages
 */


if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

define('ODS_VERSION', '1.0.0');
define('ODS_PLUGIN_URL', plugin_dir_url(__FILE__));
define('ODS_PLUGIN_DIR', plugin_dir_path(__FILE__));
define('ODS_PLUGIN_BASENAME', plugin_basename(__FILE__));

/**
 * Autoload
 */
require_once( ODS_PLUGIN_DIR . 'vendor/autoload.php' );
\A7\autoload( ODS_PLUGIN_DIR . 'src' );

```

外掛 Header 頭幾行的註解是必要的，具體內容可以參考 Handbook，define 的常數定義我會先把幾個常用的路徑存起來，因為 plugin_dir_xxx 在不同層級的檔案下會對應到不同的路徑，直接在主檔案存起來才能正確對應到外掛的根目錄。

再往下看就是最關鍵的 Autoload，第一個 require_once 載入的是 vendor/autoload.php，也就是透過 Composer 安裝的套件只需要這一行就可以全部自動引入，使用時只需要透過 Namespace 的 use 關鍵字即可使用，

而下一行的 \\A7\\autoload 則是指定自動載入 src 目錄底下所有的檔案，不管有幾層目錄、多少檔案，透過它就可以自動完整載入，縱使修改了檔名或是資料夾名稱，依舊都能正常運作。

a7\\autoload 是由 Aaron Holbrook 所開發的，觀察他的程式碼，他使用了迴圈去掃描指定目錄的所有檔案，然後再透過 autoload 機制去逐一載入，雖然該套件已經有兩年多沒更新了，但用到目前為止都非常的穩定沒出過任何問題，因此它成為我每個專案的必備套件。

到這邊自動載入就完成了，日後如果我新增或修改任何 src 資料夾裡面的任何檔案名稱，就再也不用重新調整載入的路徑了，一勞永逸 🎉

### 4. Git 設定

為了要能做到版本控管與自動部署，Git 版控也是我每個必用的工具，早期看到朋友的作法是直接把整個 wp-content 資料夾都納入版控，好處是所有外掛的更新都可以從 git 來管理，於是我也採用這樣的作法，但經過幾個專案後發現有些問題：

首先是必須要把客戶整個站的 wp-content 都 clone 下來，遇到比較大型的站這對我 128GB 的 MBP 負擔很大...

其次在設定 .gitignore 時要處理比較多的部分，有時候也會不小心把需要 ignore 的東西給 commit 進去。另外如果要提供給客戶下載正在開發中的外掛時，直接 download 就會抓回一大包，客戶還要從一大包之中去找到正確的外掛。

所以我現在都是一支外掛做一個版控，好處是本機開發環境只要安裝可能需要的外掛即可，不用把整包 wp-content 跟資料庫載到自己的電腦，然後 gitignore 也比較好設定，主要我會把測試相關以及 vendor 資料夾忽略掉，

測試主要會在本機以及測試機執行，所以不宜把所有的套件都上傳到正式機，這部分需要透過自動化部署工具來決定哪些套件該裝哪些不用裝，這部分會在自動部署一文中提及。

另外的好處是在 Github 頁面上可以設定 Releases 功能來讓客戶下載指定版本，也能看到每個版本更新的項目。

但如果一個專案裡面需要開發多支外掛，或是有改過一些既有的外掛需要納入版控時，數量一多就會不好管理，於是我採用 Github 的 Organization 來進行分類。把同一個專案不同外掛的存放庫都集中管理。

在本機的部分就使用 VSCode 的工作區功能，把需要處理到的外掛都加進到同一個工作區，然後在 commit 的時候透過內建的 Git 工具各自處理。

### 5.WordPress Coding Standard

到這邊就已經完成了開發環境的基本設定，但在真正開始 coding 之前，有件事雖然不是強制但我還是逼著自己習慣，那就是 WordPress 程式碼規範。

PHP 有一套自己的程式碼規範，叫做 PHP Standards Recommendations ( PSR )，雖然 WordPress 是用 PHP 寫的，但在規範上有許多地方不一樣，雖然自己本身從來沒在照什麼規範寫程式，但自從知道有這東西後就還是覺得要嘗試看看，來建立自己寫程式的習慣。

因為我都是在寫 WordPress，所以就理所當然的照著 WP 的規範走，一開始很不習慣，到現在老實說也還是很不自在，常常光是要整理格式就打斷了程式邏輯的思考，所以整理規範這件事我都是放在重構的時候來做，第一次在寫的時候就不管它了XD

後來透過 Terry Lin 大大的推薦知道 VSCode 有個好工具叫做 [PHP Sniffer](https://marketplace.visualstudio.com/items?itemName=wongjn.php-sniffer)，可以選擇要遵循的規範並且在編輯器中即時顯示不符合規範之處，另外還可以搭配快速鍵進行修正，可以搞定我最討厭的變數對齊：

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-boilerplate/wordpress-plugin-boilerplate-03.jpg)

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-boilerplate/wordpress-plugin-boilerplate-04.jpg)

自動修正可以修到七八成，剩下的只要補上 PHPDoc 的說明即可。

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-boilerplate/wordpress-plugin-boilerplate-01.jpg)

安裝 PHP Sniffer 記得要在它的設定裡面將 PHPCS Standard 改成 WordPress，這樣就會以 WordPress 的程式碼規範來檢查，更多這部分的說明可以參考[使用 PHP CodeSniffer 幫助熟悉 WordPress 程式碼風格](https://ithelp.ithome.com.tw/articles/10244668?sc=hot)。

安裝與設定完成後，就會看到有不符合規範的地方會在下面出現紅色波浪線，滑鼠停留在上面一下下就會出現規範說明，然後根據說明進行修改即可。

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-boilerplate/wordpress-plugin-boilerplate-02.jpg)

這邊補充一下，有時候會遇到無法符合規範的狀況，像是要使用其他外掛的變數名稱，而這外掛並沒有依照規範來寫，另外一種狀況是 WordPress 規範中類別的檔名要用 class-xxx.php，有 template tag 的要用 xxx-template.php 這樣，

但在研究 PHPUnit 單元測試的時候發現這樣無法運作，原因是 PHPUnit 測試檔要載入被測試的類別時，必須要用 PSR-4 的機制來載入，然後 PSR-4 規定類別叫什麼檔名就要叫什麼，

如果類別叫做 User，則檔名就一定要叫做 User.php，寫 class-User.php 會無法載入，我測試過用 require 或 include 也無法正確載入，最後只能忽略某些 WordPress 程式碼規範。

忽略的常用方法有兩種，分為忽略當行或多行，單行的話直接在該行後面加入註解，並把規則名稱寫在註解中即可

```php
$AbCd = 'bbb'; //phpcs:ignore WordPress.NamingConventions.ValidVariableName.VariableNotSnakeCase
```

WordPress 程式碼規範規定變數名稱必須要用底線而非駝峰式寫法，這時候直接在後面接上 //phpcs:ignore 加規則名稱，規則名稱可以在 PHP Sniffer 跳出的提示視窗中進行複製

![](https://oberonlai.blog/wp-content/uploads/wordpress-plugin-boilerplate/wordpress-plugin-boilerplate-05.jpg)

如果一次要忽略多行就更簡單了，只要用兩個註解把要忽略的部分包住即可：

```php
// @codingStandardsIgnoreStart
$AbCd = 'bbb';
$ACaaa = 'abcdefg';
// @codingStandardsIgnoreEnd
```

## 小結

在開發 WordPress 外掛時整合 Composer 可以很方便的安裝一些實用的套件以及自由組織資料夾內容，這樣在程式重構上能達到最大的彈性，另一方面，透過 Coding Standard 養成程式撰寫的習慣可以幫未來的自己或是團隊成員理解程式碼，接下來我們繼續挖掘 Composer 有什麼好物可以用吧～