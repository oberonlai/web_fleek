---
title: WooCommerce 金流串接實戰（十ㄧ）- 導入單元測試
date: 2021-09-26 16:00:00
categories:
- WooCommerce
tags:
- WooCommerce 金流
- WordPress 外掛
- WooCommerce 外掛
- 自動化測試
- 單元測試
etoc: false
---

先來回顧一下目前鐵人付外掛的資料夾結構：

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

通常我們會新增一個 tests 資料夾來放入測試的類別，並且其目錄結構會完全比照 src 裡面的所有內容，以及在既有的檔名後面加入 Test 的後綴，具體的結構如下：

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
├── tests
│   ├── Gateways
│   │   ├── CreditCardTest.php
│   │   ├── RequestTest.php
│   │   ├── ResponseTest.php
│   │   └── VirtualAccountTest.php
│   ├── OptionsTest.php
│   ├── Posts
│   │   └── ShopOrder
│   │       └── MetaboxTest.php
│   └── UtilityTest.php
└── vendor
```

在本文中會專注於測試 Utility 這個類別，這個類別主要有兩個方法，第一個是測試產生金流商驗證演算法的計算結果是否有符合預期，第二個是測試取得商店代號的格式是否正確。

<!--more-->

首先看到 ```Utility.php``` 的內容：

```php
<?php

namespace Irp;

class Utility {
	// 略

	/**
	 * Build pass code for api usage.
	 *
	 * @param string $args args.
	 * @param string $secret API secret.
	 * @return string
	 */
	public static function generate_sign( $args, $secret ) {
		$post_datastr = '';
		foreach ( $args as $name => $value ) {
			if ( ! is_array( $value ) ) {
				if ( $value !== null && $value !== '' ) {
					$post_datastr = $post_datastr . $name . '=' . $value . '&';
				}
			}
		}
		$tmp_array  = explode( '&', trim( $post_datastr, '&' ) );
		sort( $tmp_array, SORT_STRING );
		$source     = implode( '&', $tmp_array );
		$source_key = $source . '&key=' . $secret;
		$sign       = strtoupper( md5( $source_key ) );
		return $sign;
	}

	/**
	 * Get stord id
	 *
	 * @return string
	 */
	public static function get_store_id() {
		return get_option( 'irp_store_id' );
	}
}


```

這篇示範測試其中兩個靜態方法： ```generate_sign()``` 與 ```get_store_id()``` 。

為了要能夠測試，把程式碼寫得易於測試是非常重要的，易於測試的其中一個原則就是要有返回結果，有這個結果才能在測試時進行比對，同時在使用這個方法時也能預先處理沒有取得預期結果的情況。

準備好測試對象已經測試檔案後，接下來我們透過 Composer 安裝測試套件 PHPUnit，可以把它安裝在專案的目錄之下，或是可以裝在全域的環境下，這樣以後每個專案都可以直接使用 PHPUnit 的指令而無需再個別安裝，這邊介紹全域的安裝方式。

```sh
$ composer global require phpunit/phpunit
```

接下來切換到專案資料夾，執行 phpunit，看到以下訊息即代表安裝成功：

```sh
iron-pay$ phpunit

PHPUnit 9.5.4 by Sebastian Bergmann and contributors.

Usage:
  phpunit [options] UnitTest.php
  phpunit [options] <directory>

Code Coverage Options:
  --coverage-clover <file>    Generate code coverage report in Clover XML format
  --coverage-cobertura <file> Generate code coverage report in Cobertura XML format
  --coverage-crap4j <file>    Generate code coverage report in Crap4J XML format
```

首先進行 PHPUnit 的初始化：

```sh
iron-pay$ phpunit --generate-configuration
```

會看到以下 PHPUnit 詢問的問題：

```sh
Bootstrap script (relative to path shown above; default: vendor/autoload.php): 
Tests directory (relative to path shown above; default: tests): 
Source directory (relative to path shown above; default: src): 
Cache directory (relative to path shown above; default: .phpunit.cache): 

