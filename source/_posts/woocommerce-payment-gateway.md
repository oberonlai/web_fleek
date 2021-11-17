---
title: WooCommerce 金流串接實戰（七）- WC_Payment_Gateway
date: 2021-09-22 12:00:00
categories:
- WooCommerce
tags:
- WooCommerce 金流
- WordPress 外掛
- WooCommerce 外掛
- WC_Settings_Page
etoc: false
---

WooCommerce 的金流主要分為兩種，即時與非即時完成付款，即時指的是當訂單成立後就可以取得消費者的付款結果，像是信用卡、WebATM，WooCommerce 提供三種模式來處理即時付款：

1. 付款表單 - 讓消費者直接在站內完成信用卡或是付款相關資訊的填寫，由於安全性的考量，台灣有支援這種模式的金流商相對較少，但因為其結帳的便利性可降低付款跳出率，這幾年有不少廠商開始支援 Token 交易代碼化，也就是傳送過程中並非直接傳送消費者的卡號，而是使用一串代碼來避免直接暴露。

2. Iframe 嵌入 - 按下結帳時使用 Iframe 顯示卡號輸入畫面，之前國內也有金流商是採用這種方式付款，但該金流商目前已停止該類型的付款方式。

3. 頁面跳轉 - 不讓消費者在站內提供付款資訊，而是跳轉到金流商的頁面進行付款，這是目前的主流方式，根據金流商運作模式，有些是使用 Form 來 Post，或是需要呼叫 API 來取得跳轉網址。

非即時付款的金流常見的有虛擬轉帳帳號、超商代碼繳費，有些金流商會需要跳轉到他們的介面，像是選擇要產生的轉帳帳號是用哪家銀行、列印哪家超商的繳費代碼，也有廠商沒提供這些選擇，就是單純的直接回傳固定一家銀行或超商的繳費資訊，這種類型的就是直接將回傳結果顯示在站內即可。

<!--more-->

WooCommerce 最常見的結帳流程為：加入購物車 > 輸入帳單資訊 > 跳轉到第三方付款頁 > 跳轉回站內感謝頁，接下來說明新增 WooCommerce 金流付款的架構。

## WooCommerce 新增金流 Gateway

在實作之前我們先釐清中英對照，Payment 指的是付款公司，實務上會是把金流商的公司名稱作為 Payment，而一個金流商會提供許多不同的付款方式，像是信用卡、ATM、超商代碼等等，而這些付款方式就是 Gateway。

首先繼承 WooCommerce 的金流類別 ```WC_Payment_Gateway```，並將類別的宣告放在 ```plugins_loaded``` 勾點裡面：

```php
function add_gateway() {
    class CreditCard_Gateway extends WC_Payment_Gateway {}
}
add_action( 'plugins_loaded', 'add_gateway' );
```

CreditCard_Gateway 不用建立實例，這部分 WooCommerce 會幫我們處理，在命名上最好可以很直觀的理解這是哪一種付款方式，接下來在 woocommerce_payment_gateways 勾點新增付款方式的類別名稱即可：

```php
function add_gateway_class( $methods ) {
    $methods[] = 'CreditCard_Gateway'; 
    return $methods;
}
add_filter( 'woocommerce_payment_gateways', 'add_gateway_class' );
```

這樣就能完成金流功能的註冊，接下來我們需要實作 ```My_Gateway``` 類別。

## CreditCard_Gateway 類別必要實作屬性與方法

首先要有建構式來指定金流名稱、ID、描述等等基本資料，另外還需要執行設定項的初始化：

```php
function add_gateway() {
	class CreditCard_Gateway extends WC_Payment_Gateway {
		public function __construct() {
			$this->id                 = 'my_credit_card';
			$this->icon               = 'imgUrl';
			$this->has_fields         = false;
			$this->method_title       = '信用卡';
			$this->method_description = '使用信用卡付款';
		}
	}
}
add_action( 'plugins_loaded', 'add_gateway' );
```

 `$this->id` – 付款方式的唯一 ID，最好帶有自己的前綴，因為同一間金流商可能會有其他第三方開發金流外掛，如果只用金流商的前綴發生衝突的機率很高

`$this->icon` – 顯示在前台結帳頁付款名稱旁的圖標，讓消費者知道他們會透過哪一間金流公司進行付款

`$this->has_fields` – 在結帳頁是否有提供付款資訊填寫表單，因為我們大部分都是使用跳轉到第三方，所以這邊設成 false 即可

`$this->method_title` – 付款方式名稱，會顯示在 WooCommerce 的設定頁

`$this->method_description` - 付款方式描述，會顯示在 WooCommerce 的設定頁

設定完屬性後，建構式裡面還需要設定以下內容：

