---
title: WooCommerce 金流串接實戰（九）- 接收回傳資料
date: 2021-09-24 09:30:00
categories:
- WooCommerce
tags:
- WooCommerce 金流
- WordPress 外掛
- WooCommerce 外掛
etoc: false
---

完成付款請求之後，接下來是準備好接收金流商回傳資訊的 Response 類別，目前外掛的資料夾結構如下：

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

開啟 Response.php 檔案，輸入以下程式碼：

<!--more-->

```php
<?php
namespace Irp\Gateways;

use Irp\Utility;

defined( 'ABSPATH' ) || exit;

/**
 * Receive response from ironpay.
 */
class Response {

	/**
	 * Initialize and add hooks
	 *
	 * @return void
	 */
	public static function init() {

		// Online Payment listener.
		add_action( 'woocommerce_api_iron-pay', array( __CLASS__, 'receive_response' ) );
		add_action( 'ironpay_online_response', array( __CLASS__, 'valid_response' ) );

		// Offline Payment listener.
		add_action( 'woocommerce_api_iron-pay-offline', array( __CLASS__, 'receive_response' ) );
		add_action( 'ironpay_offline_response', array( __CLASS__, 'valid_response_offline' ) );

	}
}

Response::init();
```

首先，我們用靜態方法 ```init()```來初始化勾點的部分，勾點一共分為兩組，分別是即時付款 Online Payment 以及非即時付款 Offline Payment。每一組又個別有兩個勾點，我們先看 Online Payment 這一組，第一個勾點是 WooCommerce API 所提供的，也就是前面我們有提到的動態勾點：

```php
add_action( 'woocommerce_api_iron-pay', array( __CLASS__, 'receive_response' ) );
```

這代表當我們的造訪 ```https://woocommerce.test/wc-api/iron-pay/``` 會觸發 ```receive_response``` 這個方法，這個方法的主要功能為驗證當請求這個網址時有接收到金流商回傳的參數，並且通過加密演算法的驗證。

第二個勾點為 ```ironpay_online_response```，當看到有我們自己命名前綴的勾點時，就代表這是我們自己新增的，新增的方法稍後會實作。

```php
add_action( 'ironpay_online_response', array( __CLASS__, 'valid_response' ) );
```

這個勾點會在通過我們的請求來源安全性驗證後觸發 ```valid_response()```，該方法實作更新訂單資料、改變庫存狀態、以及將回傳資料寫入資料庫後進行頁面跳轉等行為。

有了 Online Payment 勾點的理解之後，Offline Payment 也是一樣的邏輯，設定好回傳網址後，我們就能在請求類別 Request 裡面將這兩個網址作為參數傳給金流商，通知他們當消費者完成後結帳後會呼叫這兩個網址。

通常金流商回有兩種呼叫方式，一種是前景呼叫，也就是透過消費者的瀏覽器跳轉到我們提供的網址，不管是即時或是非即時付款，都需要讓消費者回到原站會比較合理。

另一種是背景呼叫，常見於非即時付款，因為消費者已經走完結帳流程關閉瀏覽器了，當消費者隔天完成轉帳，需要靠金流商的主機來發起付款完成通知，這種請求也就是俗稱的背景呼叫。

不同金流商對於這兩種呼叫方式的參數命名都不太相同，前景呼叫有可能叫 ```return_url``` 或是 ```back_url```，背景呼叫有可能叫做 ```notify_url``` 或是 ```callback_url```，因為這些名稱都無法很直觀的分辨到底是前景還是背景，所以詳讀文件的參數名稱以及流程圖會比較保險些，不然只根據參數名稱來傳很容易搞錯。

接下來是實作 ```receive_response()``` 方法：

```php
<?php
namespace Irp\Gateways;

use Irp\Utility;

defined( 'ABSPATH' ) || exit;

/**
 * Receive response from ironpay.
 */
class Response {

	/**
	 * Initialize and add hooks
	 *
	 * @return void
	 */
	public static function init() {
		// 略
	}
	
	/**
	 * Receive response from ironpay
	 *
	 * @return void
	 */
	public static function receive_response() {
		if ( ! empty( $_REQUEST ) ) {
			$params = wc_clean( wp_unslash( $_REQUEST ) );
			$args   = array(
				'authcode'        => $params['authcode'],
				'nonce_str'       => $params['nonce_str'],
				'orderdate'       => $params['orderdate'],
				'orgno'           => $params['orgno'],
				'out_trade_no'    => $params['out_trade_no'],
				'periods'         => $params['periods'],
				'result'          => $params['result'],
				'secondtimestamp' => $params['secondtimestamp'],
				'status'          => $params['status'],
				'total_fee'       => $params['total_fee'],
			);

			$sign = Utility::generate_sign( $args, Utility::get_secret() );
			// $params['sign'] 是金流商回傳的，把它拿來跟我們自己算出來的 $sign 做比對
			if ( $sign === $params['sign'] ) {
				if ( current_action() === 'woocommerce_api_iron-pay' ) {
					do_action( 'ironpay_online_response', $params );
				} elseif ( current_action() === 'woocommerce_api_iron-pay-offline' ) {
					do_action( 'ironpay_offline_response', $params );
				}
			}
		}
	}
}

Response::init();
```

