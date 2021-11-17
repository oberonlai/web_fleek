---
title: 【 心得 】WordPress 外掛開發日記 (二) - 套件
date: 2021-07-26 12:30:00
categories:
- 外掛開發
tags:
- composer
- customPostType
- metabox
- option
- asset
- ajax
- router
- packagist
etoc: false
---

### WordPress 外掛開發系列文目錄

[一、WordPress 外掛架構 - 紀錄曾經使用過的架構以及目前作法](https://oberonlai.blog/tw/wordpress-plugin-boilerplate)
[二、WordPress 開發實用 Composer 套件 - 介紹在寫外掛的時候可以加速開發的套件](https://oberonlai.blog/tw/wordpress-and-composer)
三、載入佈景主題範本 - 如何在外掛裡面使用範本檔修改前端介面並整合前端開發流程
四、整合 PHPUnit 單元測試 - 避免改東牆壞西牆的問題再次發生
五、自動化部署 - 讓 WordPress 搭上 DevOps 的列車🚄
六、WordPress 外掛更新 - 自行管理未上架 wordpress.org 的外掛版本維護


## 前言

WP 最強大的部分就是有用不完的外掛，PHP 當然也有！上 Packagist 或 Github 打 WordPress 關鍵字就能找到非常多適用於 WP 開發的套件，以下就幾套我已經拿來實戰的套件做介紹：


## posttypes

做外掛最常見的需求就是要新增 custom post type、custom taxonomy，或是修改既有 post type 的顯示名稱以及功能修改，WP 原生的方法事實上不難使用，只要呼叫 register_post_type() 掛在 init 就能處理，

但隨著專案的複雜度提高，類似的程式碼就需要被重複很多遍，再加上還需要處理後台 column 的顯示欄位，新增一個 post type 就會讓程式碼落落長，萬一有兩個或更多個，管理起來就會麻煩許多。

<!--more-->

爬了一下 Github 後，發現由 Joe Grainger 寫的 [PostTypes](https://github.com/jjgrainger/PostTypes) 這個套件，完完全全可以解決我的問題，因為他都幫我們把功能封裝好了，只要 new PostType('CPT') 就可以新增，完全不用再寫 Hook，並透過該物件來實作 Post Type 的各項參數設定：

先來秒安裝一下：
```
$ composer require jjgrainger/posttypes
```

我的習慣是會在 src 資料夾底下開一個與 CPT slug 同名的 php 檔，然後因為我們已經有自動載入了，所以要用的話只需要寫 use 關鍵字即可，假設我們的需求如下：

1. 新增一個 Post Type 叫做 book
2. 新增兩個 book 的 Taxonomy 叫做 travel 跟 business

### 新增 Custom Post Type

根據資料結構，我們會處理到 wp_posts 與 wp_terms，所以資料夾先新增 posts 與 terms，然後建立相對應的檔案，檔案結構如下：

```
src
├── posts
│   └── book
│       └── Book.php
└── terms
    ├── business
    │   └── Business.php
    └── travel
        └── Travel.php
```

我自己常常會被英文的單複數搞混，所以後來就決定第一層比照資料表名稱一律使用複數 ( posts、terms )，新增的內容或是功能一律都使用單數，然後檔名首字均大寫。接下來先開啟 Book.php 來進行 Custom Post Type 的註冊，我們使用 posttypes 套件來處理：

```php
use PostTypes\PostType;
$books = new PostType('book');
$books->register();
```

兩行就搞定！ 😍

![](https://oberonlai.blog/wp-content/uploads/wordpress-and-composer/wordpress-and-composer-01.jpg)

預設名稱或是用 CPT 的名字加上 s 複數，如果要修改的話可以在建立 PostType 實例時帶入 name 參數來進行修改：

```php
use PostTypes\PostType;

$name = array(
	'name'   => 'book',  // 註冊 CPT 的名稱
	'plural' => __( 'MyBook', 'btp' ), // 顯示名稱
	'slug'   => 'mybook', // 網址代稱
);

$books = new PostType( $name );
$books->register();
```

結果如下：

![](https://oberonlai.blog/wp-content/uploads/wordpress-and-composer/wordpress-and-composer-02.jpg)


如果想要使用 register_post_type 裡面的參數，只要再傳入第二個 option 參數即可：

```php

use PostTypes\PostType;

$options = array(
	'has_archive' => true,
	'menu_icon'   => 'dashicons-book',
	'supports'    => array( 'title', 'revisions', 'editor', 'author', 'comments', 'custom-fields' ),
);

$books = new PostType( $name, $options );
$books->register();
```

另外如果想要修改標籤像是 Add New、Edit 等等的字串，只要在 register 完成後指定即可：

```php
$labels = array(
    'all_items'          => __( '所有好書', 'btp' ),
    'add_new'            => __( '新增好書', 'btp' ),
    'add_new_item'       => __( '新增好書', 'btp' ),
    'edit_item'          => __( '編輯', 'btp' ),
    'new_item'           => __( '新項目', 'btp' ),
    'view_item'          => __( '檢視', 'btp' ),
    'view_items'         => __( '檢視', 'btp' ),
    'search_items'       => __( '搜尋', 'btp' ),
    'not_found'          => __( '無結果', 'btp' ),
    'not_found_in_trash' => __( '無結果', 'btp' )
);

$books = new PostType( $name, $options );
$books->register();
$books->labels( $labels );
```

![](https://oberonlai.blog/wp-content/uploads/wordpress-and-composer/wordpress-and-composer-03.jpg)

Book.php 完整程式碼如下：

```php
<?php

use PostTypes\PostType;

$name = array(
	'name'   => 'book',
	'plural' => __( 'MyBook', 'btp' ),
	'slug'   => 'mybook',
);

$options = array(
	'has_archive' => true,
	'menu_icon'   => 'dashicons-book',
	'supports'    => array( 'title', 'revisions', 'editor', 'author', 'comments', 'custom-fields' ),
);

$books = new PostType( $name, $options );
$books->register();

$labels = array(
	'all_items'          => __( '所有好書', 'btp' ),
	'add_new'            => __( '新增好書', 'btp' ),
	'add_new_item'       => __( '新增', 'btp' ),
	'edit_item'          => __( '編輯', 'btp' ),
	'new_item'           => __( '新項目', 'btp' ),
	'view_item'          => __( '檢視', 'btp' ),
	'view_items'         => __( '檢視', 'btp' ),
	'search_items'       => __( '搜尋', 'btp' ),
	'not_found'          => __( '無結果', 'btp' ),
	'not_found_in_trash' => __( '無結果', 'btp' ),
);

$books->labels( $labels );
```

上面這段就是我平常在用的程式碼片段。

可能會有朋友問這跟直接寫 register_post_type() 有什麼差別？就設計典範來說這是程序式程式設計 ( Procedural Programming ) 跟物件導向程式設計 ( Object Oriented Programming ) 的差別，WordPress 主要使用的邏輯是前者，而一些 PHP 框架像是 Laravel 就是物件導向的方式來設計的。

程序式程式設計顧名思義就是按照程式碼的順序來執行，然後透過判斷式來決定下一條路該怎麼走，有點像小時候玩的爬梯子的遊戲，遇到橫線就要轉彎，沒有的話就直直前進，在 WordPress 中最常見的例子就是要取得文章列表時使用的迴圈寫法：

```php
if( have_posts() ){
    while( have_posts() ){
        the_post();
        the_content();
    };
}
```

先檢查 have_posts() 是否有回傳東西，有的話用 while 迴圈去把裡面的文章顯示出來，直到沒有為止，註冊 CPT 也是按照程式碼順序的邏輯，先把參數傳到 register_post_type 這個函式裡面，再把這個函式傳到 init 的勾點 ( Hook ) 上：

```php
function wpdocs_codex_custom_init() {
    $args = array(
        'public' => true,
        'label'  => __( 'Books', 'textdomain' ),
    );
    register_post_type( 'book', $args );
}
add_action( 'init', 'wpdocs_codex_custom_init' );
```

而物件導向比較像是在玩樂高，所有的積木都有自己的特性，透過組合這些積木來做成成品，同時拆掉後又能再用一樣的積木組成不同的東西，這就是物件的核心概念，也就是像上面的例子一樣，我使用了 PostTypes\\PostType 這塊積木來完成註冊 CPT 的這個成品：

```php
use PostTypes\PostType;
$books = new PostType('42115');
$books->register();
```

而 Packagist 就是全世界最大的積木池，我可以在裡面撿到各式各樣我需要的磚，然後拼成我想要的作品，有別於程序式程式設計，透過物件的介面更方便重複利用，也能減少找相對應勾點的時間。

所以我現在刻意使用這些套件來取代 WP 內建的函式，覺得這樣比較好理解並且相對簡潔，但如果外掛功能相對單純或是主機環境因素無法使用 Composer，register_post_type() 還是無敵好用的XD

### 新增 Taxonomy

接下來處理書的分類也就是旅遊與商業這兩個類別，開啟 Business.php 輸入以下程式碼：

```php
use PostTypes\Taxonomy;

$business = new Taxonomy( 'business' );
$business->posttype( 'book' );
$business->register();
```

一樣，又是俐落的三行就解決，只要宣吿 Taxonomy 實例後再用 posttype 方法去指定 Custom Post Type 就能完成。

![](https://oberonlai.blog/wp-content/uploads/wordpress-and-composer/wordpress-and-composer-04.jpg)

然後最美好之處在於設定 Taxonomy 的方式跟 Custom Post Type 一模一樣，這支類別的介面設計得真的很好，非常方便使用，看下面新增 Taxonomy 的程式碼即可明白：

```php
use PostTypes\Taxonomy;

$name = array(
	'name'   => 'business',
	'plural' => __( 'MyBusinessBook', 'btp' ),
);

$options = array(
	'hierarchical' => false,
);

$business = new Taxonomy( $name, $options );
$business->posttype( 'book' );
$business->register();

$labels = array(
	'all_items'          => __( '所有', 'btp' ),
	'add_new'            => __( '新增', 'btp' ),
	'add_new_item'       => __( '新增', 'btp' ),
	'edit_item'          => __( '編輯', 'btp' ),
	'new_item'           => __( '新項目', 'btp' ),
	'view_item'          => __( '檢視', 'btp' ),
	'view_items'         => __( '檢視', 'btp' ),
	'search_items'       => __( '搜尋', 'btp' ),
	'not_found'          => __( '無結果', 'btp' ),
	'not_found_in_trash' => __( '無結果', 'btp' ),
);

$business->labels( $labels );
```

寫法同 PostType，就只是換成 Taxonomy 以及修改成對應的變數名稱即可，所以另外一個分類 Travel 比照辦理即可。

這個套件還有另外一個強大之處，它可以控制後台文章列表欄位的新增與隱藏，這部分會跟另外一個專門做欄位的套件一起說明。


## wp-metabox

實務上常遇到要新增文章的自訂欄位來增加文章資訊，我最愛用的外掛絕對是 ACF Pro，其他還有像是 Cmb2、Metabox.io、Pods、Toolset 等等，用外掛來處理自訂欄位對我來說是再平常也不過的事，直到需要把外掛發佈給別人用時，才驚覺壓縮之後竟然有好幾 MB，追查之下光 ACF Pro 就有 7.7MB 之多。

於是重新檢視自己對於 ACF 的使用習慣，觀察常用的功能與欄位有哪些，事實上不外乎就是最基本的文字、圖片上傳、下拉選單以及重複器最常使用，其他會用到的機會很少。

為了找尋替代方案照慣例先去 Packagist 爬了一下，找到不少套件在做，但基本上都跟 ACF 差不多，功能包山包海體積也是非常可觀，最後回到 Github 找到由 Matthew Kosloski 所寫的 [wp-metabox-constructor-class](https://github.com/MatthewKosloski/wp-metabox-constructor-class)，剛好裡面提供的欄位可以滿足我的基本需求。

但該類別上次更新是四年前了，也沒有上架 Packagist，所以我就把它整理了一下後上傳，整理的過程中發現到要新增欄位類型進去也不難，所以我現在都拿這套當我的 Metabox 主力開發套件。

內建六種欄位：Text、Textarea、Checkbox、Image Upload、Editor、Repeater，安裝方式如下：

```bash
$ composer require oberonlai/wp-metabox
```

假設我們現在要幫書增加一個 ISBN 與書介欄位，分別會用文字與視覺化編輯器來做，打開 Field.php 輸入以下程式碼：

```php
use ODS\Metabox;

$metabox = new Metabox(
	array(
		'id'       => 'book_info',
		'title'    => '書籍資料',
		'screen'   => 'book',
		'context'  => 'normal',
		'priority' => 'default',
	)
);

$metabox->addText(
	array(
		'id'    => 'book_isbn',
		'label' => 'ISBN',
		'desc'  => '書籍的 ISBN',
	)
);

$metabox->addEditor(
	array(
		'id'    => 'book_intro',
		'label' => '書籍介紹',
		'desc'  => '書籍的簡短介紹',
	)
);
```

先使用 use 來載入 ODS 的命名空間 Metabox 類別，然後建立一個 Metabox 的物件，這是指 metabox 的容器，有這容器之後才能把欄位放進去，接下來使用 addText 跟 addEditor 這兩個方法，完成後如下圖：

![](https://oberonlai.blog/wp-content/uploads/wordpress-and-composer/wordpress-and-composer-05.jpg)

更多的使用方法可以參考 README.md，這邊推薦一個 VSCode 套件：[Markdown Preview Enhanced](https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced)，它可以直接在 VSCode 裡面預覽 Markdown 文件，就不用再返回 Github 頁面或是套件的線上文件去查看使用方法。

![](https://oberonlai.blog/wp-content/uploads/wordpress-and-composer/wordpress-and-composer-11.jpg)

接下來我們接到客戶需求，客戶希望可以在文章列表頁面就能看到 ISBN 以便查詢，這時候上面用過的 posttype 套件又能派上用場了。

![](https://oberonlai.blog/wp-content/uploads/wordpress-and-composer/wordpress-and-composer-06.jpg)

我們先在 posts/book 底下新增一個檔案叫做 Column.php，輸入以下程式碼：

```php
use PostTypes\Columns;

$books->columns()->add(
	array(
		'isbn' => __( 'ISBN' ),
	)
);
$books->register();
```

使用 columns 的 add 方法來新增欄位，裡面傳入的是一個陣列，用 key value 的方式來建立欄位 ID 與顯示名稱，最重要的是最後一行的 $books->register() 不能漏掉，接下來我們就可以針對這個 ID 來讀取資料：

```php
use PostTypes\Columns;

$books->columns()->add(
	array(
		'isbn' => __( 'ISBN' ),
	)
);
$books->columns()->populate(
	'isbn',
	function ( $column, $post_id ) {
		echo esc_html( get_post_meta( $post_id, 'book_isbn', true ) );
	}
);
$books->register();
```

使用的方法是 populate，第一個參數傳欄位 ID，第二個參數是回呼函式，我們用 get_post_meta 來取得 metabox 中的值，完成就能看到新增了一個 ISBN 欄位：

![](https://oberonlai.blog/wp-content/uploads/wordpress-and-composer/wordpress-and-composer-07.jpg)

但客戶希望是放在分類 MyBussiness 欄位的後面，posttype 套件也非常貼心了提供修改欄位順序的功能，從左數過來分類欄位是第三個，所以我們只要將 ISBN 設成 4 即可：

```php
use PostTypes\Columns;

$books->columns()->add(
	array(
		'isbn' => __( 'ISBN' ),
	)
);
$books->columns()->populate(
	'isbn',
	function ( $column, $post_id ) {
		echo esc_html( get_post_meta( $post_id, 'book_isbn', true ) );
	}
);
$books->columns()->order(
	array(
		'isbn' => 4,
	)
);

$books->register();
```

使用 order 方法就能指定順序，如果要用 WordPress 原本的方法寫很麻煩，首先要建立一個空陣列，然後把既有的欄位放進去，透過迴圈判斷欄位名稱是否為 isbn，再判斷迴圈已經跑到第幾個，最重要的是我永遠記不起來勾點 ( Hook ) 要用哪一個...Orz

最後客戶希望可以按照 ISBN 進行排序，這用勾點來做的話一樣是有很多步驟需要處理，但 posttype 幫我們都搞定了，只要使用 sortable 方法即可：

```php
<?php

use PostTypes\Columns;

$books->columns()->add(
	array(
		'isbn' => __( 'ISBN' ),
	)
);
$books->columns()->populate(
	'isbn',
	function ( $column, $post_id ) {
		echo esc_html( get_post_meta( $post_id, 'book_isbn', true ) );
	}
);
$books->columns()->order(
	array(
		'isbn' => 4,
	)
);

$books->columns()->sortable(
	array(
		'isbn' => array( 'isbn', true ),
	)
);

$books->register();
```

使用 sortable 方法指定要進行排序的欄位 ID，然後給它一個 true，同樣的道理，如果客戶今天想要讓某個欄位停止排序，傳入 false 即可。

以上程式示範了新增欄位、產生欄位資料、自訂順序以及排序功能：

![](https://oberonlai.blog/wp-content/uploads/wordpress-and-composer/wordpress-and-composer-08.jpg)

posttype 加上 wp-metabox 真的可以省下很多時間啊～

## wp-option

如果需要開發給社群使用的外掛，最不可或缺的功能就是設定頁面了，有了設定頁面就能把外掛的設定值交給使用者決定，進而依照實際需求彈性調整。

wp-option 這套是根據 boospot 開發的 [boo-settings-helper](https://github.com/boospot/boo-settings-helper) 加以改良，我增加了一層封裝介面，目的是想要可以跟 wp-metabox 一樣可以透過 addText()、addSelect() 這樣的語句來使用它，花了兩天的時間設計與整理文件，用起來實在是太愉悅了，再也不用記勾點了。

![](https://oberonlai.blog/wp-content/uploads/wordpress-and-composer/wordpress-and-composer-10.jpg)

要使用的話第一步先進行安裝：

```
composer require oberonlai/wp-option
```

接下來在建立實例的時候，先命名自己的前綴以作為欄位名稱的辨識，就不會跟既有的衝突：

```php
require __DIR__ . '/vendor/autoload.php';

use ODS\Option;

$prefix = 'plugin-prefix';

$books = new Option( $prefix );

```

整個套件的使用邏輯很單純，要新增一個設定頁面都是一樣的步驟：
1. 新增選單
2. 新增分頁
3. 加入欄位
4. 註冊欄位

```php
// Require the Composer autoloader.
require __DIR__ . '/vendor/autoload.php';

// Import PostTypes.
use ODS\Option;

$config = new Option( 'plugin-prefix-' );
$config->addMenu();
$config->addTab();
$config->addText();
$config->register(); // Don't forget this.
```

### 1.新增選單
首先我們先看增加選單的部分，要傳入的參數是設定頁標題、選單文字、代稱，比較要注意的是子選單的設定，如果你想把設定選單放在某個選單底下，只要把 submenu 參數設定為 true，再把 wp-admin 後面的網址路徑帶入 parent 參數即可：

```php
$config->addMenu(
	array(
		'page_title' => __( 'Plugin Name Settings', 'plugin-name' ),
		'menu_title' => __( 'Plugin Name', 'plugin-name' ),
		'capability' => 'manage_options',
		'slug'       => 'plugin-name',
		'icon'       => 'dashicons-performance',
		'position'   => 10,
		'submenu'    => true,
		'parent'     => 'edit.php?post_type=event',
	)
);
```


### 2.新增分頁
接下來新增分頁的部分，每個設定頁一定都要有一個分頁，它就是像是欄位的容器，所有的欄位都要放在分頁裡面才會正確顯示，id 參數就是分頁容器的名稱，要新增的欄位需要帶入這個名稱，就能把該欄位指定在該分頁下面：

```php
$config->addTab(
	array(
		array(
			'id'    => 'general_section',
			'title' => __( 'General Settings', 'plugin-name' ),
			'desc'  => __( 'These are general settings for Plugin Name', 'plugin-name' ),
		)
	)
);
```

如果要新增兩個以上的分頁，就傳入兩筆陣列資料即可：

```php
$config->addTab(
	array(
		array(
			'id'    => 'general_section',
			'title' => __( 'General Settings', 'plugin-name' ),
			'desc'  => __( 'These are general settings for Plugin Name', 'plugin-name' ),
		),
		array(
			'id'    => 'advance_section',
			'title' => __( 'Advanced Settings', 'plugin-name' ),
			'desc'  => __( 'These are advance settings for Plugin Name', 'plugin-name' )
		)
	)
);
```

### 3.新增欄位
目前該套件可以使用的欄位有十四種：
-   Text
-   URL
-   Number
-   Color
-   Textarea
-   Radio Button
-   Select
-   HTML
-   Checkbox
-   Multi Select
-   Related
-   Password
-   File
-   Media Upload

每種的用法都一樣，譬如要增加 Text 欄位，使用 addText() 方法，然後有兩個參數要傳入，第一個是分頁 id，第二個是設定內容，第三個參數是選填的回呼函式，主要功能在自訂 input 的 DOM 結構：

```
$config->addText(
	'general_section',
	array(
		'id'                => 'text_field_id',
		'label'             => __( 'Hello World', 'plugin-name' ),
		'desc'              => __( 'Some description of my field', 'plugin-name' ),
		'placeholder'       => 'This is Placeholder',
		'show_in_rest'      => true,
		'class'             => 'my_custom_css_class',
		'size'              => 'regular',
	),
);
```

帶入第三個回呼函式的用法：
```php
$config->addText(
	'general_section',
	array(
		'id'    => 'text_field_id',
		'label' => __( 'Hello World', 'plugin-name' ),
	),
	function( $args ) {
		$html  = sprintf(
			'<input 
 class="regular-text"
 type="%1$s"
 name="%2$s"
 value="%3$s"
 placeholder="%4$s"
 style="border: 3px solid red;"
 />',
			$args['type'],
			$args['name'],
			$args['value'],
			'Placeholder from callback'
		);
		$html .= '<br/><small>This field is generated with callback parameter</small>';
		echo $html;
		unset( $html );
	}
);
```

如果是要做 Radio 或是下拉選單用法也一樣，差別在於要多設定 option 參數來決定選項：

```php
$config->addRadio(
	'general_section',
	array(
		'id'      => 'radio_field_id',
		'label'   => __( 'Radio Button', 'plugin-name' ),
		'desc'    => __( 'A radio button', 'plugin-name' ),
		'options' => array(
			'radio_1' => 'Radio 1',
			'radio_2' => 'Radio 2',
			'radio_3' => 'Radio 3',
		),
		'default' => 'radio_2', // 預設值
	),
);
```

所有欄位的使用方法可以參考套件裡面的 README.md 說明。

### 4.註冊欄位
最後一個方法不需要傳入任何參數但卻是是必須的，因為它包含了所有以上行為的勾點註冊，所以萬一你發現選單、分頁、欄位都寫好了卻沒反應，檢查看看是否有正確註冊欄位：

```php
$config->register();
```

以上是設定外掛設定選項的用法，你可以增加多個設定選單，都是一樣的步驟與流程，如果要取得設定值，使用 WordPress 內建的 get_option() 即可，記得不要忘記前綴喔！

```php
$my_text_field_value = get_option( 'plugin-prefix-my_text_field' );
```

## wp-asset

載入 JS、CSS 靜態資源檔幾乎是每個外掛都會用到的功能，原始的寫法要帶入五個參數，參數的帶入順序我每次都會搞錯，再加上 wp_register_script()、wp_localize_script()、wp_enqueue_script() 這三個函式都有各自的參數要帶入，常常被折騰一番後就火大乾脆直接引入，然後就會看到 PHPCS 噴錯XD

幸好後來有發現 Josantonius 寫的 [WP_Register](https://github.com/Josantonius/WP_Register) 套件幫助我解決了這個問題，然後我又根據實務上會遇到的情境再改寫成 wp-asset 套件，並加入了 Ajax 判斷功能，安裝方式如下：

```bash
$composer require oberonlai/wp-asset
```

接下來載入一個 JS 檔：
```php

use ODS\Asset

Asset::addScript(
	array(
		'name'  => 'my_script',
		'url'   => ODS_PLUGIN_URL . 'assets/js/script.js',
	),
);
```

或是載入 CSS：

```php
Asset::addStyle(
	array(
		'name'    => 'my_style',
		'url'     => ODS_PLUGIN_URL . 'assets/css/style.css',
	)
);
```

分別使用 addScript 與 addStyle 兩個方法，然後透過 name 命名檔案名稱與 url 指定檔案路徑就能完成載入，比原始的 wp_enqueue_script 好記多了，其他可選參數如下：

key | 說明 | 類型 | 必填 | 預設
:--------------|:------------|:-----:|:----:|------------------------
name | 檔案唯一名稱 | 字串 | 是	
url	| 檔案路徑 |字串 | 是	
admin | 在後台載入 | 布林值 | 否 | false
deps | 依賴套件 | 陣列 | 否 | 
version | 版本號 | 字串 | 否 | 
footer | 在頁尾載入 ( JS 限定 ) | 布林值 | 否 | true
ajax | 啟用 Ajax 功能 ( JS 限定 ) | 布林值 | 否 | false
params | 需要輸出給前端的參數  ( JS 限定 ) | 陣列 | 否
media | 媒體類型 ( CSS 限定 ) | 字串 | 否 |

需要特別說明的是 Ajax 檔案的載入，當 ajax 設定為 true 時，會自動帶入兩個參數，分別為 ajax_url 與 ajax_nonce，這樣就能直接在 JS 取得，如果要加入其他參數使用 params 即可，範例如下：

```php
Asset::addScript(
	array(
		'name'    => 'my_ajax',
		'url'     => YOUR_PLUGIN_URL . 'assets/js/ajax.js',
		'deps'    => array( 'jquery' ),
		'ajax'    => true,
		'params'  => array(
			'data1'  => 'my_data_1',
			'data2'  => 'my_data_2',
		)
	)
)
```

在前台就可以看到以下的輸出：
```html
<script id="my_ajax-js-extra">
var my_ajax = {"data1":"my_data_1","data2":"my_data_2","ajax_url":"https:\/\/local.test\/wp-admin\/admin-ajax.php?action=my_ajax","ajax_nonce":"fead4137e4"};
</script>
```

最後在 JS 裡面就可以直接取得：

```js
jQuery(function($){
    $(document).ready(function(){ 
        $('#btn').on('click',function(){
			var data = {
				action: "my_ajax",
				nonce: my_ajax.ajax_nonce,
				data1: my_ajax.data1,
				data2: my_ajax.data2
			};
			$.ajax({
				url: my_ajax.ajax_url,
				data: data,
				type: 'POST',
				dataType: "json",
				success: function(data){
					console.log(data);
				},
				error: function(data){
					console.log(data);
				}
			})
        })
    }) 
})
```

這樣就可以省去寫 wp_register_script()、wp_localize_script()、wp_enqueue_script() 的麻煩了，透過統一的介面來完成 Ajax 的檔案引入。


## wp-ajax

Ajax 在外掛開發裡面遇到的機率不定，也就是因為不一定每次都會用到，當要用的時候就會忘記該有的流程跟步驟：

1. 註冊 JS 檔並設定要傳給前端的後端變數
2. 註冊 Ajax 的回呼函數，做 Nonce 驗證跟取得前端傳來的參數後做事情
3. 把處理結果回傳給前端

我常用的是由 Anthony Budd 的 [WP_AJAX](https://github.com/anthonybudd/WP_AJAX)，他寫了一個 WP_AJAX 抽象類別來讓我們繼承，裡面有內建一些實用的方法，像是用 get() 取得從前端傳來的參數、檢查使用者是否登入、取得 HTTP 請求方式等等。

安裝方式：

```
$ composer require oberonlai/wp-ajax
```

基本用法如下：


```php
use ODS\Ajax;

Class MyAjax extends Ajax {

    protected $action = 'my_ajax';

    protected function run(){

    	// 要做的事情放這邊
    	
    	update_option('name', $this->get('name'));

    }
}
MyAjax::listen();
```

先繼承 Ajax 這個類別，然後設定 \$action 這個給 JS 用的識別名稱以及實作 run 方法也就是實際要處理的動作，最後再用靜態方法 listen 完成 Ajax 註冊即可。

listen 方法帶有一個布林值參數，如果設定為 MyAjax::listen( false ) 的話則代表只有已登入的使用者才能執行這個 Ajax。

我們搭配上面的 wp-asset 套件來實作使用 Ajax 更新文章標題：

```php
use ODS\Asset;
use ODS\Ajax;

Asset::addScript(
	array(
		'name'    => 'my_ajax',
		'url'     => ODS_PLUGIN_URL . 'assets/js/ajax.js',
		'deps'    => array( 'jquery' ),
		'version' => ODS_VERSION,
		'ajax'    => true,
		'params'  => array(
			'data1' => 'my_data_1',
			'data2' => 'my_data_2',
		),
	),
);

class MyAjax extends Ajax {
	protected $action = 'my_ajax';
	protected function run() {
		$nonce = $this->get( 'nonce' );
		if ( ! wp_verify_nonce( $nonce, 'my_ajax' ) ) {
			$this->JSONResponse( __( '發生錯誤，不合法的請求來源！', 'my-plugin' ) );
			exit;
		}
		$url     = wp_get_referer();
		$post_id = url_to_postid( $url );
		$my_post = array(
			'ID'         => $post_id,
			'post_title' => 'This is the 123',
		);
		wp_update_post( $my_post );
		$this->JSONResponse( $post_id );
	}
}
MyAjax::listen(false);

```

首先，我們可以使用 $this->get() 來取得從 JS 傳來的參數，在做任何 Ajax 的工作之前，使用 nonce 來做請求來源驗證是必要的，所以我們先取得前端傳來的 nonce，然後用 wp_verify_nonce 來判斷。

接下來把要執行的工作放在 run() 裡面，這邊使用 wp_update_post 做資料更新，最後用內建的 JSONResponse 方法把處理結果回傳給前端，注意到這邊使用 listen( false ) 也就是代表只有登入的使用者才能執行這個修改。

透過以上寫法可以省略要組合勾點 wp_ajax_my_ajax 與 wp_ajax_nopriv_my_ajax 這兩個動作，再搭配 wp-asset 更能方便載入 JS 檔。


## wp-router
在開發外掛的時候常遇到需要背景呼叫的狀況，像是串接 API 時要接收第三方回傳的資料，或是一些不需要前端介面的資料處理頁，以前我會在後台新增頁面，然後用 page-slug.php 的範本來寫這些功能，或是註冊 REST API 的 Endpoint 來處理，但不管是哪個方法，都會有多個步驟需要建立，用起來不是那麼直覺。

後來找到 Tim Field 寫的 [wordpress-dispatcher](https://github.com/tim-field/wordpress-dispatcher) 套件，可以非常直覺的來根據網址執行函式，安裝方式如下：

```shell
$ composer require oberonlai/wp-router
```

然後使用靜態方法 routes，傳入指定的陣列參數：

```php
use ODS\Router;

Router::routes( 
	array (
		'testing-a-url' => function(){
		    echo 'Hello World';
		},
	)
);
```

這樣當輸入網址 https://example.com/testing-a-url 就能看到輸出的內容，網址要傳入動態的參數，也能用正規表達式來限定可帶入的參數：

```php
use ODS\Router;

Router::routes( 
	array (
		'hello-([a-z]+)' => function($request, $name){
		    echo "Hello $name";
		}
	)
);
```

當輸入網址 https://example.com/hello-a 或是 https://example.com/hello-b 一樣可以輸出函式的內容，這個匿名函式帶有兩個參數，\$request 是請求路徑，\$name 是括弧內的參數，所以上面這個例子會輸出 Hello a 以及 Hello b，這樣就能很直覺的根據請求路徑來執行對應的程式。


### 同場加映：如何上傳自己的套件？

如果找不到適合自己的套件該怎麼辦？很簡單，寫一套自己的或是改寫既有的套件來達成吧！像這篇文章裡面提到的所有套件，全都是踩在大大的肩膀上整理出來的，而我習慣用自己的命名空間來方便安裝，因此複製到自己的存放庫之後再上傳到 Packagist 即可。

要上傳 Packagist 套件的 Composer 設定檔基本內容如下：

```json
{
    "name": "oberonlai/wp-router",
    "description": "Adding router for WordPress.",
    "homepage": "https://github.com/oberonlai/wp-router",
    "license": "MIT",
    "authors": [
        {
            "name": "Oberon Lai",
            "email": "m615926@gmail.com",
            "homepage": "https://oberonlai.blog"
        }
    ],
    "minimum-stability": "stable",
    "require": {
        "php": ">=7.2"
    },
    "autoload": {
        "psr-4": {
            "ODS\\": "src/"
        }
    }
}
```

有兩個地方需要注意：第一個是不要放入 version 版本號的參數，因為 Packagist 是用 Github 的 tag 來判斷版本，如果 composer.json 裡面包含 version 會發生錯亂。

第二個關鍵是 autoload，我的命名空間統一使用 ODS ( Oberon Design System )，然後自動載入 src 資料夾裡面的檔案，所以之後要用的時候只要寫 use ODS\\類別名稱 即可。

我慣用的版本控管是 Github，所以準備好套件跟命名空間後就可以推上去了，接下來前往 Packagist 註冊一個帳號，註冊完成後就能提交自己的套件：

![](https://oberonlai.blog/wp-content/uploads/wordpress-and-composer/wordpress-and-composer-12.jpg)

直接在 Repository URL 貼入你的 Github 存放庫網址即可，按下 Check 後他會提醒你有其他類似名稱的套件，直接 Submit 後就完成了。

如果之後發現套件有地方需要修改，修改完後一樣直接先推上 Github，因為 Packagist 是用 tag 來抓版本資訊，所以我們先把修改後的 commit 建立一個新的 tag：

```shell
git tag -a 'v1.0.1' -m '這次改的東西'
```

再把這個 tag 推上去：

```php
git push origin v1.0.1
```

這樣在 Packagist 就能接收到更新版本的資訊了，版本號的命名慣例是 v1.0.x 是除錯、安全性修改、v1.x.0 是新增功能、vX.0.0 是包含相容性問題的大版本更新，這樣就能讓使用者知道這次更新的規模大小，以便讓他們自行決定是否要更新。

<br>

## 小結
透過既有的套件來開發外掛可以大幅提升工作效率，但如果是已經熟悉 WordPress API 的朋友，可能會需要花一點時間來適應這樣的開發模式，因為我一開始也不太習慣，直到後來決定把外掛以物件導向的模式來開發，才驚覺到這樣的作法才能讓程式碼易於維護與重複利用，以及組合不同的物件來完成任務。

而這樣的好處是可以讓程式更易於被測試，避免改東牆壞西牆的情況一再發生。接下來，我們把這樣的設計模式衍伸到前端開發，下一篇文章將介紹如何在外掛裡面使用範本檔修改前端介面並整合前端開發流程，另外跟風一下最近很紅的 TailwindCSS，讓後端可以無痛手刻 UI ~