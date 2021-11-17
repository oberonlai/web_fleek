---
title: WooCommerce 金流串接實戰（三）- 建立資料夾結構
date: 2021-09-18 09:30:00
categories:
- WooCommerce
tags:
- WooCommerce 金流
- WordPress 外掛
- WooCommerce 外掛
etoc: false
---

使用 Valet 或是其他本機環境軟體把 WordPress 安裝好之後，切換到網站根目錄，可以看到 以下的檔案結構：

```
├── index.php
├── license.txt
├── readme.html
├── wp-activate.php
├── wp-admin
├── wp-blog-header.php
├── wp-comments-post.php
├── wp-config-sample.php
├── wp-config.php
├── wp-content
├── wp-cron.php
├── wp-includes
├── wp-links-opml.php
├── wp-load.php
├── wp-login.php
├── wp-mail.php
├── wp-settings.php
├── wp-signup.php
├── wp-trackback.php
└── xmlrpc.php
```

一共有三個資料，分別是 wp-admin、wp-content 與 wp-includes，其中 wp-admin 是存放後台相關的檔案，wp-includes 是提供所有開發時會用到的 API，關於這兩個資料夾我們必須要記住的唯一一個重點就是：千萬絕對不可不要去修改裡面的檔案，因為他們是 WordPress 的核心程式，只要一更新就會被複寫，如果你在網路上爬到一些程式碼片段是要進去這兩個資料夾修改的，請直接忽略跳過。

> 絕對不要修改 wp-admin 與 wp-includes 裡面的檔案

根目錄中我們唯一可以自行修改的檔案是 wp-config.php，裡面包含了資料庫連線資訊、驗證用的金鑰，以及可以控制環境的常數設定，如果需要啟用除錯模式、設定資料表前綴，都是在 wp-config.php 裡面處理。

接下來我們的主戰場是在 wp-content，結構說明如下：

<!--more-->

```
wp-content
├── languages
├── plugins
├── themes
└── uploads
```

**languages** : 放佈景主題或是外掛的語系檔，只會有 .po 與 .mo 兩種檔案類型

**plugins** : 放外掛的資料夾，如果不透過後台而要手動安裝的話，把外掛資料夾放到這個目錄即可

**themes** 放佈景主題的資料夾，同樣的，直接將佈景主題資料夾放到這個目錄就能在後台看到相對應的佈景主題

**uploads** 所有透過後台上傳的檔案、圖片都會放在這個目錄中

## 新增外掛

我們現在要來開發外掛，所以先將目錄切換至 plugins，並且新增一個名為 iron-pay 的資料夾：

```sh
$ cd Sites/woocommerce/wp-content/plugins
~/Sites/woocommerce/wp-content/plugins $ mkdir iron-pay
```

這時候我們就可以看到 plugins 目錄裡面有兩個資料夾：

```
plugins
├── iron-pay
└── woocommerce
```

開發 WordPress 外掛與 WooCommerce 外掛基本上是一模一樣的，因為 WordPress 的機制讓我們可以開發外掛的外掛，只要該外掛本身有提供勾點 ( Hook )，就能在不動到原始程式碼的情境下進行修改，WooCommerce 提供非常豐富且數量龐大的勾點，因此我們可以不用修改 WooCommerce 的原始碼就能新增金流功能。

要寫一支外掛很簡單，開好資料夾並切換到該目錄後新增一個與資料夾同名的檔案：

```sh
~/Sites/woocommerce/wp-content/plugins $ cd iron-pay
~/Sites/woocommerce/wp-content/plugins/iron-pay $ touch iron-pay.php
```

開啟 iron-pay.php 將以下註解放入檔案最上方：

```php
<?php

/**
 * @link              https://oberonlai.blog
 * @since             1.0.0
 * @package           irp
 *
 * @wordpress-plugin
 * Plugin Name:       WooCommerce 鐵人付
 * Plugin URI:        https://oberonlai.blog
 * Description:       新增 WooCommerce 鐵人支付金流
 * Version:           1.0.0
 * Author:            Oberon Lai
 * Author URI:        https://oberonlai.blog
 * License:           GPL-2.0+
 * License URI:       http://www.gnu.org/licenses/gpl-2.0.txt
 * Text Domain:       irp
 * Domain Path:       /languages
 * WC tested up to: 5.6.0
 * WC requires at least: 5.3
 */
```

需要注意的地方是 Plugin Name 與 Description，這是外掛名稱與描述，WC tested up to 是 WooCommerce 的版本相容性，WC requires at least 是該站的 WooCommerce 最低版本，存檔後就可以在 WordPress 後台的外掛清單看到這支外掛並啟用它。

