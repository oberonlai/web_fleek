---
title: WooCommerce 金流串接實戰（八）- 發起付款請求
date: 2021-09-23 09:00:00
categories:
- WooCommerce
tags:
- WooCommerce 金流
- WordPress 外掛
- WooCommerce 外掛
- WC_Settings_Page
etoc: false
---

了解 WooCommerce 金流的基本架構後，我們來進行串接的實作，在開始前先回顧一下目前的外掛結構：

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

首先我們假設鐵人付這家金流公司的支付運作模式，以信用卡與虛擬帳號轉帳兩種付款方式為例，說明實際狀況中可能會遇到的方法以及對應的設計方式，理論上同一家金流公司都會採用同一種邏輯，本文為了示範不同情境所以採用不同邏輯，實際情況以金流商提供的技術文件為主。

<!--more-->


### 鐵人付信用卡付款流程

建立訂單採用 HTTPS 協議， 將訂單資料以 POST ( HTTP Method ) 傳送，返回結果採用 JSON 格式，返回結果如下：

```json
{
	"status": "200",
	"info": "訂單建立成功",
	"data": {
		"html": "https://ironpay.test/payment", 	
		"out_trade_no": "111"
	}
}
```

當按下結帳按鈕時要發起 API 請求，在經過驗證請求來源後會回傳付款網址，取得該網址後提供給 ```WC_Payment_Gateway``` 的 ```process_payment()``` 來進行跳轉。


### 鐵人付虛擬帳號付款流程

建立訂單採用 HTTPS 協議， 將訂單資料以 POST ( HTTP Method ) 傳送後立即返回取號結果，等消費者轉帳完成後金流商由伺服器呼叫客戶網站的通知付款網址。

## 新增付款方式

接下來我們先在 src 建立 Gateways 資料夾，並新增 ```CreditCard.php```、```VirtualAccount.php```、```Request.php``` 以及 ```Response.php``` 四個檔案，並另外在 src 資料夾增加一個工具類別 ```Utility.php```：

```
iron-pay
├── composer.json
├── composer.lock
├── iron-pay.php
├── src
│   ├── Gateways
│   │   ├── CreditCard.php
│   │   ├── Request.php
│   │   ├── Response.php
│   │   └── VirtualAccount.php
│   ├── Options.php
│   ├── Posts
│   │   └── ShopOrder
│   │       └── Metabox.php
│   └── Utility.php
└── vendor
```

CreditCard 與 VirtualAccount 分別為實作信用卡與虛擬帳號的類別，Request 為建立訂單的請求類別，Response 負責處理從金流商回傳的資料。根據上一篇的架構，```CreditCard.php``` 內容如下：

```php
<?php

namespace Irp\Gateways;

defined( 'ABSPATH' ) || exit;

/**
 * Set payment class
 */
function add_irp_credit_card() {
	class CreditCard extends \WC_Payment_Gateway {

		public function __construct() {
			$this->id                 = 'irp_credit_card';
			$this->icon               = '';
			$this->has_fields         = false;
			$this->method_title       = '鐵人付信用卡';
			$this->method_description = '使用鐵人付信用卡付款';

			$this->init_form_fields();
			$this->init_settings();

			$this->title       = $this->get_option( 'title' );
			$this->description = $this->get_option( 'description' );

			add_action( 'woocommerce_update_options_payment_gateways_' . $this->id, array( $this, 'process_admin_options' ) );
		}

		public static function register() {
			add_filter( 'woocommerce_payment_gateways', array( __CLASS__, 'add_gateway_class' ) );
		}

		public function add_gateway_class( $methods ) {
			$methods[] = __CLASS__;
			return $methods;
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
					'css'   => 'max-width: 400px;',
				),
			);
		}

		public function process_payment( $order_id ) {
			// 處理付款請求
		}
	}

	CreditCard::register();

}
add_action( 'plugins_loaded', 'Irp\Gateways\add_irp_credit_card' );


```

可以看到除了基本的架構外，我們新增了兩個方法，一個是靜態方法 ```reigster()``` 來管理勾點 ```woocommerce_payment_gateways``` 的類別名稱註冊，另一個 ```add_gateway_class()``` 方法來取得類別名稱，最後再用 ```CreditCard::register()``` 呼叫勾點，就能把新增付款方式的功能包在同一個函式之中。

然後我們依樣畫葫蘆來建立虛擬帳號的付款方式，開啟 VirtualAccount.php，貼入以下程式碼：

```php
<?php

namespace Irp\Gateways;

defined( 'ABSPATH' ) || exit;

/**
 * Set payment class
 */
function add_irp_virtual_account() {
    class VirtualAccount extends \WC_Payment_Gateway {

        public function __construct() {
            $this->id                 = 'irp_virtual_account';
            $this->icon               = '';
            $this->has_fields         = false;
            $this->method_title       = '鐵人付虛擬帳號';
            $this->method_description = '使用鐵人付虛擬帳號付款';

            // 以下略
    }

    VirtualAccount::register();

}
add_action( 'plugins_loaded', 'Irp\Gateways\add_irp_virtual_account' );
```

