---
title: WooCommerce 客製化商品外掛
date: 2021-11-12 11:00:00
categories:
- WooCommerce
- WooCommerce 外掛
etoc: false
---


上週遇到一個很有趣的需求，客戶來信說希望可以在商品頁面增加一個文字欄位輸入框，當顧客輸入數字時可以改變商品售價，這位客戶的網站是販售某種電線還纜線之類的產品，除了會依照線的材質會有不同的售價外，還會根據線的長度來計價。

具體的需求是他們希望當顧客輸入 1~100 這個區間的任意數字時，商品售價可以依照公式計算出除了基本規格的售價外，再將長度變數加入最後價格中，假設當商品售價初始價格為 100 元，輸入 10 會變成 110 元，輸入 50 會變成 150 元，有點類似客製化商品的作法。

<!--more-->

一開始我的思路是從變化商品下手，想說能否開一個屬性是可以動態決定這個長度的價格，而當這個屬性被選擇時，就能多一個文字輸入框來提供顧客輸入任意的數字，但研究後發現變化商品的售價就是一個固定的欄位，沒有辨法進行動態的改變。

於是又想說直接在前端根據輸入文字欄位來修改商品定價，但跟變化商品的價格一樣，只要售價在後台設定後它就是一個固定的值，任何的變動都會影響到訂單的總金額，弄不好可能還會影響到其他訂單，既然直接修改商品價格不可行，我就朝著訂單附加費用的角度來思考，可能是把長度的計算加總在運費或是稅務上，但這樣一樣會讓運費與稅務大亂。

後來想起關鍵字 Dynamic Product Price，但找到的多半是像買兩件折 50，或是買三件打八折這種行銷用途的外掛，但從他們的作法中我可以確信使用附加費用解決這個需求是正確的方向，最後我找到了 [Advanced Product Fields (Product Addons) for WooCommerce](https://wordpress.org/plugins/advanced-product-fields-for-woocommerce/) 這隻外掛。

![](https://oberonlai.blog/wp-content/uploads/woocommerce-product-dynamic-price/woocommerce-product-dynamic-price-01.jpg)

這支外掛的主要功能是可以增加商品在前台的欄位，免費版本可以增加九種欄位在商品頁面中，付費版還有更多欄位可以選擇，最重要的是它可以根據欄位的輸入結果來加入訂單的附加費用，完完全全可以解決客戶的需求。

![](https://oberonlai.blog/wp-content/uploads/woocommerce-product-dynamic-price/woocommerce-product-dynamic-price-02.jpg)

但免費版本的商品附加費用，只能是固定費率，也就是判斷有輸入這個長度欄位的話，附加費用是固定金額，升級付費版的話就可以依照輸入的輸字來決定金額，甚至還能寫入自己的公式來進行計算，操作介面如果你有使用過 ACF 的話，這一套完全可以無痛上手，想試用的話在他們的外掛說明頁上都有提供展示連結，或是直接下載玩看看最快。

![](https://oberonlai.blog/wp-content/uploads/woocommerce-product-dynamic-price/woocommerce-product-dynamic-price-03.jpg)

我進一步研究他們的程式碼，想了解附加費用的機制是如何運作的，發現到幾支關鍵的勾點可以使用：

### woocommerce_add_cart_item_data
用於修改自行新增的商品自訂欄位到結帳購物車中，讓購物車可以讀到商品的自訂欄位，可以傳入三個參數：\$cart_item_data、\$product_id 以及 \$variation_id，回傳 \$cart_item_data

```php
// define the woocommerce_add_cart_item_data callback 
function filter_woocommerce_add_cart_item_data( $cart_item_data, $product_id, $variation_id ) { 
    // make filter magic happen here... 
    return $cart_item_data; 
}; 
         
// add the filter 
add_filter( 'woocommerce_add_cart_item_data', 'filter_woocommerce_add_cart_item_data', 10, 3 ); 
```

### woocommerce_get_item_data
用於取得已經儲存的商品自訂欄位，可以傳入兩個參數：\$item_data 以及 \$cart_item，回傳 \$item_data，透過這個勾點可以把長度的規格顯示在結帳頁的商品清單中：

```php
// define the woocommerce_get_item_data callback 
function filter_woocommerce_get_item_data( $item_data, $cart_item ) { 
    // make filter magic happen here... 
    return $item_data; 
}; 
         
// add the filter 
add_filter( 'woocommerce_get_item_data', 'filter_woocommerce_get_item_data', 10, 2 ); 
```


### woocommerce_before_calculate_totals
用於計算訂單金額，訂單的總金額可以針對商品自訂欄位進行加總靠得就是它，下面為這支外掛外掛處理訂單金額計算的方法：

```php
public function adjust_cart_item_pricing($cart_obj) {

	if (is_admin() && ! defined('DOING_AJAX'))
		return;

	foreach( $cart_obj->get_cart() as $key=>$item ) {

		if(empty($item['wapf']))
			continue;

		$quantity = empty($item['quantity']) ? 1 : wc_stock_amount($item['quantity']);
		$product = wc_get_product($item['variation_id'] ? $item['variation_id'] : $item['product_id']);
		$base = Helper::get_product_base_price($product);
		$options_total = 0;

		foreach ($item['wapf'] as $field) {
			if(!empty($field['price'])) {
				foreach ($field['price'] as $price) {

					if($price['value']  === 0)
						continue;

					$options_total = $options_total + Fields::do_pricing($price['value'],$quantity);

				}
			}

		}

		if($options_total > 0)
			$item['data']->set_price($base + $options_total);

	}
	
}
```

它先檢查購物車裡面的商品是否帶有價格的自訂欄位，有的話就透過 Fields 類別的靜態方法來取得附加費用的計算邏輯，最後再把原始價格 $base 加上算出來的費用，使用 WooCommerce 的 ```set_price()``` 來做為訂單總價。

結合以上三個勾點我們就能搞懂這支外掛基本運作邏輯：先將商品自訂欄位寫入，然後在前台的商品頁加入前端表格，透過 JS 計算出附加費用後，再去修改購物車的商品總金額，藉此達成根據顧客手動輸入的結果來改變訂單總價。

另外值得一提的是 Advanced Product Fields 的設定頁面做得很好，除了比照 ACF 的設定邏輯外，整個資料夾結構也非常清楚，未來如果要開發自訂欄位的操作介面，這支外掛是非常好的參考教材。

補充：[Measurement Price Calculator](https://woocommerce.com/products/measurement-price-calculator/) 也可以達成類似的功能