接下來加入一些開發外掛時常會用到的功能：

### 判斷 WooCommerce 是否有啟用

因為我們的金流外掛是依附在 WooCommerce 上面的，如果它沒有啟用的話可能會造成某些預期外的錯誤，因此先做啟用判斷會比較保險，繼續在 iron-pay.php 中加入以下程式碼：

```php
<?php

/**
 * @link              https://oberonlai.blog
 * @since             1.0.0
 * @package           irp
 *
 * @wordpress-plugin
 * Plugin Name:       WooCommerce 鐵人付外掛
 * Plugin URI:        https://oberonlai.blog
 * Description:       新增 WooCommerce 鐵人支付金流
 * Version:           1.0.0
 * Author:            Oberon Lai
 * Author URI:        https://oberonlai.blog
 * License:           GPL-2.0+
 * License URI:       http://www.gnu.org/licenses/gpl-2.0.txt
 * Text Domain:       irp
 * Domain Path:       /languages
 * WC tested up to: 5.6.0
 * WC requires at least: 5.3
 */
	
/**
 * Check WooCommerce Activied
 */
if ( ! in_array( 'woocommerce/woocommerce.php', apply_filters( 'active_plugins', get_option( 'active_plugins' ) ), true ) ) {
    
	require_once ABSPATH . 'wp-admin/includes/plugin.php';
    
	if ( is_plugin_active( plugin_basename( __FILE__ ) ) ) {
        deactivate_plugins( plugin_basename( __FILE__ ) );
        /**
         * Error admin notice
         */
        function require_woocommerce_notice() {
            echo '<div class="error"><p>外掛啟用失敗，需要安裝並啟用 WooCommerce 5.3 以上版本。</p></div>';
        }
        add_action( 'admin_notices', 'require_woocommerce_notice' );
        return;
    }
}
```

第一行先判斷在已啟用的外掛列表中，是否含有 woocommerce/woocommerce.php 這筆資料，get_option() 是取得設定值的函式，apply_filters 是放入勾點的函式，這邊代表的是如果有其他的程式使用了 ```add_filter( 'active_plugins' )``` 這個勾點來修改已啟用的外掛清單，那麼也會被列入 in_array 的判斷之中。

接下來我們因為要使用 is_plugin_active  所以要先引入 wp-admin/includes/plugin.php，這個函式會判斷我們的外掛是否被啟用，它傳入的參數是外掛的主檔案，也就是 iron-pay.php，所以我們先判斷 WooCommerce 是否有被啟用，如果沒有的話而我們的外掛被啟用了，就會觸發 ```deactivate_plugins()``` 停用外掛以及出現錯誤提示。

```add_action()``` 是另外一種勾點，這邊帶入兩個參數，第一個是勾點的名稱 admin_notices，也就是後台的提示訊息，第二個是回呼函式，也就是我們要在這個勾點上做什麼事情。

函式 ```require_woocommerce_notice()``` 裡面輸出提示訊息，提示說要先啟用 WooCommerce 才能啟用我們的外掛。這段程式碼我們帶到了 WordPress 的核心概念勾點，勾點有兩種：action 與 filter。

action 是可以把一些特定的函式交由勾點來執行，就像 ```add_action( 'admin_notices', 'require_woocommerce_notice' )``` 這樣。Filter 是可以修改參數、顯示文字或設定值，就像```add_filter( 'active_plugins', 'new_plugin_active' )``` 這樣，action 的回呼函式不一定要輸出結果，而 filter 因為修改資料所以會需要 return 回傳修改後的結果。

勾點可以放在佈景主題或是外掛來執行，這樣就不用修改到核心程式碼。如果現在還不了解勾點是什麼也沒關係，後續關於勾點的部分會在外掛實作中一直反覆出現，

### 常數定義與自動載入

當判斷我們的外掛可以正確被啟用後，接下來就先把之後會常用到的常數定義出來：