Generated phpunit.xml in /Users/oberonlai/Sites/woocommerce/wp-content/plugins/iron-pay.
Make sure to exclude the .phpunit.cache directory from version control.
```

第一個問題會需要指定 Bootstrap 的檔案路徑，這個檔案是用來做自動載入的設定以及存放一些全域變數的檔案，預設的話會是 Composer 的 ```autoload.php``` 這個檔案。

其他問題會問你是否要修改測試目錄、原始檔目錄以及快取目錄的路徑，這些都採用預設值即可，完成後記得把 ```.phpunit.cache``` 目錄在版控中忽略掉，設定完成後可以在外掛根目錄看到一個 ```phpunit.xml``` 的設定檔，之後要修改 PHPUnit 的設定都可以在這個檔案調整。

通常我們要執行測試時，需要到終端機下指令輸入 phpunit，但畢竟還要切換介面會稍微增加進行測試的阻力，如果這時候你是使用 VSCode 的話，建議可以安裝由 Recca Tsai 開發的 [PHPUnit Test Explorer](https://marketplace.visualstudio.com/items?itemName=recca0120.vscode-phpunit) 測試套件。

![](https://oberonlai.blog/wp-content/uploads/wordpress-unit-test/wordpress-unit-test-01.jpg)

它可以直接在 VSCode 的面板中管理已經寫好的測試項目，透過介面點選要執行的測試，更方便之處在於可以透過快捷鍵來進行測試，這樣可以大幅增加執行測試的效率。

VScode 還有另外一個寫測試超好用的提示工具，它是由 Winnie Lin 開發的 [PHPUnit Snippets](https://marketplace.visualstudio.com/items?itemName=onecentlin.phpunit-snippets) 套件，只要輸入關鍵字 pu:，就能自動帶入寫測試時的框架，完全不需要再自己手動輸入，尤其像我這種手殘黨特別適用。

安裝好兩款測試套件後，我們還需要更新  compose.json 檔，加入 autoload 的指定路徑，並且新增 autoload-dev 的 tests 載入，這讓 PHPUnit 可以抓到 src 裡面的類別：

```json
"autoload": {
	"psr-4": {
		"Irp\\": "src"
	}
},
"autoload-dev": {
	"psr-4": {
		"Irp\\Tests\\": "tests/"
	}
}
```

路徑是根據命名空間指定的，更新完後記得要執行 dump 指令才會重新產生自動載入的檔案：

```sh
iron-pay$ composer dump
```

準備好以上所有寫測試的環境後，我們就來開始實作 UtilityTest.php，測試的基本架構如下：

```php
<?php

namespace Irp\Tests;

use PHPUnit\Framework\TestCase;
use Irp\Utility;

/**
 * UtilityTest
 *
 * @group group
 */
class UtilityTest extends TestCase {

	protected function setUp(): void {
		parent::setUp();

		// code
	}

	protected function tearDown(): void {
		// code
	}


	public function test_generate_sign() {
		// Test
	}

	public function test_get_store_id() {
		// Test
	}

}

```

測試類別從 ```TestCase``` 繼承而來，```setUp()``` 會在執行測試方法前觸發，因此可以把要測試的類別建立以及相關變數放在這邊，那麼在測試中就可以取得這些內容。

相反的 ```tearDown()``` 則是在每一個測試跑完的當下會被觸發，如果需要在測試完後還原某些資料或是重設設定，就可以放在這個方法之中。

所有的測試方法要加入 test 前綴，並且盡量遵循 Arrange、Act、Assert 的 3A 架構：

- Arrange 指的是測試前的初始資料準備，像是預期的結果、變數的初始值等等。
- Act 是呼叫測試對象的方法，藉此取得執行該方法後的返回結果。
- Assert 斷言，也就是檢查 Act 中的返回結果跟 Arrange 裡面的預期結果是否吻合。

## 測試加密演算法


接下來我們實作 ```test_generate_sign()``` ，這邊我想測試的是當參數傳入之後，是否有產出我們所期待的演算結果。鐵人付的加密演算邏輯為傳入參數經過整理後與店家金鑰組合做 MD5 雜湊，因此金鑰應為必填欄位，測試程式碼如下：

```php
public function test_generate_sign() {
	// Arrange.
	$args = array(
		'nonce_str'       => 71669965,
		'orgno'           => 1265,
		'secondtimestamp' => 1489215551,
		'total_fee'       => 8888,
	);

	$secret   = '12345';
	$expected = 'B59951E5AA2E6FCA1596253E2978ED2B';

	// Act.
	$actual = Utility::generate_sign( $args, $secret );

	// Assert.
	$this->assertEquals( $expected, $actual );
}
```

Arrange 的部分先設定要傳入的變數，以及預期的演算結果，Act 則是執行 ```generate_sign()``` 並將結果存在 $actual 變數中，最後的 Assert 則是檢查這兩個變數是否有符合預期結果，```assertEquals()``` 是判斷傳入的兩個變數值是否相等。

接下來切換到 VSCode 左側面板找到燒杯的圖案，就能看到一個測試項目，這時候點選上面的播放圖示或是用快捷鍵輸入 Cmd + t + s 就能執行測試，記得 t 跟 s 不用同時，只要按住 Cmd 先按 t 再按 s 即可，也可以點擊下圖中間箭頭處的 Run 來執行：

![](https://oberonlai.blog/wp-content/uploads/wordpress-unit-test/wordpress-unit-test-02.jpg)

順利的話就能在下方的 Output 視窗看到測試結果，當然，你也可以在終端機輸入 phpunit 來執行測試。

```sh
PHPUnit 9.5.9 by Sebastian Bergmann and contributors.

