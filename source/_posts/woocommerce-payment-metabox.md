---
title: WooCommerce 金流串接實戰（六）- 自訂欄位
date: 2021-09-21 9:00:00
categories:
- WooCommerce
tags:
- WooCommerce 金流
- WordPress 外掛
- WooCommerce 外掛
- WC_Settings_Page
etoc: false
---

先來回顧一下目前鐵人付金流外掛的資料夾結構：

```
iron-pay
├── composer.json
├── composer.lock
├── iron-pay.php
├── src
│   └── Options.php
└── vendor
```

上一篇我們完成了 WooCommerce 的全域設定，接下來我們要在後台的訂單管理頁新增一個交易結果以及紀錄金流商的回傳訊息，以便客戶日後可以針對有問題的訂單進行追蹤。WordPress 的後台有很多現成的欄位，參考下圖：

![](https://oberonlai.blog/wp-content/uploads/wordpress-metabox/wordpress-metabox-01.jpg)

如果要新增的很容易，使用函式 ```add_meta_box()``` 跟勾點 ```add_meta_boxes``` 來傳入產生表單欄位的 HTML，最後使用勾點 ```save_post``` 在儲存文章時觸發 ```update_post_meta()``` 即可將該欄位的資訊寫入資料庫：

```php
<?php
function irp_add_custom_box() {
	$screens = array( 'shop_order' );
	foreach ( $screens as $screen ) {
		add_meta_box(
			'iron_pay_result',
			'鐵人付交易結果',
			'irp_custom_box_html',
			$screen,
			'side',
			'low'
		);
	}
}
add_action( 'add_meta_boxes', 'irp_add_custom_box' );

function irp_custom_box_html( $post ) {
	$code   = get_post_meta( $post->ID, '_irp_resp_code', true );
	$result = get_post_meta( $post->ID, '_irp_resp_result', true );
	?>
	<p>回傳代號<input type="text" name="irp_resp_code" value="<?php echo esc_attr( $code ); ?>" x-readonly style="display:block;width:100%;margin-top: 5px;"></p>
	<p>交易結果<textarea rows="5" name="irp_resp_result" x-readonly style="display:block;width:100%;margin-top: 5px;"><?php echo esc_textarea( $result ); ?></textarea></p>
	<?php
}

function irp_save_postdata( $post_id ) {
	if ( array_key_exists( 'irp_resp_code', $_POST ) ) {
		update_post_meta(
			$post_id,
			'_irp_resp_code',
			$_POST['irp_resp_code']
		);
	}
	if ( array_key_exists( 'irp_resp_result', $_POST ) ) {
		update_post_meta(
			$post_id,
			'_irp_resp_result',
			$_POST['irp_resp_result']
		);
	}
}
add_action( 'save_post', 'irp_save_postdata' );
```

這樣就完成我們要的欄位了～

![](https://oberonlai.blog/wp-content/uploads/wordpress-metabox/wordpress-metabox-02.jpg)

以上範例修改自官方文件，然而這個做法真的要記起來每個步驟真的非常困難，再加上還必須要知道一堆函式與勾點，我寫了這麼多年，每次真的要用到時都還是要去查文件，覺得效率不高，因此我整理了一套 wp-metabox 的 PHP 套件來簡化這些步驟，接下來示範如何使用這個套件來完成一樣的自訂欄位。

<!--more-->


## 新增訂單資料夾結構

我們先進入 src，資料夾，並新增 Posts 以及在 Posts 裡面的 ShopOrder 資料夾，並增加 Metabox.php 檔案，完成後資料夾結構如下：

```
iron-pay
├── composer.json
├── composer.lock
├── iron-pay.php
├── src
│   ├── Options.php
│   └── Posts
│       └── ShopOrder
│           └── Metabox.php
└── vendor
```

之所以要用兩個資料夾來分類，主要目的是為了方便日後管理，我是根據 WordPress 的資料庫結構來設計的，Posts 資料夾對應的是 wp_posts，而 ShopOrder 是 post type 為 shop_order 的相關檔案，裡面放了自訂欄位的 Metabox.php，如果之後需要做訂單相關的非同步處理，就可以增加 Ajax.php，或是要做訂單資料的撈取就可以新增 Query.php。

萬一之後客戶需要在商品管理頁面增加自訂欄位，那它的檔案路徑就會是 Posts/Product/Metabox.php，因為 Product 也是存放在 wp_posts 裡面的。這樣的分類方式可能要對資料表有一定的熟悉程度會比較好上手，這樣做的好處是如果在其他專案也需要用到訂單的自訂欄位或是其他功能，整包資料夾複製過去後修改命名空間的第一層名稱就可以重用。

```php
namespace {yourprefix}/Posts/ShopOrder
```

然後類別名稱、檔案名稱都不用再修改，不然常見的 WordPress 外掛檔名寫法都是要帶一堆前綴，然後宣告類別前要先檢查類別是否存在，引入命名空間之後可以很大程度增加程式碼的重用性，同時讓資料夾結構對應到命名空間，就不用再取 class-yourprefix-shop-order-metabox.php 這種又臭又長的名字。

## 安裝 wp-metabox 套件

將目錄切回到 iron-pay 資料夾底下，來進行 wp-metabox 套件的安裝：

```sh
 ~/Sites/woocommerce/wp-content/plugins/iron-pay$ composer require oberonlai/wp-metabox 
```

安裝完成後開啟 src/Posts/ShopOrder/Metabox.php 檔案，貼入以下程式碼：

```php
<?php

namespace Irp\Posts\ShopOrder;

use ODS\Metabox;

$metabox = new Metabox(
	array(
		'id'       => 'iron_pay_field',
		'title'    => '鐵人付交易結果',
		'screen'   => 'shop_order',
		'context'  => 'side',
		'priority' => 'low',
	)
);

$metabox->addText(
	array(
		'id'    => 'irp_resp_code',
		'label' => '回應代號',
	)
);

$metabox->addTextarea(
	array(
		'id'    => 'irp_resp_result',
		'label' => '交易結果',
	)
);
```

結果如下：

![](https://oberonlai.blog/wp-content/uploads/wordpress-metabox/wordpress-metabox-03.jpg)

有別於最原始的範例，我們再也不用記一堆韓式跟勾點，也不用處理欄位值得儲存或更新，只要建立 Metabox 實例，然後看要新增什麼欄位就用對應的方法，透過跟 WooCommerce Settings API 類似的陣列值傳入方式來設定欄位，以下針對 Metabox 類別傳入的參數做說明：

**id** 自訂區塊的唯一 ID，所有的自訂欄位都要放在自訂區塊中，如果想要新增不同的區塊則需要再建立另外一個 new Metabox()

**title** 自訂欄位的顯示名稱

**screen** 設定顯示在哪個 Post Type 底下，我們要顯示在訂單頁所以是 shop_order。該如何知道目標的頁面是哪種 post type? 可以從網址最後的一個參數 post_type 就知道：

```
https://woocommerce.test/wp-admin/edit.php?post_type=shop_order
```

**context** 自訂區塊的顯示位置，可選值有 normal, side, advanced，side 是在右側欄

**priority** 自訂區塊與其他區塊的順序，可選值有 high、core、default、low，越高越前面

此外，該套件支援八種自訂欄位，常見的文字、單選框、下拉選單，另外還有圖片上傳、所見即所得編輯器以及重複器，具體的用法都很相似，傳入的參數都只有三個，id、label 與 desc，比較特別的是重複器欄位，但在這個外掛之中我們先不會用到它，更多欄位用法可以參考套件中的 README.md。

另外如果我們需要傳入更多的參數像是 class、style、attr 之類的屬性也非常容易，只要開啟 vendor/oberonlai/wp-metabox/Metabox.php 這個類別，就能看到 add 欄位以及產出 HTML 的方法，從中我們還可以學到 WordPress 的過濾方法，像是 esc_attr() 以及 esc_html() 等等，這些都是在開發外掛時需要注意的安全性。

眼尖的朋友可能會發現 WooCommerce 有內建一個訂單備註的欄位，在實務上我們常常也會用它來記錄訂單的狀態變更以及相關資訊，但如果一個訂單備註過多時就會變得不太好找，所以我自己在習慣上都還是會另外開一個自訂區塊來處理金流商的回傳資訊。

完成設定頁面後下一篇我們先來了解 WooCommerce 的結帳流程，以及最重要的金流類別 WC_Payment_Gateway。








