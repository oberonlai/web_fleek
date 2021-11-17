---
title: 【 筆記 】WooCommerce 客製化報表自動匯出&上傳功能開發
tags:
  - csv
  - ftp
  - WooCoomerce訂單
  - wp-cron
  - 報表匯出
id: '3547'
categories:
  - - WooCommerce
date: 2020-07-09 21:44:50
etoc: true
---

WooCoomerce 有許多方便又實用的外掛，可以解決企業在經營電子商務時所遇到的各種問題，但如果要與企業內部現有的系統進行整合，就勢必要針對既有系統的規格來進行 WooCommerce 的客製化開發。

## 需求分析

客戶希望把 WooCommerce 訂單能於每日特定的時段自動匯出後整合至內部的管理系統，並當發生訂單取消或退貨時，也能把該筆訂單的資料傳送至該系統中。

既有的操作流程為人工使用外掛匯出 csv 後，手動調整其報表內容傳送至內部系統，問題點在於無法自動化與取得某些類型商品的價格，以及折扣金額的計算皆需要人為介入處理，因此需要設計一套自動化流程來解決這些繁瑣的任務，並減少人工整理報表時可能產生的問題。

完整流程如下：

<!--more-->

一、訂單新增系統傳送狀態欄位，用來紀錄該訂單是否已被傳送至企業內部系統

二、後台可以篩選尚未傳送的訂單，勾選後使用批次動作來進行報表的匯出

三、訂單狀態變更為退費時自動匯出報表

四、用任何方式匯出的報表皆透過 FTP 傳到客戶的伺服器中

五、每天於特定時段把尚未傳送的訂單自動進行傳送

## 任務拆解

理解了業務邏輯之後，接下來就是把每個流程轉變為具體的開發步驟：

ㄧ、WooCommerce 後台訂單介面新增欄位、欄位篩選下拉選單、批次編輯選項

二、WooCommerce 訂單資料取得

三、將取得的 WooCommerce 訂單資料組成 csv 檔，並寫入到 wp-content/uploads 資料夾

四、FTP 上傳功能開發

五、排程設定

確認任務清單後，開始設計程式結構：

**ods-wc-order-export/ods-wc-order-export.php** - 外掛主要程式，用來設定常數與類別檔案引入

**ods-wc-order-export/js** - 放後台可能會需要用到 Ajax 的 JS 檔

**ods-wc-order-export/class/class-ods-hook.php** - 負責所有功能所需 Hook 掛載的類別

****ods-wc-order-export/class/class-ods-wc-admin.php**** - 負責新增 WooCommerce 後台訂單介面的類別

****ods-wc-order-export/class/class-ods-wc-order.php**** - 負責取得 WooCommerce 訂單資料的類別

****ods-wc-order-export/class/class-ods-csv.php**** - 負責組成與匯出 csv 的類別

****ods-wc-order-export/class/class-ods-ftp.php**** - 負責 FTP 上傳的類別

****ods-wc-order-export/class/class-ods-helper.php**** - 負責工具類 class

## Let's Party Start!

首先把可能未來會需要修改的資料定義為常數，讓接手的工程師方便些，如果更新頻率高且操作人員非工程師的話，建議還是把它做成後台的設定頁面會更友善些，定義的資料如下：

****ods-wc-order-export/ods-wc-order-export.php****

```php
<?php

/**
 * Plugin Name:       ODS WooCommerce Order Export
 * Plugin URI:        https://oberonlai.blog
 * Description:       WooCommerce 客製化報表自動匯出&上傳功能
 * Version:           1.0.1
 * Author:            Oberon Lai
 * Author URI:        https://oberonlai.blog
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

// WC 訂單傳送狀態欄位名稱
const ODS_WC_ORDER_EXPORT_META_KEY = 'export_status';             

// WC 訂單傳送狀態欄位值
const ODS_WC_ORDER_EXPORT_META_VALUE = array('未傳送','已傳送','取消傳送');

// FTP 上傳資訊
const ODS_WC_ORDER_EXPORT_TO_FTP = array(
  'enable'  => false, // 啟用/停用
  'host'    => '',
  'user'    => '',
  'pass'    => '',
  'path'    => 'wp-content/uploads/',
);

// 引入主要類別檔
require plugin_dir_path( __FILE__ ) . 'class/class-ods-hook.php';

```

ODS_WC_ORDER_EXPORT_META_KEY 為 Post Meta 的名稱，可以接受的值為 ODS_WC_ORDER_EXPORT_META_VALUE，我們會需要用到這裡面的值來判斷該訂單應該要做什麼樣的任務。

FTP 上傳資訊也是定義在這邊，可以設定是否要進行上傳以及上傳路徑，最後則是引入負責 Hook 的 class-ods-hook.php。如果未來有新的常數要定義都可以來這邊做新增。

接下來我們把鏡頭轉到 class-ods-hook.php，這個檔案負責三個任務，首先是引入需要用到的 class 檔，其次是把 WC 訂單傳送狀態欄位寫入，但也許網站已經經營一陣子了，所以會有既存的訂單資料，我們需要把傳送狀態欄位也寫入這些訂單。

最後是所有的 Hook，因為 Hook 會邊寫邊加，所以先把我們要做的任務先以註解的方式來標記，就能把整支外掛需要完成的功能做總覽，以便追蹤還有什麼功能沒有完成。

**ods-wc-order-export/class/class-ods-hook.php**

```php
<?php

/**
 * 1.引入 class
 * 2.訂單新增系統傳送狀態欄位，用來紀錄該訂單是否已被傳送至企業內部系統
 * 3.處理 Hooks
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

require plugin_dir_path( __FILE__ ) . 'class-ods-wc-admin.php';
require plugin_dir_path( __FILE__ ) . 'class-ods-wc-order.php';
require plugin_dir_path( __FILE__ ) . 'class-ods-csv.php';
require plugin_dir_path( __FILE__ ) . 'class-ods-ftp.php';

if(!class_exists( 'ODS_Hook' )){
  
  class ODS_Hook {
    
    private $plugin_version;

    function __construct( $wc_admin ){

      // 第一次啟用外掛時加入所有 WC 訂單的傳送狀態欄位
      $this->plugin_version = get_option('ods_wc_order_export');
      if( $this->plugin_version == '' ){
        update_option( 'ods_wc_order_export', 'enable' );
        add_action('init',array($wc_admin,'set_order_field'),99,'9999,1');
      }
      
      // 新訂單加入系統傳送狀態的 Post Meta
      if( get_option('ods_wc_order_export') === 'enable' ){
        add_action('woocommerce_new_order',array($wc_admin,'set_order_field'),10,'1,1');
      }
    }
  }
  
  $erp = new ODS_Hook(
    new ODS_WC_Admin()
  );
}

```