Runtime:       PHP 7.4.13
Configuration: /Users/oberonlai/Sites/woocommerce/wp-content/plugins/iron-pay/phpunit.xml

R                                                                   1 / 1 (100%)

Time: 00:00.016, Memory: 6.00 MB

There was 1 risky test:

1) Irp\Tests\UtilityTest::test_generate_sign
This test does not have a @covers annotation but is expected to have one

OK, but incomplete, skipped, or risky tests!
Tests: 1, Assertions: 1, Risky: 1.
```

這邊我們會發現雖然測試通過了，但出現一個提示訊息說：This test does not have a @covers annotation but is expected to have one，這代表我們沒有這個方法沒有標記要測試哪些項目，更多關於 annotation 的用法可以參考官方文件。

這邊我們先忽略它，要關閉這個提示的話開啟 phpunit.xml，找到 ```forceCoversAnnotation``` 設定為 false 即可：

```xml
forceCoversAnnotation="false"
```

再執行一次測試就可以看到通過的訊息：

```sh
PHPUnit 9.5.9 by Sebastian Bergmann and contributors.

Runtime:       PHP 7.4.13
Configuration: /Users/oberonlai/Sites/woocommerce/wp-content/plugins/iron-pay/phpunit.xml

.                                                                   1 / 1 (100%)

Time: 00:00.018, Memory: 6.00 MB

OK (1 test, 1 assertion)
```


## 測試取得商店代號

第二個我們要測試的是 ```get_store_id()```，這方法我們要測試的是從資料庫取得的商店代號的位數是否為 8 碼，以及是否皆整數。先回到這個方法的實作：

```php
public function get_store_id(){
	return get_option( 'irp_payment_orgno' );
}
```

可以看到裡面只有一個 ```get_option()```，這個方法是 WordPress 取得 wp_options 資料表的函式，但在這邊我們只是要驗證 ```get_store_id()``` 返回的資料是否正確，而非 ```get_option()``` 是否能正確的從資料庫取得資料，再加上我們要隔離外部環境也就是資料庫的操作，因此我們不能直接呼叫 WordPress 的函式，而是要用模擬的方式來做出假的結果，藉此來確保我們自己寫出來的程式是正確的。

而模擬的方法叫做測試分身 - Test Doubles，我們這邊使用 PHPMock 來模擬 ```get_option()```，先透過 Composer 來進行安裝，記得要加 --dev 來指定該套件只用開發環境：

```sh
iron-pay$ composer require --dev php-mock/php-mock-phpunit
```

然後使用 PHPMock 來模擬 ```get_option()```：

```php
namespace UnitTestDemo;

use phpmock\phpunit\PHPMock;

class UtilityTest extends TestCase {

    use PHPMock;
	
	// 略