完成後就能在 WooCommerce 設定頁看到鐵人付相關的付款方式：

![](https://oberonlai.blog/wp-content/uploads/woocommerce-payment-gateway/woocommerce-payment-gateway-02.jpg)

在實務中，如果不同的 Gateway 有相同的屬性或方法，可以把共用元素獨立成抽象類別，然後讓抽象類別繼承 ```WC_Payment_Gateway```，要實作的付款方式再繼承這個抽象類別，或是把相同的部分拆成 Trait，就能方便管理不同付款方式之間的共有屬性與方法。

## 新增請求類別

請求的部分因為有很多參數要彙整以及帶有不同的請求方式，所以我們把請求拆成獨立的 Request 類別來處理，然後再於付款方式裡面的 ```process_payment()``` 來建立實例，並傳入 Gateway 類別本身來完成請求，Request 基本架構如下：

```php
<?php

namespace Irp\Gateways;

defined( 'ABSPATH' ) || exit;

class Request {
	/**
	 * The gateway instance
	 *
	 * @var WC_Payment_Gateway
	 */
	protected $gateway;

	/**
	 * Constructor
	 *
	 * @param  WC_Payment_Gateway $gateway The payment gateway instance.
	 */
	public function __construct( $gateway ) {
		$this->gateway = $gateway;
	}	
}
```

首先，我們要取得 Gateway 實例的相關屬性與方法，所以使用 ```$gateway``` 來存放付款類別，並在建構式指定給 ```$gateway``` 屬性。

```php
<?php

namespace Irp\Gateways;

defined( 'ABSPATH' ) || exit;

class Request {
	// 略
	public function __construct( $gateway ) {
		// 略
	}	
	/**
	 * Build transaction args.
	 *
	 * @param WC_Order $order The order object.
	 * @return array
	 */
	public function get_transaction_args( $order ) {
		$args = apply_filters(
			$this->gateway->id . '_transaction_args' ,
			array(
				'nonce_str'       => 'nonce',
				'orgno'           => '商家代號',
				'out_trade_no'    => $order->get_order_number(),
				'secondtimestamp' => time(),
				'total_fee'       => $order->get_total(),
			),
			$order
		);
		return $args;
	}
}
```

接下來透過 ```get_transaction_args()``` 方法來整理要傳送給金流商的參數，這邊的參數陣列會放在 ```apply_filters()``` 裡面，這個 WordPress 內建的函式讓我們可以自訂勾點，第一個參數傳勾點名稱、第二個傳陣列內容，第三個可以讓這個勾點帶入參數，之後我們就可以用 ```add_filter( 'irp_credit_card_transaction_args', 'callback', 10, 2 )```方式來動態新增參數：

```php
function add_transaction_args( $args, $order ){
	return array_merge(
		$args,
		array(
			'returnurl' => home_url( 'wc-api/iron-pay' ),
			'backurl'   => home_url( 'wc-api/iron-pay-offline' ),
		)
	);
}
add_filter( 'irp_credit_card_transaction_args', 'add_transaction_args', 10, 2 )
```

因為每個付款方式可能會需要帶入不同的參數，所以 Request 這邊先寫好共用的參數，在付款方式那邊就可以使用勾點 Filter 額外新增需要的參數。

Utility 類別定義了一系列靜態方法，像是做演算法的雜湊、產出隨機字串、Log 方法等等，這裡面的方法大部分可以通用在不同的專案之中，像是一種工具箱的概念。接下來是第一種呼叫 API 的方式：

```php
<?php

namespace Irp\Gateways;

defined( 'ABSPATH' ) || exit;

class Request {
	// 略
	public function __construct( $gateway ) {
		// 略
	}	
	/**
	 * Build transaction args.
	 *
	 * @param WC_Order $order The order object.
	 * @return array
	 */
	public function get_transaction_args( $order ) {
		// 略
	}
	
	/**
	 * Request api and get redirect url
	 *
	 * @param WC_Order $order The order object.
	 * @return void
	 */
	public function build_request( $order ) {
		$order   = new WC_Order( $order );
		$options = array(
			'method'  => 'POST',
			'timeout' => 60,
			'body'    => $this->get_transaction_args( $order ),
		);

		$response = wp_remote_request( '金流商 API 請求網址', $options );

		if ( ! is_wp_error( $response ) ) {
			// API 回傳資料處理
			$body = json_decode( wp_remote_retrieve_body( $response ), true );
			// 取得跳轉網址後返回
			return $body['data']['html']; // 取得跳轉網址：'https://ironpay.test/payment';
		} else {
			// API 請求失敗的處理
		}
	}
}
```

這一段需要值得注意的是 ```wp_remote_request()``` 這個函式，它封裝了 WordPress 的 HTTP 請求物件，透過它可以很方便的發起 HTTP 請求，第一個參數為請求網址，第二個為要傳送的參數，我們把金流商所需的資料藉由 ```get_transaction_args()``` 取得後放在 body 進行傳送，```wp_remote_request()``` 的如果請求正確回傳 JSON 時，則用 ```wp_remote_retrieve_body()``` 取得 JSON 後進行格式轉換，如果請求的結果有誤，使用 ```is_wp_error()``` 來進行判斷。

第二種傳送方法是使用表單：

```php
<?php

namespace Irp\Gateways;

defined( 'ABSPATH' ) || exit;

class Request {
	// 略
	public function __construct( $gateway ) {
		// 略
	}	
	/**
	 * Build transaction args.
	 *
	 * @param WC_Order $order The order object.
	 * @return array
	 */
	public function get_transaction_args( $order ) {
		// 略
	}
	
	/**
	 * Request api and get redirect url
	 *
	 * @param WC_Order $order The order object.
	 * @return void
	 */
	public function build_request( $order ) {
		// 略
	}
	
	/**
	 * Generate the form and redirect to IronPay
	 *
	 * @param WC_Order $order The order object.
	 * @return void
	 */
	public function build_request_form( $order ) {

		$order = new \WC_Order( $order );

		try {
			?>
			<div>請稍候重新導向中...</div>
			<form method="post" id="IrpForm" action="<?php echo esc_url( 'API 請求網址' ); ?>" accept="UTF-8" accept-charset="UTF-8">
				<?php
				$fields = $this->get_transaction_args( $order );
				foreach ( $fields as $key => $value ) {
					echo '<input type="hidden" name="' . esc_html( $key ) . '" value="' . esc_html( $value ) . '">';
				}
				?>
			</form>
			<script type="text/javascript">
				document.getElementById('IrpForm').submit();
			</script>
			<?php

		} catch ( Exception $e ) {
			Utility::log( $e->getMessage() . ' ' . $e->getTraceAsString() );
		}
	}
}
```

準備好兩種傳送方式後，讓我們回到 CreditCard.php，繼續信用卡的付款實作 ```process_payment()```：

```php
<?php

namespace Irp\Gateways;

defined( 'ABSPATH' ) || exit;

/**
 * Set payment class
 */
function add_irp_credit_card() {
	class CreditCard extends \WC_Payment_Gateway {

		public function __construct() {
			// 略
		}

		public static function register() {
			// 略
		}

		public function add_gateway_class( $methods ) {
			// 略
		}

		public function init_form_fields() {
			// 略
		}

		public function process_payment( $order_id ) {
			$order      = new \WC_Order( $order_id );
			$request    = new Request( $this );
			$return_url = $request->build_request( $order );
			return array(
				'result'   => 'success',
				'redirect' => $return_url,
			);
		}
	}

	CreditCard::register();

}
add_action( 'plugins_loaded', 'Irp\Gateways\add_irp_credit_card' );
```

我們先建立了 Request 實例並將付款方式類別本身當作參數傳進去，然後使用 ```build_request()``` 來取得跳轉網址，這邊記得要先取得 WooCommerce 的 ```$order``` 訂單物件後傳給它，才能在 Request 實例中取得訂單資訊，最後返回一個付款網址，並將取得的網址指定給 redirect。

接下來是處理虛擬轉帳的付款實作，開啟 VirtualAccount.php：

```php
<?php

namespace Irp\Gateways;

defined( 'ABSPATH' ) || exit;

/**
 * Set payment class
 */
function add_irp_virtual_account() {
	class VirtualAccount extends \WC_Payment_Gateway {

		public function __construct() {
			// 略
			add_action( 'woocommerce_receipt_' . $this->id, array( $this, 'receipt_page' ) );
		}

		public static function register() {
			// 略
		}

		public function add_gateway_class( $methods ) {
			// 略
		}

		public function init_form_fields() {
			// 略
		}

		/**
		 * Process payment
		 *
		 * @param string $order_id The order id.
		 * @return array
		 */
		public function process_payment( $order_id ) {
			$order = new \WC_Order( $order_id );
			return array(
				'result'   => 'success',
				'redirect' => $order->get_checkout_payment_url( true ),
			);
		}

		/**
		 * Redirect to IronPay payment page
		 *
		 * @param WC_Order $order The order object.
		 * @return void
		 */
		public function receipt_page( $order ) {
			$request = new Request( $this );
			$request->build_request_form( $order );
		}
	}

	VirtualAccount::register();

}
add_action( 'plugins_loaded', 'Irp\Gateways\add_irp_virtual_account' );
```

需要注意的地方有三個：

1. ```process_payment()``` 指定的跳轉網址為 ```get_checkout_payment_url()```，這是指站內的付款頁面

2. 新增 ```receipt_page()``` 方法發起 Request 的前端表單傳送方法 ```build_request_form()```

3. 在建構式新增勾點 ```woocommerce_receipt_irp_virtual_account``` 來觸發 ```receipt_page()``` 方法

這邊的運作的邏輯是當消費者按下結帳按鈕後，直接跳轉去結帳完成頁，然後從結帳完成頁產生的表單來做資料傳送，這樣就不用處理 API 的回傳結果直接進行頁面跳轉判斷。

當資料傳送過去後，接下來就是要準備 WC API 來接收金流商的回傳結果，下一篇我們繼續處理接收回傳資料的部分。