首先建立一個私有變數叫做 $plugin_version，用來紀錄該外掛是否是第一次啟用，如果是第一次的話，會在 wp_options 資料表裡面建立一個叫做 ods_wc_order_export 的欄位，其值為 enable，如果沒有欄位就代表是第一次啟用，然後在初始化的時候執行 set_order_field 這個方法。

set_order_field 可以帶一個參數，設定需要寫入的訂單筆數，在 add_action 最後一個參數 '9999,1'，這代表的是 set_order_field(9999)，然後因為只需帶一個參數所以第二個數字為 1。在第一次啟用外掛的狀況下可以帶入需要寫入系統傳送狀態的訂單筆數，之後新訂單只要抓最新的一筆就好，所以第 35 行最後的參數為 '1,1'。

set_order_field 實作細節在 class-ods-wc-admin.php：

****ods-wc-order-export/class/class-ods-wc-admin.php****

```php
<?php

/**
 * 處理後台系統傳送欄位註冊、介面顯示
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

if(!class_exists('ODS_WC_Admin')){
  
  class ODS_WC_Admin {
    /**
     * 新增訂單的系統傳送狀態欄位
     */
    public function set_order_field( $num ){
      $args = array(
        'numberposts' => $num
      );
      $wc_orders = wc_get_orders( $args );
      if(!empty($wc_orders)){
        foreach ($wc_orders as $order) {
          if ( !metadata_exists( 'post', $order->get_id(), ODS_WC_ORDER_EXPORT_META_KEY ) ) {
            update_post_meta( $order->get_id(), ODS_WC_ORDER_EXPORT_META_KEY, ODS_WC_ORDER_EXPORT_META_VALUE\[0\] );
          }
        }
      }
    }

  }
}
```