    public function test_get_store_id() {
		// Arrange.
		$get_option = $this->getFunctionMock( 'Irp', 'get_option' );
		$get_option->expects( $this->once() )
					->with( $this->equalTo( 'irp_payment_orgno' ) )
					->willReturn( 'mystoreid' );

		$expected = 'mystoreid';

		// Act.
		$actual = Utility::get_store_id();

		// Assert.
		$this->assertEquals( $expected, $actual );
	}
}
```

變數 ```$get_option``` 就是模擬出來的對象，它透過 ```getFunctionMock()``` 來建立分身，第一個參數傳的是命名空間，第二個是要模擬的函式名稱，接下來呼叫 expects、will、willReturn 三個方法，第一個 expects 的參數為 ```$this->once()```，代表在這個測試中只會呼叫一次我們自己做出來的 ```get_option()```。

第二個 with 代表要傳入這個假方法的參數，也就是原本方法的裡面的 ```return get_option( 'irp_store_id' )``` 的 ```irp_store_id``` 欄位名稱，第三個 willReturn 代表的是這個假方法會回傳的結果，也就是商店代號，最後就可以檢查是否吻合。

測試寫到這邊，會發現到測試對象 ```get_store_id()``` 並不完整，它既沒有檢查資料是否有正確取得以及格式是否正確，而這也是寫測試的最大好處，為了要能夠寫測試，會思考到當下沒有考慮到的情境，我們把 ```get_store_id()``` 重構如下：

```php
public function get_store_id(){
	$store_id = get_option( 'irp_store_id' );
	if ( empty( $store_id ) ) {
		return '未輸入商店代號';
	}

	if ( ! is_numeric( $stord_id ) ) {
		return '商店代號限定數字';
	}

	if ( strlen( $stord_id ) !== 8 ) {
		return '商店代號須為 8 碼';
	}
	
	return $store_id;
}
```

回到測試裡面，將原本的 ```$get_option``` 修改為執行四次，並且每次都回傳不同的結果：

```php
class DemoTest extends \PHPUnit_Framework_TestCase
{
    use PHPMock;

    public function test_get_store_id()
    {
        $get_option = $this->getFunctionMock( 'Irp', 'get_option' );
		$get_option->expects( $this->exactly( 4 ) )
					->with( $this->equalTo( 'irp_payment_orgno' ) )
					->willReturnOnConsecutiveCalls(
						'12345678',
						'',
						'abc123',
						'123'
					);
		$expected        = '12345678';
		$expected_empty  = '未輸入商店代號';
		$expected_number = '商店代號限定數字';
		$expected_length = '商店代號須為 8 碼';

		// Act.
		$actual        = Utility::get_store_id();
		$actual_empty  = Utility::get_store_id();
		$actual_number = Utility::get_store_id();
		$actual_length = Utility::get_store_id();

		// Assert.
		$this->assertEquals( $expected, $actual );
		$this->assertEquals( $expected_empty, $actual_empty );
		$this->assertEquals( $expected_number, $actual_number );
		$this->assertEquals( $expected_length, $actual_length );
    }
}
```

原本使用 ```willReturn()``` 來模擬傳回一個結果，現在改用 ```willReturnOnConsecutiveCalls()``` 傳回四個，第一個是正確的格式，另外三個是例外情況，然後分別對應到在例外情況下 ```test_get_store_id()``` 會回傳的結果。

除了使用 PHPMock 來模擬內建的函式外，在社群中也有提供針對 WordPress 使用的測試替身，像是 [WP_Mock](https://github.com/10up/wp_mock) 以及 [Brain Mockey](https://brain-wp.github.io/BrainMonkey/)，PHP 也有一套專門的測試框架叫做 [CodeCeption](https://codeception.com)，而用在 WordPress 上面的叫做 [wp-browser](https://wpbrowser.wptestkit.dev)，另外還有針對 WooCommerce 的 [wp-browser-woocommerce](https://github.com/level-level/wp-browser-woocommerce)，這些工具讓我們可以更無痛的導入測試流程。

寫測試的價值在於讓我們想到原本沒有想到的狀況，進而來改善原始程式碼對於例外情況的處理，這也是為什麼寫了測試反而可以省下除錯時間，因為當例外狀況都在我們的掌控之中，要追查問題出在哪裏就會很快，省去很多摸索的時間。

在業界流行一種測試驅動開發 ( Test-Driven Development ) 的流程，先把測試寫完再去真正實作功能的開發，寫測試的當下就能先思考這個類別該如何使用以及可能會有的例外情況，把一切都考慮進去想清楚後再來動手，就能減少很多修改程式的時間。

但要測試開發的功能能否在真實世界正常運作，我們還需要整合測試與端對端測試，下一篇我們先介紹端對端測試，也就是透過模擬使用者的操作行為來進行測。