```php
<?php

/**
 * @link              https://oberonlai.blog
 * @since             1.0.0
 * @package           irp
 *
 * @wordpress-plugin
 * Plugin Name:       WooCommerce 鐵人付外掛
 * Plugin URI:        https://oberonlai.blog
 * Description:       新增 WooCommerce 鐵人支付金流
 * Version:           1.0.0
 * Author:            Oberon Lai
 * Author URI:        https://oberonlai.blog
 * License:           GPL-2.0+
 * License URI:       http://www.gnu.org/licenses/gpl-2.0.txt
 * Text Domain:       irp
 * Domain Path:       /languages
 * WC tested up to: 5.6.0
 * WC requires at least: 5
 */
	
/**
 * Check WooCommerce Activied
 * 略
 */
	
/**
 * Define Variable
 */
define( 'IRP_PLUGIN_VERSION', '1.0.0' );
define( 'IRP_PLUGIN_URL', plugin_dir_url( __FILE__ ) );
define( 'IRP_PLUGIN_DIR', plugin_dir_path( __FILE__ ) );
define( 'IRP_PLUGIN_BASENAME', plugin_basename( __FILE__ ) );

/**
 * Autoload
 */
require_once( IRP_PLUGIN_DIR . 'vendor/autoload.php' );
\A7\autoload( IRP_PLUGIN_DIR . 'src' );
```

這四個常數經常會用到，像是引入 js 或 css 檔，就可以用這些常數來指定路徑與版本號，Autoload 的部分使用了由 Aaron Holbrook 所開發的 a7/autoload，他使用了迴圈去掃指定目錄的所有檔案，然後再透過 autoload 機制去逐一載入，雖然該套件已經有兩年多沒更新了，但用到目前為止都非常的穩定沒出過任何問題。

用了它在 src 目錄底下所有的檔案，不管有幾層目錄、多少檔案，透過它就可以自動完整載入，縱使修改了檔名或是資料夾名稱，依舊都能正常運作，非常適合需要頻繁變動的資料夾結構。

下一步這我們就用 Composer 來進行安裝。


### 設定 Composer 環境

安裝 Composer 的教學網路非常多，這邊主要示範給 WordPress 外掛開發者的環境設定。第一步先開啟終端機然後把目錄切到正在開發的外掛底下然後執行 composer init：

```sh
~/Sites/woocommerce/wp-content/plugins/iron-pay $ composer init
```

composer 會用對話式的介面來協助我們進行設定，依序要回答六個問題：

```sh
This command will guide you through creating your composer.json config.

Package name (<vendor>/<name>) [oberonlai/my-plugin]: 
Description []: 
Author [Oberon Lai <m615926@gmail.com>, n to skip]: 
Minimum Stability []: stable
Package Type (e.g. library, project, metapackage, composer-plugin) []: project
License []: propreitary
```

基本上因為這不是要上傳到 Composer 套件庫 Packagist 的套件，我習慣都直接採用預設值，如果有特殊需求可以再個別調整，第七題是關鍵，它會問你是否要安裝相依套件：

```sh
Define your dependencies.

Would you like to define your dependencies (require) interactively [yes]? 
Search for a package: a7/autoload
Enter the version constraint to require (or leave blank to use the latest version): 
```

輸入套件名稱 a7/autoload 按下 Enter 後 Composer 就會開始自動搜尋 Packagist 上面的所有 PHP 套件並且指定安裝的版本，如果要安裝最新版就直接 Enter 下一步即可，[a7/autoload](https://github.com/a7/autoload) 這個套件可以把指定資料夾裡面的檔案全部自動載入，如果沒有它就需要一個一個手動指定要自動載入的資料夾。

完成後接下來又會再問一次是否要搜尋其他套件，這邊我們什麼都不輸入直接下 Enter 即可。

接下來它會詢問是否要安裝只有在開發時才需要的套件，像是做單元測試 PHPUnit 這一類只會在開發環境中需要的套件就可以在這邊安裝，但目前我們先跳過，之後講解單元測試的文章再另行說明：

```sh
Would you like to define your dev dependencies (require-dev) interactively [yes]? no
```

接下來就會看到整個 Composer 設定檔的草稿，它會請你確認是否正確，輸入 Enter 即可，這樣就完成 composer.json 設定檔了。

```sh
{
    "name": "oberonlai/irpon-pay",
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

```sh
Would you like to install dependencies now [yes]? yes
```

最後就會出現安裝完成的訊息，這就代表我們已經把 Composer 已經所需套件準備完成：

```sh
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

觀察我們外掛的目錄，會發現多了 composer.json、composer.lock 以及 vendor 資料夾，未來我們任何從 Composer 安裝的套件都會放在 vendor 這個資料夾底下。

完成 Composer 的設定後我們再新增一個 src 資料夾，之後需要自動載入的類別都會放在這個目錄下：

```sh
~/Sites/woocommerce/wp-content/plugins/iron-pay $ mkdir src
```

到這邊，外掛的基本結構就完成了：

```
iron-pay
├── composer.json
├── composer.lock
├── iron-pay.php
├── src
└── vendor
    ├── a7
    ├── autoload.php
    └── composer
```

接下來我們開始來進行設定頁面的實作。