set_order_field 主要是使用 wc_get_orders 來取得 WooCommerce 訂單，取得後先確認該訂單是否已經帶有系統傳送狀態的 Post Meta，沒有的話再利用 update_post_meta 進行狀態寫入的動作，如果需要篩選特定類型的訂單可以參考 WooCommerce 的 Github 頁面 --> [https://github.com/woocommerce/woocommerce/wiki/wc_get_orders-and-WC_Order_Query#description](https://github.com/woocommerce/woocommerce/wiki/wc_get_orders-and-WC_Order_Query#description)

## WooCommerce 後台訂單介面

把基底搭建好了之後，我們先從後台介面來進行開發，根據需求我們新增以下三種功能：

一、訂單列表頁的表格增加系統傳送狀態的欄位

二、加入篩選功能，可以使用系統傳送狀態來篩選訂單

三、勾選訂單後的批次操作處理

我們會先把 Hook 定義好後再來發展細節，首先是增加訂單列表的表格欄位，一樣是編輯 class-ods-hook.php，在 function __construct 建構式裡面加入以下 Hook：

### 一、訂單列表頁的表格增加系統傳送狀態的欄位

**ods-wc-order-export/class/class-ods-hook.php**

```php
<?php

...

if(!class_exists('ODS_Hook')){
  
  class ODS_Hook {
    
    private $plugin_version;

    function __construct( $wc_admin ){

      ...

      // WC 訂單列表頁表格加入標頭
      add_filter('manage_shop_order_posts_columns', array($wc_admin,'set_admin_th'),99);

      // WC 訂單列表頁表格加入欄位
      add_action('manage_shop_order_posts_custom_column' , array($wc_admin,'set_admin_columns'), 10, 2 );

    }
  }
  
  $erp = new ODS_Hook(
    new ODS_WC_Admin(),
  );
}
```

manage_shop_order_posts_columns 與 manage_shop_order_posts_custom_column 是可以動態帶入 post type 的 Hook，寫法為 manage_{$post_type}_posts_columns，範例中 WooCommerce 訂單的 post type 為 shop_order，因此帶入 {$post_type} 即可，而該欄位只有在對應的 post type 下才會顯示。

set_admin_th 與 set_admin_columns 實作在 class-ods-wc-admin.php：

****ods-wc-order-export/class/class-ods-wc-admin.php****

```php
<?php

/**
 * 處理後台系統傳送欄位註冊、介面顯示
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

if(!class_exists('ODS_WC_Admin')){
  
  class ODS_WC_Admin {
   
    ...
   
   /**
     * 註冊訂單列表系統傳送狀態顯示的標頭
     * 
     * @param array $columns 現有欄位
     * 
     * @return array $columns 新增欄位
     */
    public function set_admin_th( $columns ){
      $columns\['send_status'\] = '系統傳送狀態';    
      return $columns;
    }
    
    /**
     * 註冊訂單列表的系統傳送狀態顯示
     * 
     * @param string $column_name 欄位名稱
     * @param int $post_id 訂單 ID
     * 
     * @return string $send_status 訂單系統傳送狀態顯示文字
     */
    public function set_admin_columns( $column_name, $post_id ){
      switch ( $column_name ) {
        case 'send_status':
          $order = new WC_Order( $post_id );
          echo $send_status = $order->get_meta(ODS_WC_ORDER_EXPORT_META_KEY);
          break;
      }
    }  
    
  }
}
```

set_admin_th 新增欄位叫做 send_status，賦予的值也就是在後台看到的標頭名稱叫做「系統傳送狀態」，set_admin_columns 則用 switch 判斷當欄位名稱叫做 send_status 的時候，去取得該 post 的 post meta，就能把資料顯示出來，完成後如下圖：

![](https://oberonlai.blog/wp-content/uploads/2020/07/CleanShot-2020-07-08-at-11.14.48.jpg)

其次是加入篩選功能，並將系統傳送狀態的值加入到搜尋結果，這樣就能提供後台操作人員快速篩選出需要的訂單，以便後續的訂單處理，先回到 class-ods-hook.php 來加入繼續 Hook：

### 二、加入篩選功能，可以使用系統傳送狀態來篩選訂單

**ods-wc-order-export/class/class-ods-hook.php**

```php
<?php

...

if(!class_exists('ODS_Hook')){
  
  class ODS_Hook {
    
    private $plugin_version;

    function __construct( $wc_admin ){

      ...

      // 訂單搜尋功能加入系統傳送狀態關鍵字索引
      add_filter( 'woocommerce_shop_order_search_fields', array($wc_admin,'set_status_seach'),100 );

    }
  }
  
  $erp = new ODS_Hook(
    new ODS_WC_Admin(),
  );
}
```

負責搜尋功能索引的 filter 是 woocommerce_shop_order_search_fields，set_status_seach 實作細節在 class-ods-wc-admin.php

****ods-wc-order-export/class/class-ods-wc-admin.php****

```php
<?php

/**
 * 處理後台系統傳送欄位註冊、介面顯示
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

if(!class_exists('ODS_WC_Admin')){
  
  class ODS_WC_Admin {
   
    ...
   
   /**
     * 訂單列表加入可搜尋系統傳送狀態關鍵字功能
     * 
     * @param array $serach_fields 搜尋欄位
     * 
     * @return array $search_fields 加入系統傳送搜尋狀態的欄位
     */
    public function set_status_seach($search_fields){
      $search_fields\[\] = ODS_WC_ORDER_EXPORT_META_KEY;
      return $search_fields;
    }
    
  }
}
```

把 post meta 的 Key 存進 $search_fields 陣列，就能如下圖透過搜尋功能進行搜尋：

![](https://oberonlai.blog/wp-content/uploads/2020/07/CleanShot-2020-07-08-at-11.27.22.jpg)

接下來回到 class-ods-hook.php 來處理篩選下拉選單：

**ods-wc-order-export/class/class-ods-hook.php**

```php
<?php

...

if(!class_exists('ODS_Hook')){
  
  class ODS_Hook {
    
    private $plugin_version;

    function __construct( $wc_admin ){

      ...

     // 訂單篩選下拉選單
      add_action( 'restrict_manage_posts',  array($wc_admin,'set_status_filter'),20 );
      add_filter( 'parse_query', array($wc_admin,'get_status_filter_result'),99,1 );

    }
  }
  
  $erp = new ODS_Hook(
    new ODS_WC_Admin(),
  );
}
```

第一個 action restrict_manager_posts 為篩選訂單列表的顯示狀態，第二個 filter parse_query 為處理網址帶入的篩選參數，當 parese_query 傳送參數後，WordPress 會透過 restrict_manager_posts 來取得符合條件的資料，set_status_filter 與 get_status_filter_result 實作細節一樣在 class-ods-wc-admin.php

****ods-wc-order-export/class/class-ods-wc-admin.php****

```php
<?php

/**
 * 處理後台系統傳送欄位註冊、介面顯示
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

if(!class_exists('ODS_WC_Admin')){
  
  class ODS_WC_Admin {
   
    ...
   
   /**
     * 訂單列表加入系統傳送狀態查詢下拉選單
     */
    public function set_status_filter(){
      global $typenow;
      global $wp_query;
        if ( $typenow == 'shop_order' ) { 
          $plugins = ODS_WC_ORDER_EXPORT_META_VALUE;
          $current_plugin = '';
          if( isset( $_GET\['send_status'\] ) ) {
            $current_plugin = $_GET\['send_status'\];
          } ?>
          <select name="send_status" id="send_status" style="width: 200px;">
            <option value="all" <?php selected( 'all', $current_plugin ); ?>>系統傳送狀態（所有）</option>
            <?php foreach( $plugins as $key ) { ?>
              <option value="<?php echo esc_attr( $key ); ?>" <?php selected( $key, $current_plugin ); ?>><?php echo esc_attr( $key ); ?></option>
            <?php } ?>
          </select>
      <?php }
    }
    
    /**
     * 訂單列表取得系統傳送狀態查詢結果
     */
    public function get_status_filter_result( $query ){
      global $pagenow;
      $post_type = isset( $_GET\['post_type'\] ) ? $_GET\['post_type'\] : '';
      if ( is_admin() && $pagenow=='edit.php' && $post_type == 'shop_order' && isset( $_GET\['send_status'\] ) && $_GET\['send_status'\] !='all' ) {
        $query->query_vars\['post_status'\] = 'all';
        $query->query_vars\['meta_key'\] = ODS_WC_ORDER_EXPORT_META_KEY;
        $query->query_vars\['meta_value'\] = $_GET\['send_status'\];
        $query->query_vars\['meta_compare'\] = '=';
      }
    }
    
  }
}
```

set_status_filter 增加一組下拉選單，選項內容為系統傳送狀態的文字，並將選取到的值透過 $_GET 方式進行傳送，get_status_filter_result 則處理資料顯示，根據從 set_status_filter 傳出來的 $_GET\['send_status'\] 來進行比對，比對方式為完全符合，參考第 48 行 $query->query_vars\['meta_compare'\] = '=' 。

完成後的介面如下：

![](https://oberonlai.blog/wp-content/uploads/2020/07/CleanShot-2020-07-08-at-11.40.58.jpg)

![](https://oberonlai.blog/wp-content/uploads/2020/07/CleanShot-2020-07-08-at-11.41.16.jpg)

### 三、勾選訂單後的批次操作處理

後台介面最後要處理的動作，就是當使用批次操作時可以改變系統傳送狀態，我們需要先新增操作的動作，然後根據選到的動作來進行相對應的處理方式，一樣把需要的 Hook 先掛好：

**ods-wc-order-export/class/class-ods-hook.php**
```php
<?php

...

if(!class_exists('ODS_Hook')){
  
  class ODS_Hook {
    
    private $plugin_version;

    function __construct( $wc_admin, $csv ){

      ...

      // 訂單批次操作加入新動作
      add_filter( 'bulk_actions-edit-shop_order', array($csv,'set_send_custom_bulk_actions'),10,1 );
      add_filter( 'handle_bulk_actions-edit-shop_order', array($csv,'set_send_custom_bulk_actions_handler'), 10, 3 );
      add_action( 'admin_notices', array($csv,'set_send_custom_bulk_actions_notice'),20 );

    }
  }
  
  $erp = new ODS_Hook(
    new ODS_WC_Admin(),
    new ODS_CSV()
  );
}
```

因為批次操作會關係到 csv 的產出作業，所以我們用了新類別 ODS_CSV，並宣告物件 $csv，bulk_actions-edit-shop_order 是加入新的批次操作動作，handle_bulk_actions-edit-shop_order 是處理對應的動作，這兩個 filter 最後帶的都是 post type，所以可以根據需要動態置換 Hook 名稱。

admin_notices 是批次操作結束後所顯示的提示訊息，用來告知管理員處理狀態，稍後我們也會做訂傳送狀態的驗證判斷，像是要把訂單變成已傳送必須是未傳送的訂單，透過 admin_notices 就可以提醒管理員為何變更失敗或成功。

接下來的實作細節在 class-ods-csv.php

**ods-wc-order-export/class/class-ods-csv.php**

```php
<?php

/**
 * 1.設定批次操作要執行的動作
 * 2.組合 CSV 欄位
 * 3.執行 FTP 上傳
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

if(!class_exists('ODS_CSV')){
  class ODS_CSV {

     /**
     * 訂單列表加入系統傳送狀態批次操作項目
     * 
     * @param array $bulk_actions 現有操作項目
     * 
     * @return array $bulk_actions 新增操作項目
     */
    public function set_send_custom_bulk_actions( $bulk_actions ){
      $bulk_actions\['send_to_cancel'\] = '將系統傳送狀態變更為'.ODS_WC_ORDER_EXPORT_META_VALUE\[1\];
      $bulk_actions\['send_to_delivered'\] = '將系統傳送狀態變更為'.ODS_WC_ORDER_EXPORT_META_VALUE\[2\];
      return $bulk_actions;
    }

    /**
     * 訂單列表系統傳送批次操作項目點選後要做的事
     * 
     * @param string $redirect_to 跳轉頁面
     * @param string $doaction 操作項目代稱
     * @param array $post_ids 被勾選的訂單 ID
     * 
     * @return string $redirect_to 操作完成後跳轉的頁面
     */
    public function set_send_custom_bulk_actions_handler( $redirect_to, $doaction, $post_ids){
      switch ($doaction) {
        case 'send_to_cancel':
          // 變為已傳送要做的事
          break;
        case 'send_to_delivered':
          // 變為取消傳送要做的事
          break;
        default:
          # code...
          break;
      }
      return $redirect_to;
     
    }

    /**
     * 訂單列表顯示系統傳送狀態變更完成後的訊息
     */
    public function set_send_custom_bulk_actions_notice(){ ?>
      <?php if ( ! empty( $_GET\['send_status_text'\] ) && ! empty( $_GET\['send_status_changed'\] ) ): ?>
        <?php if( 'error' === $_GET\['send_status_changed'\] ): ?>
        <div class="notice notice-error is-dismissible" style="display:block;">
          <p><?php echo $_GET\['send_status_text'\]; ?></p>
        </div>
        <?php else: ?>
        <div class="notice notice-success is-dismissible" style="display:block;">
          <p><?php echo $_GET\['send_status_text'\]; ?></p>
        </div>
        <?php endif; ?>
      <?php endif; ?>
    <?php }    

  }
}
```

使用 set_send_custom_bulk_actions 加入兩個新的批次操作動作，然後 set_send_custom_bulk_actions_handler 就可以根據選擇的動作進行事件的處理。set_send_custom_bulk_actions_notice 提示文字稍後會再補上，主要是用兩個參數 send_status_text 與 send_status_changed 來決定是否顯示以及提示文字內容。

加入批次操作後介面如下圖：

![](https://oberonlai.blog/wp-content/uploads/2020/07/CleanShot-2020-07-08-at-12.36.54.jpg)

後台的介面設計差不多這樣就算完成了，接下來是處理 WooCommerce 訂單資料。

## WooCommerce 訂單資料取得

為了方便取得 WooCommerce 訂單資料，我們另外寫一支 class-ods-wc-order.php 來處理，除了訂單本身的資料外，還可以把需要計算或是組合資料的先在這邊處理，讓 class-ods-csv.php 專注在組合 csv。

這支類別包含了訂單的五個部分：基本資料、商品明細、運費資料、稅務資料費用資料與折價券資料，完整原始碼為參考 Web Hat 的 [How to Get Order Details by Order ID](https://www.webhat.in/article/woocommerce-tutorial/how-to-get-order-details-by-order-id/) 這篇教學，改寫為 class 後節錄如下：

****ods-wc-order-export/class/class-ods-wc-order.php****
```php
<?php


/**
 * 取得訂單資訊
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

if(!class_exists('ODS_WC_Order')){
  class ODS_WC_Order {

    /**
 * 取得訂單內容
 *
 * @param int $id 訂單 post id
 * @param string $field 訂單主要欄位名稱
 * @param string $subfield 訂單次要欄位名稱
 * 
 * @return array $order_data 訂單資料
 */
    public static function get_order_detail($id, $field = null, $subfield = null, $filter = array()) {

if (is_wp_error($id))
return false;

$dp = (isset($filter\['dp'\])) ? intval($filter\['dp'\]) : 0;
$order = wc_get_order($id);

if ($order === false)
return false;

// 基本資料
$order_data = array(
'id' => $order->get_id(),
'order_number' => $order->get_order_number(),
        'created_at' => $order->get_date_created()->date('Y-m-d'),
        ...
        // 略
);

foreach ($order->get_items() as $item_id => $item) {
$product = $item->get_product();
$product_id = null;
$product_sku = null;
if (is_object($product)) {
$product_id = $product->get_id();
$product_sku = $product->get_sku();
}


// 商品資料
$order_data\['line_items'\]\[\] = array(
'id' => $item_id,
'subtotal' => wc_format_decimal($order->get_line_subtotal($item, false, false), $dp),
'subtotal_tax' => wc_format_decimal($item\['line_subtotal_tax'\], $dp),
'total' => wc_format_decimal($order->get_line_total($item, false, false), $dp),
'total_tax' => wc_format_decimal($item\['line_tax'\], $dp),
'price' => wc_format_decimal($order->get_item_total($item, false, false), $dp),
...
        // 略
);

}

// 運費資料
foreach ($order->get_shipping_methods() as $shipping_item_id => $shipping_item) {
$order_data\['shipping_lines'\]\[\] = array(
'id'  => $shipping_item_id,
'method_id' => $shipping_item\['method_id'\],
'method_title' => $shipping_item\['name'\],
'total' => wc_format_decimal($shipping_item\['cost'\], $dp),
);
}

// 税相關資料
foreach ($order->get_tax_totals() as $tax_code => $tax) {
$order_data\['tax_lines'\]\[\] = array(
'id' => $tax->id,
'rate_id' => $tax->rate_id,
'code' => $tax_code,
'title' => $tax->label,
'total'=> wc_format_decimal($tax->amount, $dp),
'compound' => (bool) $tax->is_compound,
);
}

// 費用資料
foreach ($order->get_fees() as $fee_item_id => $fee_item) {
$order_data\['fee_lines'\]\[\] = array(
'id' => $fee_item_id,
'title' => $fee_item\['name'\],
'tax_class' => (!empty($fee_item\['tax_class'\]) ) ? $fee_item\['tax_class'\] : null,
'total' => wc_format_decimal($order->get_line_total($fee_item), $dp),
'total_tax' => wc_format_decimal($order->get_line_tax($fee_item), $dp),
);
}

// 折價券資料
foreach ($order->get_items('coupon') as $coupon_item_id => $coupon_item) {
$order_data\['coupon_lines'\]\[\] = array(
'id' => $coupon_item_id,
'code' => $coupon_item\['name'\],
'amount' => wc_format_decimal($coupon_item\['discount_amount'\], $dp),
);
}

if( $field === 'line_items'){
$line_items = array();
$i=0;
foreach ($order->get_items() as $item_id => $item) {
$line_items\[\] = $order_data\[$field\]\[$i\]\[$subfield\];
$i++;
};
return $line_items;
} else {
return ($subfield)?$order_data\[$field\]\[$subfield\]:$order_data\[$field\];
}
}

  }
}
```

等下在 class-ods-csv.php 裡面就可以使用 ODS_WC_Order::get_order_detail 來取得訂單資料。

## 匯出 csv 檔與 FTP 上傳

準備好訂單資料，接下來就是開始整理要輸出的 csv 檔，當訂單的系統傳送狀態變更為已傳送與取消傳送時，都需要執行匯出的動作，我們先處理匯出 csv 的細節，也就是組合訂單資料，我們先定義好資料的標頭，然後使用商品名稱來依序輸出：

### 整理 csv 欄位

**ods-wc-order-export/class/class-ods-csv.php**

```php
<?php

/**
 * 1.設定批次操作要執行的動作
 * 2.組合 CSV 欄位
 * 3.執行 FTP 上傳
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

if(!class_exists('ODS_CSV')){
  class ODS_CSV {

    ...
    
    /**
     * 傳送訂單到 send
     * 
     * @param array $post_ids 要傳送的訂單 post ID
     * @param string $send_stauts 此次傳送處理的動作
     * @param string $send_status 傳送後的狀態
     */
    public function set_send_transfer($post_ids, $send_status, $send_status_update){

      //設定系統環境，確保輸出執行環境無礙
      set_time_limit(0);
      
      //開始準備一組匯出陣列
      $csv_arr = array();
      
      //先放置 CSV 檔案的標頭資料
      $csv_arr\[\] = array('訂購人','訂購人電話','訂單編號','收貨人','下單時間','商品編號','商品名稱','商品數量','訂單金額','運費','送貨郵遞區號','送貨地址','原售價','商品售價','付款方式');
      
      //設定檔案輸出名稱
      $filename = "order-export_" . current_time("YmdHis").".csv";
      header('Pragma: no-cache');
      header('Expires: 0');

      $order_data = array();
      $count = $amount = '';
      $i = 0;

      foreach ($post_ids as $post_id) {  
        $order_data\[\] = array(
          'billing_last_name'   => ODS_WC_Order::get_order_detail($post_id,'billing_address','last_name'),
          'billing_phone'       => ODS_WC_Order::get_order_detail($post_id,'billing_address','phone'),
          'order_id'            => ODS_WC_Order::get_order_detail($post_id,'order_number'),
          'shipping_last_name'  => ODS_WC_Order::get_order_detail($post_id,'shipping_address','last_name'),
          'order_date'          => ODS_WC_Order::get_order_detail($post_id,'created_at'),
          'sku'                 => ODS_WC_Order::get_order_detail($post_id,'line_items','sku'),
          'product_name'        => ODS_WC_Order::get_order_detail($post_id,'line_items','name'),
          'quantity'            => ODS_WC_Order::get_order_detail($post_id,'line_items','quantity'),
          'order_total'         => ODS_WC_Order::get_order_detail($post_id,'total'),
          'shipping_cost'       => ODS_WC_Order::get_order_detail($post_id,'total_shipping'),
          'biiling_postcode'    => ODS_WC_Order::get_order_detail($post_id,'billing_address','postcode'),
          'shipping_address'    => ODS_WC_Order::get_order_detail($post_id,'taiwan_address'),
          'regular_price'       => ODS_WC_Order::get_order_detail($post_id,'line_items','regular_price'),
          'payment_method'      => ODS_WC_Order::get_order_detail($post_id,'payment_details','method_title'),
        );
        $i++;
      }

      foreach( $order_data as $item){
        $j = $n = 0;

        /**
         * 組 csv row
         */
        foreach( $item\['product_name'\] as $name ){
          $csv_arr\[\] = array(
            $item\['billing_last_name'\],
            $item\['billing_phone'\],
            $item\['order_id'\],
            $item\['shipping_last_name'\],
            $item\['order_date'\],
            $item\['sku'\]\[$j\],
            $name,
            $item\['quantity'\]\[$j\],
            $item\['order_total'\],
            $item\['shipping_cost'\],
            $item\['biiling_postcode'\],
            $item\['shipping_address'\],
            $item\['regular_price'\]\[$j\],
            $item\['regular_price'\]\[$j\],
            $item\['payment_method'\],
          );
          $j++;
        }
      }

      // 正式循環輸出陣列內容
      $content = '';
      for ($j = 0; $j < count($csv_arr); $j++) {
        $content .= ODS_Helper::csvstr($csv_arr\[$j\])."\\n";
      }

      // 把 csv 寫入 wp-content/uploads
      $upload_dir = wp_upload_dir();
      file_put_contents( $upload_dir\['basedir'\].'/send/'.$filename, $content, FILE_APPEND  LOCK_EX );
    
  }
}
```

set_send_transfer 主要負責 csv 的欄位整理，接受三個參數，分別是被勾選的訂單 ID，訂單未處理前跟處理後要變更的系統傳送狀態，這在稍後會用來驗證訂單是否為正確的傳送狀態，驗證過才能進行變更。

### 指定給批次操作對應動作

到這邊匯出 csv 的動作就完成了，接下來我們要把它指派給批次操作的動作，另外還要做訂單驗證以免重複傳送已傳送的訂單。

**ods-wc-order-export/class/class-ods-csv.php**

```php
<?php

/**
 * 1.設定批次操作要執行的動作
 * 2.組合 CSV 欄位
 * 3.執行 FTP 上傳
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

if(!class_exists('ODS_CSV')){
  class ODS_CSV {

     ...
       
     /**
     * 訂單列表改變系統傳送顯示狀態
     * 
     * @param array $post_ids 有被勾選到的訂單 ID
     * 
     * @return string $status 送出完成後的新狀態
     */
    public function update_send_status( $post_ids, $status ){
      foreach ($post_ids as $post_id) {
        update_post_meta( $post_id, ODS_WC_ORDER_EXPORT_META_KEY, $status);
      }
    }
    
    /**
     * 取得選取訂單的系統傳送狀態
     * 
     * @param array $post_ids 有被勾選到的訂單 ID
     */
    private function get_send_status($post_ids){
      $send_stauts_array = \[\];
      foreach ($post_ids as $post_id) {
        $send_stauts_array\[\] = ODS_WC_Order::get_order_detail($post_id,'send_status');
        return $send_stauts_array;
      }
    }
       
     /**
     * 判斷狀態送出後的頁面跳轉要做的事
     * 
     * @param array $post_ids 有被勾選到的訂單 ID
     * @param string $send_status_before 訂單更新前的系統傳送狀態
     * @param string $send_status_updated 訂單更新後的系統傳送狀態
     * @param string $redirect 跳轉網址
     * 
     * @return string $redirect 返回更新狀態判斷與顯示文字
     */
    private function set_send_custom_bulk_actions_redirect( $post_ids, $send_status_before, $send_stauts_updated, $redirect ){
      if ( count($this->get_send_status($post_ids)) === 1 && end($this->get_send_status($post_ids)) === $send_status_before ) {
        $this->set_send_transfer( $post_ids, $send_status_before, $csv_filename );
        $this->update_send_status( $post_ids,$send_stauts_updated );
        $redirect = add_query_arg( 'send_status_text', '所有選取訂單之系統傳送狀態變更完成！', $redirect );
        $redirect = add_query_arg( 'send_status_changed', 'success', $redirect );
      } else {
        $redirect = add_query_arg( 'send_status_text', '系統傳送狀態變更失敗，所有選取訂單之系統傳送狀態應皆為「'.$send_status_before.'」！', $redirect );
        $redirect = add_query_arg( 'send_status_changed', 'error', $redirect );
      }
      return $redirect;
    }
       
  }
}
```

set_send_custom_bulk_actions_redirect 帶四個參數，分別是被勾選的訂單 ID、變更前與變更後的傳送壯台，以及變更後的跳轉網址。

首先先判斷被勾選訂單的系統傳送狀態是否都一樣，然後這個狀態要符合變更前的系統傳送狀態，條件都成立後才進行 set_send_transfer 的動作，執行完成後使用 update_send_status 來改變訂單的傳送狀態，最後兩個 add_qeury_arg 是提供給 admin_notice 用的顯示判斷，用來告知後台操作人員系統傳送狀態的變更結果。

最後我們回到前面的 set_send_custom_bulk_actions_handler，把 set_send_custom_bulk_actions_redirect 帶入對應的參數指派給選擇到的操作：

**ods-wc-order-export/class/class-ods-csv.php**

```php
<?php

/**
 * 1.設定批次操作要執行的動作
 * 2.組合 CSV 欄位
 * 3.執行 FTP 上傳
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

if(!class_exists('ODS_CSV')){
  class ODS_CSV {

     /**
     * 訂單列表加入系統傳送狀態批次操作項目
     * 
     * @param array $bulk_actions 現有操作項目
     * 
     * @return array $bulk_actions 新增操作項目
     */
    public function set_send_custom_bulk_actions( $bulk_actions ){
      $bulk_actions\['send_to_cancel'\] = '將系統傳送狀態變更為'.ODS_WC_ORDER_EXPORT_META_VALUE\[1\];
      $bulk_actions\['send_to_delivered'\] = '將系統傳送狀態變更為'.ODS_WC_ORDER_EXPORT_META_VALUE\[2\];
      return $bulk_actions;
    }

    /**
     * 訂單列表系統傳送批次操作項目點選後要做的事
     * 
     * @param string $redirect_to 跳轉頁面
     * @param string $doaction 操作項目代稱
     * @param array $post_ids 被勾選的訂單 ID
     * 
     * @return string $redirect_to 操作完成後跳轉的頁面
     */
    public function set_send_custom_bulk_actions_handler( $redirect_to, $doaction, $post_ids){
      switch ($doaction) {
        case 'send_to_cancel':
          // 變為已傳送要做的事
          $redirect_to = $this->set_send_custom_bulk_actions_redirect( $post_ids, ODS_WC_ORDER_EXPORT_META_VALUE\[0\], ODS_WC_ORDER_EXPORT_META_VALUE\[1\], $redirect_to );
          break;
        case 'send_to_delivered':
          // 變為取消傳送要做的事
          $redirect_to = $this->set_send_custom_bulk_actions_redirect( $post_ids, ODS_WC_ORDER_EXPORT_META_VALUE\[1\], ODS_WC_ORDER_EXPORT_META_VALUE\[2\], $redirect_to ) ;
          break;
        default:
          # code...
          break;
      }
      return $redirect_to;
     
    }
    
  }
}
```

補上 case 為 send_to_cancel 與 send_to_delivered 的實作細節，也就是呼叫 set_send_custom_bulk_actions_redirect，這樣就完成了執行批次操作後，csv 檔匯出到 wp-content/uploads/send 資料夾的功能了！

![](https://oberonlai.blog/wp-content/uploads/2020/07/CleanShot-2020-07-09-at-11.03.29-1024x319.jpg)

### FTP 上傳

因為是要整合客戶的內部系統，所以必須把我們整理好的 csv 檔上傳到第三方主機上，這次使用的是 FTP 上傳方式，為了方便日後重複使用，另外寫一支 class-ods-ftp.php 來處理 FTP 的上傳功能，加入後只要在 class-ods-csv.php 直接使用即可。

**ods-wc-order-export/class/class-ods-ftp.php**

```php
<?php

/**
 * FTP 作業
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

if(!class_exists('ODS_FTP')){
  class ODS_FTP {
    
    private $host='';//遠端伺服器地址
    private $user='';//ftp使用者名稱
    private $pass='';//ftp密碼
    private $port=21;//ftp登入埠
    private $error='';//最後失敗時的錯誤資訊
    protected $conn;//ftp登入資源

    /**
     * 可以在例項化類的時候配置資料，也可以在下面的connect方法中配置資料
     * Ftp constructor.
     * @param array $config
     */
    public function __construct(array $config=\[\]){
        empty($config) OR $this->initialize($config);
    }

    /**
     * 初始化資料
     * @param array $config 配置檔案陣列
     */
    public function initialize(array $config=\[\]){
        $this->host = $config\['host'\];
        $this->user = $config\['user'\];
        $this->pass = $config\['pass'\];
        $this->port = isset($config\['port'\]) ?: 21;
    }
    
    // 以下略
```
FTP 的帳密設定在外掛的主要檔案 ods-wc-order-export.php 裡面，接下來回到 class-ods-csv.php 裡面的 set_send_transfer 方法，在最下面把 FTP 上傳加進去：

**ods-wc-order-export/class/class-ods-csv.php**

```php
<?php

/**
 * 1.設定批次操作要執行的動作
 * 2.組合 CSV 欄位
 * 3.執行 FTP 上傳
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

if(!class_exists('ODS_CSV')){
  class ODS_CSV {

    ...
    
    /**
     * 傳送訂單到 send
     * 
     * @param array $post_ids 要傳送的訂單 post ID
     * @param string $send_stauts 此次傳送處理的動作
     * @param string $send_status 傳送後的狀態
     */
    public function set_send_transfer($post_ids, $send_status, $send_status_update){

      ...
        
      // FTP 上傳作業
      if( ODS_FTP_DATA\['enable'\] ){
        $config = \[
          'host'=>ODS_FTP_DATA\['host'\],
          'user'=>ODS_FTP_DATA\['user'\],
          'pass'=>ODS_FTP_DATA\['pass'\],
        \];
        $ftp = new ODS_FTP($config);
        $result = $ftp->connect();
        $local_file = $upload_dir\['basedir'\].'/send/'.$filename;
        $remote_file = ODS_FTP_DATA\['path'\].$filename;
        $ftp->upload($local_file,$remote_file,'ascii');
      }
    
  }
}
```

指定 wp-content/uploads/send/.csv 來進行上傳，這樣一來當匯出完成後就能自動上傳到遠端主機。

## 排程設定

完成手動的操作之後，使用 wp-cron 排程就能讓上述全部動作自動化，只要在固定時間呼叫 class-ods-csv.php 的 set_send_custom_bulk_actions_redirect 方法就能完成了，wp-cron 設定包含三個步驟：外掛啟用時註冊排程 Hook 名稱、把要做的任務掛到這個 Hook 上、外掛停用時取消排程 Hook。

### 註冊排程 Hook 名稱

有別於其他功能的 Hook，我們要把排程 Hook 寫在外掛的主程式裡面：

**ods-wc-order-export.php**

```php
<?php

/**
 * Plugin Name:       ODS WooCommerce Order Export
 * Plugin URI:        https://oberonlai.blog
 * Description:       WooCommerce 客製化報表自動匯出&上傳功能
 * Version:           1.0.1
 * Author:            Oberon Lai
 * Author URI:        https://oberonlai.blog
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

...

// 加入排程 Hook
function sp_activation(){
  if ( ! wp_next_scheduled( 'ods_wc_order_export_cron' ) ) {
    wp_schedule_event( strtotime('00:00:00'), 'daily', 'ods_wc_order_export_cron' );
  }
}
register_activation_hook( __FILE__, 'sp_activation');

function sp_deactivation(){
  wp_clear_scheduled_hook('ods_wc_order_export_cron');
}
register_deactivation_hook( __FILE__, 'sp_deactivation');

ods_wc_order_export_cron 為 Hook 名稱，稍後需要執行的任務會掛在這個 Hook 上，wp_schedule_event 為註冊排程 Hook 的 function，第一個參數 timestamp，使用 strtotime 為 UTF-0，所以如果想在台灣時間下午一點執行的話，就要寫 13-8 = 5:00:00。

第二個參數 daily 設定執行頻率為每天，最後則是 Hook 名稱。使用 register_activation_hook 讓外掛啟用時註冊這個排程 Hook，而用 register_deactivation_hook 在外掛停用時取消這個排程，這樣可以減少主機的壓力，沒用到的東西就把它移除掉。

接下來與前面的步驟一樣，我們先把 Hook 處理好再來寫實作細節，開啟 class-ods-hook.php 來加入排程：
```
**ods-wc-order-export/class/class-ods-hook.php**
```php
<?php

/**
 * 1.引入 class
 * 2.訂單新增系統傳送狀態欄位，用來紀錄該訂單是否已被傳送至企業內部系統
 * 3.處理 Hooks
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

require plugin_dir_path( __FILE__ ) . 'class-ods-helper.php';
require plugin_dir_path( __FILE__ ) . 'class-ods-wc-admin.php';
require plugin_dir_path( __FILE__ ) . 'class-ods-wc-order.php';
require plugin_dir_path( __FILE__ ) . 'class-ods-csv.php';
require plugin_dir_path( __FILE__ ) . 'class-ods-ftp.php';

if(!class_exists('ODS_Hook')){
  
  class ODS_Hook {
    
    private $plugin_version;

    function __construct( $wc_admin, $csv ){

      ...

      // 訂單自動匯出 csv
      add_action('ods_wc_order_export_cron', array( $csv, 'set_transfer_delivered'));

    }
  }
  
  $erp = new ODS_Hook(
    new ODS_WC_Admin(),
    new ODS_CSV()
  );
}
```
這邊還有一個 Hook 要補上：因為自動處理的訂單需要篩選出系統傳送狀態為未傳送的訂單，而使用 WC_Order_Query 要篩選自訂欄位必須要另外增加 Hook 來處理，所以再加上下面這段 Hook：

**ods-wc-order-export/class/class-ods-hook.php**
```php
<?php

/**
 * 1.引入 class
 * 2.訂單新增系統傳送狀態欄位，用來紀錄該訂單是否已被傳送至企業內部系統
 * 3.處理 Hooks
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

require plugin_dir_path( __FILE__ ) . 'class-ods-helper.php';
require plugin_dir_path( __FILE__ ) . 'class-ods-wc-admin.php';
require plugin_dir_path( __FILE__ ) . 'class-ods-wc-order.php';
require plugin_dir_path( __FILE__ ) . 'class-ods-csv.php';
require plugin_dir_path( __FILE__ ) . 'class-ods-ftp.php';

if(!class_exists('ODS_Hook')){
  
  class ODS_Hook {
    
    private $plugin_version;

    function __construct( $wc_admin, $csv ){

      ...

      // 把系統傳送狀態加入 wc order query 的搜尋條件
      add_filter( 'woocommerce_order_data_store_cpt_get_orders_query', 'ODS_WC_Order::set_order_query_custom_key', 10, 2 );

    }
  }
  
  $erp = new ODS_Hook(
    new ODS_WC_Admin(),
    new ODS_CSV()
  );
}
```

set_order_query_custom_key 因為是跟 WooCommerce 訂單相關的事件，所以定義在 class-ods-wc-order.php 裡面

**ods-wc-order-export**/**class/class-ods-wc-order.php**

```php
<?php

/**
 * 取得訂單資訊
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

if(!class_exists('ODS_WC_Order')){
  class ODS_WC_Order {

...

/**
     * 加入 order query 搜尋條件系統傳送狀態
     */
    public static function set_order_query_custom_key( $query, $query_vars ) {
      if ( ! empty( $query_vars\[ODS_WC_ORDER_EXPORT_META_KEY\] ) ) {
        $query\['meta_query'\]\[\] = array(
          'key' => ODS_WC_ORDER_EXPORT_META_KEY,
          'value' => esc_attr( $query_vars\[ODS_WC_ORDER_EXPORT_META_KEY\] ),
        );
      }
      return $query;
    }
  }
}

```
接下來去 class-ods-csv.php 實作 set_send_transfer_delivered 的細節：

****ods-wc-order-export****/**class/class-ods-csv.php**
```php
<?php

/**
 * 1.設定批次操作要執行的動作
 * 2.組合CSV欄位
 * 3.執行FTP上傳
 */

if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

if(!class_exists('ODS_CSV')){
  class ODS_CSV {

    ...

    /**
     * 每日自動處理配系統傳送狀態改為已傳送
     */
    public function set_send_transfer_delivered(){
      $query = new WC_Order_Query( array(
        'limit' => 9999,
        'return' => 'ids',
        ODS_WC_ORDER_EXPORT_META_KEY => ODS_WC_ORDER_EXPORT_META_VALUE\[0\]
      ));
      $orders = $query->get_orders();
     if( $orders ){
        foreach ($orders as $order_id) {
          $this->set_send_transfer( $order_id, ODS_WC_ORDER_EXPORT_META_VALUE\[0\], ODS_WC_ORDER_EXPORT_META_VALUE\[1\] );
          $this->update_send_status( $order_id, ODS_WC_ORDER_EXPORT_META_VALUE\[1\] );
        }
      }
    }

  }
}
```
在第 25 行的地方就是我們剛剛使用 set_order_query_custom_key 加入的功能，篩選出系統傳送狀態為未傳送的訂單，然後直接呼叫 set_send_transfer 來匯出 csv 檔即可。

做 WP Cron 需要注意的地方是只有當網站有訪客瀏覽時才會進行觸發，如果是三更半夜或是頁面有快取的話，很可能就不會被觸發到，最保險的方法還是用系統的 crontab 來執行 wp-cron.php，或是 wget 來進行呼叫。

註冊好排程之後，可以用 [WP Crontrol](https://wordpress.org/plugins/wp-crontrol/) 這隻外掛來檢視目前站內已經有排定的 Cron，然後用 [Cron Log](https://wordpress.org/plugins/cron-logger/) 來追蹤已經執行過的排程，用這兩隻外掛就能很方便管理 WP Cron。

## 總結

以上完整的程式碼可以參考我的 Github ---> [https://github.com/oberonlai/ods-wc-order-export](https://github.com/oberonlai/ods-wc-order-export)，如果在實務上需要加入更多報表的運算或處理，像是計算折扣商品、變化商品甚至是組合商品等等，都可以用這隻外掛下去做衍伸修改，希望能幫助到有需要的朋友！

### 參考資料

感謝下面各位大大的指導與分享：

[PHP使用FTP上傳檔案到伺服器(實戰篇)](https://www.itread01.com/content/1544468049.html)  
[\[PHP\] 寫出一個匯出 CSV 檔案的起手式](https://www.mxp.tw/6852/)  
[\[PHP\] 匯出處理 – CSV、EXCEL匯出實例教學](https://code.yidas.com/php-csv-excel-output/)  
[How to Get Order Details by Order ID](https://www.webhat.in/article/woocommerce-tutorial/how-to-get-order-details-by-order-id/)  
[Using Custom Bulk Actions](https://make.wordpress.org/core/2016/10/04/custom-bulk-actions/)  
[How to sort orders based on orders meta value in Woocommerce / Wordpress](https://stackoverflow.com/questions/24723182/how-to-sort-orders-based-on-orders-meta-value-in-woocommerce-wordpress)  
[Add custom order status to filter menu in WooCommerce Admin Orders list](https://stackoverflow.com/questions/53346544/add-custom-order-status-to-filter-menu-in-woocommerce-admin-orders-list)  
[wc_get_orders and WC_Order_Query](https://github.com/woocommerce/woocommerce/wiki/wc_get_orders-and-WC_Order_Query#description)  
[Filter orders by product post type in WooCommerce admin orders list page](https://stackoverflow.com/questions/56132215/filter-orders-by-product-post-type-in-woocommerce-admin-orders-list-page)