```php
function add_gateway() {
	class CreditCard_Gateway extends WC_Payment_Gateway {
		public function __construct() {
			// 略
			$this->init_form_fields();
			$this->init_settings();
			
			$this->title = $this->get_option( 'title' );
			$this->description = $this->get_option( 'description' );

			add_action( 'woocommerce_update_options_payment_gateways_' . $this->id, array( $this, 'process_admin_options' ) );
		}
	}
}
add_action( 'plugins_loaded', 'add_gateway' );
```

第一個方法 ```init_form_fields()``` 稍後會實作，它的功用在建立 Gateway 的設定項，在前面的文章中，我們已經把金流的相關設定放在全域設定裡面，因為一個金流商會提供多種 Gateway，所以把設定放在 Gateway 裡面的話對於使用者來說會很麻煩，一樣的金流商店代號、密鑰資訊在信用卡、ATM、超商代碼都要個別輸入，因此這類資訊放在全域設定裡面會比較恰當。

但如果你接的金流只有一個 Gateway 的話，把設定項放在這邊就很適合。第二個方法是 ```init_settings()```，它的功用是初始化設定，之後就可以透過 ```get_option()``` 取得 init_form_fields 裡面的設定欄位值，像是：

```php
$this->title = $this->get_option( 'title' );
$this->description = $this->get_option( 'description' );
```


title 跟 description 稍後會實作在 ```init_form_field()``` 方法裡面，而這邊 ```$this->title``` 以及 ```$this->description``` 有別於 ```method_title``` 與 ```method_description```，他們是顯示在前台結帳頁的付款方式名稱，這個名稱可以透過設定項來讓使用者自行修改，而最後的勾點 ```woocommerce_update_options_payment_gateways_{gateway_id}``` 則是更新設定內容，```process_admin_options``` 我們不需實作，交由 ```WC_Payment_Gateway``` 處理即可。

接下來是實作 ```init_form_fields``` 方法，我們只需要最基本的三個欄位即可：

```php
function add_gateway() {
	class CreditCard_Gateway extends WC_Payment_Gateway {
		public function __construct() {
			// 略
		}
		public function init_form_fields() {
			$this->form_fields = array(
				'enabled'     => array(
					'title'   => '啟用/停用',
					'label'   => '啟用付款',
					'type'    => 'checkbox',
					'default' => 'no',
				),
				'title'       => array(
					'title' => '付款方式名稱',
					'type'  => 'text',
				),
				'description' => array(
					'title' => '付款方式描述',
					'type'  => 'textarea',
				),
			);
		}
	}
}
add_action( 'plugins_loaded', 'add_gateway' );
```

最後是 ```process_payment``` 方法，它會處理點擊結帳按鈕後的行為，包含建立訂單、改變訂單狀態、清空購物車、將欄位傳送到金流商、取得網址後進行頁面跳轉，這部分我們會在下一篇串接信用卡來進行實作。

## WC API 的用途

通常傳送訂單資料給金流商的時候，都會需要帶一個成功付款後的完成結帳網址，當金流商走完流程後就會根據該網址來進行頁面跳轉，WooCommerce API 提供一個動態的勾點 ```woocommerce_api_{slug}```，只要替換 slug 就能製造出這個跳轉網址，範例如下：

```php
function redirect_thankyou_page( $order ){
	// 處理金流商回傳的參數
	$args = $_REQUEST;
	wp_safe_redirect( $order->get_checkout_order_received_url() );
}
add_action( 'woocommerce_api_thankyou', array( $this, 'redirect_thankyou_page' ) );
```

當金流商導回 https://example.com/wc-api/thankyou 的時候，我們就能拿到參數的資訊做處理，然後將頁面跳轉到感謝頁，在非即時付款金流商要背景通知付款完成的情境下，也可以使用這個勾點來做到接收參數的功能。


以下為付款方式 Gateway 類別的完整基礎架構：

```php
function add_gateway() {
	class CreditCard_Gateway extends WC_Payment_Gateway {

		public function __construct() {
			$this->id                 = 'credit_card';
			$this->icon               = 'imgUrl';
			$this->has_fields         = false;
			$this->method_title       = '信用卡';
			$this->method_description = '使用信用卡付款';

			$this->init_form_fields();
			$this->init_settings();

			$this->title       = $this->get_option( 'title' );
			$this->description = $this->get_option( 'description' );

			add_action( 'woocommerce_update_options_payment_gateways_' . $this->id, array( $this, 'process_admin_options' ) );
		}

		public function init_form_fields() {
			$this->form_fields = array(
				'enabled'     => array(
					'title'   => '啟用/停用',
					'label'   => '啟用付款',
					'type'    => 'checkbox',
					'default' => 'no',
				),
				'title'       => array(
					'title' => '付款方式名稱',
					'type'  => 'text',
				),
				'description' => array(
					'title' => '付款方式描述',
					'type'  => 'textarea',
				),
			);
		}

		public function process_payment(){
			// 處理點擊結帳按鈕後的行為
		}
	}
}
add_action( 'plugins_loaded', 'add_gateway' );
```

下一篇我們來實作鐵人付金流的信用卡 Gateway。