首先我們先檢查是否有回傳參數，因為不同金流商有可能會用 ```$_POST``` 或是 ```$_GET``` 回傳，所以用 ```$_REQUEST``` 可以通用在不同專案之中，```wc_clean()``` 跟 ```wp_unslash()``` 是 WooCommerce 內建的函式，用來消除斜線或是空格等字元，我們把回傳的資料組成一個陣列，在建立勾點之前，我們先要判斷回傳的演算法是否有符合我們自己算出來的結果，藉此來確保這次的請求是由金流商呼叫。

我們先用 Utility 類別來算出我們的 sign，計算的方法為傳入所有參數加上商店金鑰，算好後再用它來比對金流商的 sign 是否吻合，如果正確的話才會建立勾點。 ```current_action()``` 是判斷該方法是在哪個勾點被執行，```do_action()``` 則是建立勾點，第一個參數為勾點名稱，第二個為使用該勾點的函式可以拿到的變數。

接下來是做訂單處理的 ```valid_response()``` 方法：

```php
<?php
namespace Irp\Gateways;

use Irp\Utility;

defined( 'ABSPATH' ) || exit;

/**
 * Receive response from ironpay.
 */
class Response {

	/**
	 * Initialize and add hooks
	 *
	 * @return void
	 */
	public static function init() {
		// 略
	}
	
	/**
	 * Receive response from ironpay
	 *
	 * @return void
	 */
	public static function receive_response() {
		// 略
	}
	
	/**
	 * Receive online payment
	 *
	 * @param array $params The post data received from ironpay.
	 * @return void
	 */
	public static function valid_response( $params ) {
		global $woocommerce;
		$order = new \WC_Order( $params['out_trade_no'] );
		if ( $order ) {
			if ( '0000' === $params['status'] ) {
				$order->payment_complete();
				$order->reduce_order_stock();
			} else {
				$order->update_status( 'on-hold' );
			}
			$woocommerce->cart->empty_cart();
			update_post_meta( $order->get_id(), '_irp_resp_code', $params['status'] );
			update_post_meta( $order->get_id(), '_irp_resp_result', $params['result'] );
			$order->add_order_note( '鐵人付交易結果：' . $params['result'], true );
			wp_safe_redirect( $order->get_checkout_order_received_url() );
			exit;
		}
	}
}

Response::init();
```

我們先取得全域變數 ```$woocommerce```，這個變數等下可以來操作購物車，接著是用金流商回傳的訂單 ID 來建立訂單物件，訂單物件有很多方法提供給我們修改訂單資料。```$params['status']``` 為金流商回傳的交易代號，通常每個代號會代表不同的結果，所以根據金流文件來判斷當代號為 0000 成功時，執行以下的動作：

```php
$order->payment_complete();
$order->reduce_order_stock();
$woocommerce->cart->empty_cart();
update_post_meta( $order->get_id(), '_irp_resp_code', $params['status'] );
update_post_meta( $order->get_id(), '_irp_resp_result', $params['result'] );
$order->add_order_note( '鐵人付交易結果：' . $params['result'], true );
```

分別是將訂單狀態改完完成、減少庫存、清空購物車，以及更新我們的自訂欄位 ```_irp_resp_code``` 與 ```_irp_resp_result```，最後的 ```add_order_note()``` 方法是新增訂單備註，回傳資料要做哪些事會根據客戶需求而有所不同，但大致上不外乎是以上幾種流程。

處理完訂單後則使用 ```wp_safe_redirect()``` 指定跳轉頁面，```$order->get_checkout_order_received_url()``` 為完成購買頁，至於背景呼叫 ```valid_response_offline()``` 也差不多，主要差別在於不需做頁面跳轉，以及可能會需要返回金流商指定的內容：

```php
<?php
namespace Irp\Gateways;

use Irp\Utility;

defined( 'ABSPATH' ) || exit;

/**
 * Receive response from ironpay.
 */
class Response {

	/**
	 * Initialize and add hooks
	 *
	 * @return void
	 */
	public static function init() {
		// 略
	}
	
	/**
	 * Receive response from ironpay
	 *
	 * @return void
	 */
	public static function receive_response() {
		// 略
	}
	
	/**
	 * Receive online payment
	 *
	 * @param array $params The post data received from ironpay.
	 * @return void
	 */
	public static function valid_response( $params ) {
		// 略
	}
	
	/**
	 * Receive offline payment
	 *
	 * @param array $params The post data received from ironpay.
	 * @return void
	 */
	public static function valid_response_offline( $params ) {
		global $woocommerce;
		$order = new \WC_Order( $params['out_trade_no'] );
		if ( $order ) {
			// 略
			wp_send_json( 'success' );
			exit;
		}
	}
}

Response::init();
```

上述 ```wp_send_json()```為 WordPress 內建的函式，主要功能是輸出 JSON 字串，假設金流商那邊希望當呼叫我們的 API 網址時，如果有成功接收的話回傳 success 字串，那我們就可以寫成 ```wp_send_json( 'success' );``` 來符合金流商的要求。

完整的 Response 類別程式碼如下：

```php
namespace Irp\Gateways;

use Irp\Utility;

defined( 'ABSPATH' ) || exit;

/**
 * Receive response from ironpay.
 */
class Response {

	/**
	 * Initialize and add hooks
	 *
	 * @return void
	 */
	public static function init() {

		// Online Payment listener.
		add_action( 'woocommerce_api_iron-pay', array( __CLASS__, 'receive_response' ) );
		add_action( 'ironpay_online_response', array( __CLASS__, 'valid_response' ) );

		// Offline Payment listener.
		add_action( 'woocommerce_api_iron-pay-offline', array( __CLASS__, 'receive_response' ) );
		add_action( 'ironpay_offline_response', array( __CLASS__, 'valid_response_offline' ) );

	}

	/**
	 * Receive response from ironpay
	 *
	 * @return void
	 */
	public static function receive_response() {

		if ( ! empty( $_REQUEST ) ) {
			$params = wc_clean( wp_unslash( $_REQUEST ) );
			$args   = array(
				'authcode'        => $params['authcode'],
				'nonce_str'       => $params['nonce_str'],
				'orderdate'       => $params['orderdate'],
				'orgno'           => $params['orgno'],
				'out_trade_no'    => $params['out_trade_no'],
				'periods'         => $params['periods'],
				'result'          => $params['result'],
				'secondtimestamp' => $params['secondtimestamp'],
				'status'          => $params['status'],
				'total_fee'       => $params['total_fee'],
			);

			$sign = Utility::generate_sign( $args, Utility::get_secret() );

			if ( $sign === $params['sign'] ) {
				if ( current_action() === 'woocommerce_api_iron-pay' ) {
					do_action( 'ironpay_online_response', $params );
				} elseif ( current_action() === 'woocommerce_api_iron-pay-offline' ) {
					do_action( 'ironpay_offline_response', $params );
				}
			}
		}
	}

	/**
	 * Receive online payment
	 *
	 * @param array $params The post data received from ironpay.
	 * @return void
	 */
	public static function valid_response( $params ) {
		global $woocommerce;
		$order = new \WC_Order( $params['out_trade_no'] );
		if ( $order ) {
			if ( '0000' === $status ) {
				$order->payment_complete();
				$order->reduce_order_stock();
			} else {
				$order->update_status( 'on-hold' );
			}
			$woocommerce->cart->empty_cart();
			update_post_meta( $order->get_id(), '_irp_resp_code', $params['status'] );
			update_post_meta( $order->get_id(), '_irp_resp_result', $params['result'] );
			$order->add_order_note( '鐵人付交易結果：' . $params['result'], true );
			wp_safe_redirect( $order->get_checkout_order_received_url() );
			exit;
		}
	}

	/**
	 * Receive offline payment
	 *
	 * @param array $params The post data received from ironpay.
	 * @return void
	 */
	public static function valid_response_offline( $params ) {
		global $woocommerce;
		$order = new \WC_Order( $params['out_trade_no'] );
		if ( $order ) {
			if ( '0000' === $status ) {
				$order->payment_complete( $buysafe_no );
				$order->reduce_order_stock();	
			}
			$woocommerce->cart->empty_cart();
			update_post_meta( $order->get_id(), '_irp_resp_code', $params['status'] );
			update_post_meta( $order->get_id(), '_irp_resp_result', $params['result'] );
			$order->add_order_note( '鐵人付交易結果：' . $params['result'], true );
			wp_send_json( 'success' );
			exit;
		}
	}
}

Response::init();
```

這樣就算是完成接收金流商回傳資料的工作了，剩下的就是根據金流商的規格去做調整，WooCommerce 金流串接就能大功告成了，但在交給客戶測試驗收前，我們先自己來做單元測試，下一篇來介紹如何寫測試以及說明應該要測試哪些類別與方法。


