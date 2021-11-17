---
title: WooCommerce 金流串接實戰（四）- 資料庫結構
date: 2021-09-19 16:00:00
categories:
- WooCommerce
tags:
- WooCommerce 金流
- WordPress 外掛
- WooCommerce 外掛
etoc: false
---

在開始開發金流外掛的後台設定頁面前，我們先來快速認識一下 WordPress 的資料表，同時介紹讀取寫入會用到的函式，以下為有安裝 WooCommerce 的 WordPress 網站所有的資料表：

```
wp_actionscheduler_actions
wp_actionscheduler_claims
wp_actionscheduler_groups
wp_actionscheduler_logs
* wp_commentmeta
* wp_comments
* wp_links
* wp_options
* wp_postmeta
* wp_posts
* wp_term_relationships
* wp_term_taxonomy
* wp_termmeta
* wp_terms
* wp_usermeta
* wp_users
wp_wc_admin_note_actions
wp_wc_admin_notes
wp_wc_category_lookup
wp_wc_customer_lookup
wp_wc_download_log
wp_wc_order_coupon_lookup
wp_wc_order_product_lookup
wp_wc_order_stats
wp_wc_order_tax_lookup
wp_wc_product_meta_lookup
wp_wc_reserved_stock
wp_wc_tax_rate_classes
wp_wc_webhooks
wp_woocommerce_api_keys
wp_woocommerce_attribute_taxonomies
wp_woocommerce_downloadable_product_permissions
wp_woocommerce_log
wp_woocommerce_order_itemmeta
wp_woocommerce_order_items
wp_woocommerce_payment_tokenmeta
wp_woocommerce_payment_tokens
wp_woocommerce_sessions
wp_woocommerce_shipping_zone_locations
wp_woocommerce_shipping_zone_methods
wp_woocommerce_shipping_zones
wp_woocommerce_tax_rate_locations
wp_woocommerce_tax_rates
```

WordPress 本身預設的資料表只有 * 註記的那幾個，其他都是 WooCommerce 產生的，每張表都有 wp_ 的前綴，這可以在 wp-config.php 裡面透過以下變數來修改：

```php
$table_prefix = 'wp_'; // 在第一次安裝還沒有建立庫時修改
```

如果你的 WordPress 是使用套裝軟體安裝的，因為安全性考量他們會自動幫你修改資料表前綴，所以當你進入資料庫要找的是 xxx_options、xxx_posts 這些資料表，看前綴後面的名稱就好。接下來介紹五個表，分別是與設定、訂單、以及會員資料相關的表。

<!--more-->


## wp_options

該表存放全站設定相關的資料，也就是在後台左側選單 > 設定裡面的設定都會存在這張表裡面，我們要開發的 WooCommerce 金流外掛，也是把相關的變數存在這個表中。為了節省效能，在寫入時可以先把設定的值組成陣列再進行 INSERT。


| # | column_name  | data_type           | character_set | collation          | is_nullable | column_default | extra          | foreign_key | comment |
|---|--------------|---------------------|---------------|--------------------|-------------|----------------|----------------|-------------|---------|
| 1 | option_id    | bigint(20) unsigned | NULL          | NULL               | NO          | NULL           | auto_increment |             |         |
| 2 | option_name  | varchar(191)        | utf8mb4       | utf8mb4_unicode_ci | NO          |                |                |             |         |
| 3 | option_value | longtext            | utf8mb4       | utf8mb4_unicode_ci | NO          | NULL           |                |             |         |
| 4 | autoload     | varchar(20)         | utf8mb4       | utf8mb4_unicode_ci | NO          | yes            |                |             |         |


wp_options 相關的函式定義在 wp-include/option.php，常用的如下：

**add_option( $option_name, $option_value, autoload=yes) 新增設定**

它會先自動檢查 $option_name 是否存在，不存在的話新增一筆資料，已存在的話會把 $option_value 進行覆寫。autoload 預設為 yes，作用是設定該 $option 是否加入到快取之中。如果你確定每個頁面都會用到這個設定的話，就保持預設值 yes，反之為 no。範例如下：

```php
// add option
$twitters = array( '@abc', '@cde', '@fgh' );// 用陣列來整理 option_value
add_option( 'irp_twitter_accounts', $twitters );
```

**update_option( $option_name, $option_value ) 更新設定**

更新已存在的 $optioni_value，如果該 $option_name 不存在的會自動新增一筆，範例如下：

```php
// update option
$twitters = array_merge(
	$twitters,
	array(
		'@ijk',
		'@lmn'
	)
);
update_option( 'irp_twitter_accounts', $twitters );
```

**get_option( $option_name ) 取得設定欄位的值**

範例如下：

```php
// get option
$irp_twitter_accounts = get_option( 'irp_twitter_accounts' );
foreach( $irp_twitter_accounts as $account ){
	echo $account.', '; // 輸出 @abc, @cde, @fgh, @ijk, @lmn
}
```

**delete_option( $option_name ) 刪除設定欄位**

範例如下：

```php
// delete option
delete_option( 'irp_twitter_accounts' );
```


## wp_posts

存放文章、頁面、選單、文章版本、以及自定義文章的資料表，而自定義文章就是用這個表裡面的 post_type 欄位來紀錄不同的文章類型，WooCommerce 商品的 post_type 為 proudct，訂單則為 shop_order，它並沒有新建資料表來存放，而是放在 wp_posts 裡面。


| #  | column_name           | data_type           | character_set | collation          | is_nullable | column_default      | extra          | foreign_key | comment |
|----|-----------------------|---------------------|---------------|--------------------|-------------|---------------------|----------------|-------------|---------|
| 1  | ID                    | bigint(20) unsigned | NULL          | NULL               | NO          | NULL                | auto_increment |             |         |
| 2  | post_author           | bigint(20) unsigned | NULL          | NULL               | NO          | NULL                |                |             |         |
| 3  | post_date             | datetime            | NULL          | NULL               | NO          | 0000-00-00 00:00:00 |                |             |         |
| 4  | post_date_gmt         | datetime            | NULL          | NULL               | NO          | 0000-00-00 00:00:00 |                |             |         |
| 5  | post_content          | longtext            | utf8mb4       | utf8mb4_unicode_ci | NO          | NULL                |                |             |         |
| 6  | post_title            | text                | utf8mb4       | utf8mb4_unicode_ci | NO          | NULL                |                |             |         |
| 7  | post_excerpt          | text                | utf8mb4       | utf8mb4_unicode_ci | NO          | NULL                |                |             |         |
| 8  | post_status           | varchar(20)         | utf8mb4       | utf8mb4_unicode_ci | NO          | publish             |                |             |         |
| 9  | comment_status        | varchar(20)         | utf8mb4       | utf8mb4_unicode_ci | NO          | open                |                |             |         |
| 10 | ping_status           | varchar(20)         | utf8mb4       | utf8mb4_unicode_ci | NO          | open                |                |             |         |
| 11 | post_password         | varchar(255)        | utf8mb4       | utf8mb4_unicode_ci | NO          |                     |                |             |         |
| 12 | post_name             | varchar(200)        | utf8mb4       | utf8mb4_unicode_ci | NO          |                     |                |             |         |
| 13 | to_ping               | text                | utf8mb4       | utf8mb4_unicode_ci | NO          | NULL                |                |             |         |
| 14 | pinged                | text                | utf8mb4       | utf8mb4_unicode_ci | NO          | NULL                |                |             |         |
| 15 | post_modified         | datetime            | NULL          | NULL               | NO          | 0000-00-00 00:00:00 |                |             |         |
| 16 | post_modified_gmt     | datetime            | NULL          | NULL               | NO          | 0000-00-00 00:00:00 |                |             |         |
| 17 | post_content_filtered | longtext            | utf8mb4       | utf8mb4_unicode_ci | NO          | NULL                |                |             |         |
| 18 | post_parent           | bigint(20) unsigned | NULL          | NULL               | NO          | NULL                |                |             |         |
| 19 | guid                  | varchar(255)        | utf8mb4       | utf8mb4_unicode_ci | NO          |                     |                |             |         |
| 20 | menu_order            | int(11)             | NULL          | NULL               | NO          | NULL                |                |             |         |
| 21 | post_type             | varchar(20)         | utf8mb4       | utf8mb4_unicode_ci | NO          | post                |                |             |         |
| 22 | post_mime_type        | varchar(100)        | utf8mb4       | utf8mb4_unicode_ci | NO          |                     |                |             |         |
| 23 | comment_count         | bigint(20)          | NULL          | NULL               | NO          | NULL                |                |             |         |


wp_posts 相關的函式定義在 wp-include/post.php，常用的如下：

**wp_inset_post( $postarr, $wp_error=false) 插入新文章**

\$postarr 是一個定義新文章內容的陣列，裡面有許多參數可以定義，$wp_error 為 true 時會在文章插入失敗時回傳一個 WP_Error 物件，範例如下：

```php
// insert post - set post status to draft
$args = array(
	'post_title'   => '文章標題',
	'post_excerpt' => '文章摘要',
	'post_content' => '內文',
	'post_status'  => 'draft',  // 文章狀態，有 draft、publish
	'post_type'    => 'post', // 文章類型，預設為 post，可以是 page 或其他 CPT
	'post_author'  => 1, // 作者 ID
	'menu_order'   => 0 // 文章順序
);
$post_id = wp_insert_post( $args );  // 新文章插入完成後會回傳該文章的 post ID
echo 'post ID: ' . $post_id . '<br>';
```

**wp_update_post( $postarr, $wp_error = false) 更新文章資料**

同樣使用 $postarr 陣列來更新資料，記得要提供需要被更新的文章 ID，範例如下：

```php
// update post - change post status to publish
$args = array(
	'ID'          => $post_id,
	'post_status' => 'publish',
);
wp_update_post( $args );
```

**get_post( $post=null, $output=OBJECT, $filter=’raw’) 取得文章資料**

第一個參數指定要取得的文章的 ID，如果是在 post loop 中的話留空會自動取得當前文章。\$output 資料格式可以設定為 OBJECT 或是 ARRAY_A (關聯式陣列) 與 ARRAY_N(索引式陣列)。\$filter 參數為設定要 sanitize 的模式，可選值有 raw、edit、db、display、attribute 與 js，可以做輸出內容的過濾。

```php
// get post - return post data as an object
$post = get_post( $post_id );
echo 'Object Title: ' . $post->post_title . '<br>';

// get post - return post data as an array
$post = get_post( $post_id, ARRAY_A );
echo 'Array Title: ' . $post['post_title'] . '<br>';
```

**get_posts( $args = null ) 取得文章列表**

當要取得文章清單時，像是最新文章列表、特定分類列表就可以使用這支函式。它是根據 WP_Query 類別所設計的。$args 參數可以用陣列的方式來提供文章取得條件，範例如下：

```php
// get posts - return 100 posts
$posts = get_posts(
	array(
		'numberposts'      => '100', // 顯示文章數量
		'category'         => 0,  // 文章分類 ID
		'orderby'          => 'date',  // 排序條件
		'order'            => 'DESC',  // 排序方式，DESC 為降冪，ASC 為升冪
		'include'          => array( 1, 2, 3 ),  // 只顯示特定的文章
		'exclude'          => array( 4, 5, 6 ),  // 排除指定文章
		'meta_key'         => '',  // 顯示帶有指定欄位的文章
		'meta_value'       => '',  // 顯示帶有指定欄位值的文章
		'post_type'        => 'post',  // 文章類型
		'suppress_filters' => true,  // 如果需要使用 filter 來修改 get_posts 的結果，這個參數要設為 false
	)
);
// loop all posts and display the ID & title
foreach ( $posts as $post ) {
	echo $post->ID . ': ' . $post->post_title . '<br>';
}
```

**wp_delete_post( $postid=0, $force_delete =false ) 刪除指定文章**

第一個參數為要刪除的文章 ID，第二個參數如果為 true 的話，文章就不會進垃圾桶而直接被刪除，代表連救回來的機會都沒有，預設值為 false，範例如下：

```php
// delete post - skip the trash and permanently delete it  
wp_delete_post( $post_id, true );
```

## wp_postmeta

如果內建的文章欄位不夠用，想要新增其他欄位的話就會寫在這張表中。WordPress 提供很方便的作法讓你不用再自行增加資料表，每個 postmeta 使用 post_id 來對應到文章。

如果你需要新增欄位但又不想讓使用者在文章編輯畫面看到該欄位，可以用下底線開頭的欄位名稱，這樣 WordPress 會自動在後台隱藏這個欄位而不被使用者看見。

在開發金流外掛時會需要新增一些訂單欄位，像是從金流商回傳的訂單資料、卡號末四碼、以及相關資訊，而這些資料就可以放在 postmeta。


| # | column_name | data_type           | character_set | collation          | is_nullable | column_default | extra          | foreign_key | comment |
|---|-------------|---------------------|---------------|--------------------|-------------|----------------|----------------|-------------|---------|
| 1 | meta_id     | bigint(20) unsigned | NULL          | NULL               | NO          | NULL           | auto_increment |             |         |
| 2 | post_id     | bigint(20) unsigned | NULL          | NULL               | NO          | NULL           |                |             |         |
| 3 | meta_key    | varchar(255)        | utf8mb4       | utf8mb4_unicode_ci | YES         | NULL           |                |             |         |
| 4 | meta_value  | longtext            | utf8mb4       | utf8mb4_unicode_ci | YES         | NULL           |                |             |         |


相關函式如下：

**get_post_meta( $post_id, $meta_key, $single=false ) 取得文章欄位值**

第一個參數為文章 ID，第二個為欄位名稱，如果留空值，會回傳所有相同 \$meta_key 的 \$meta_value，\$single 為 true 的話會以字串回傳符合第一個 \$meta_key 的 \$meta_value，false 的話則會使用陣列回傳所有符合的 \$meta_value，範例如下：

```php
// get post meta - get 1st instance of key
$student = get_post_meta( $post_id, 'irp_order', true );
echo 'oldest student: ' . $student;
```

**update_post_meta( $post_id, $meta_key, $meta_value, $prev_value=’’) 更新文章欄位**

需要注意的是第四個參數，如果 \$meta_key 有多筆符合，在 \$prev_value 為空的情況下，\$meta_value 只會修改第一筆 \$meta_key，如果 \$prev_value 有符合多筆 \$meta_key 的 \$meta_value 其中一筆，則這一筆的 value 會被第三個 \$meta_value 給取代掉。範例如下：

```php
// update post meta - public metadata
$content = 'You SHOULD see this custom field when editing your latest post.';
update_post_meta( $post_id, 'irp_displayed_field', $content );

// update post meta - hidden metadata
$content = str_replace( 'SHOULD', 'SHOULD NOT', $content );
update_post_meta( $post_id, '_irp_hidden_field', $content );
```

**add_post_meta( $post_id, $meta_key, $meta_value, $unique=false)**

習慣上新增文章欄位會使用 update_post_meta()，因為它有判斷 \$meta_key 是否存在的功能，會用 add_post_meta 的情境在於要新增多筆 \$meat_value 到相同的 \$meta_key。第四個參數 \$unique 為 true 時代表不允許有多筆相同的 \$meta_key，範例如下：

```php
add_post_meta( $post_id, 'irp_orders', $orders, true );
```

**delete_post_meta( $post_id, $meta_key, $meta_value=’’ ) 刪除文章欄位**

第三個參數當有指定 \$meta_value 只會刪除符合該 value 的 \$meta_key，範例如下

```php
// delete post meta
delete_post_meta( $post_id, 'irp_student' );
```

## wp_users

存放使用者帳密與基本資料，如果要從資料庫修改使用者相關資訊，像是客戶忘記密碼或是要新增一個管理員帳號，就可以在這個表進行處理。


| #  | column_name         | data_type           | character_set | collation          | is_nullable | column_default      | extra          | foreign_key | comment |
|----|---------------------|---------------------|---------------|--------------------|-------------|---------------------|----------------|-------------|---------|
| 1  | ID                  | bigint(20) unsigned | NULL          | NULL               | NO          | NULL                | auto_increment |             |         |
| 2  | user_login          | varchar(60)         | utf8mb4       | utf8mb4_unicode_ci | NO          |                     |                |             |         |
| 3  | user_pass           | varchar(255)        | utf8mb4       | utf8mb4_unicode_ci | NO          |                     |                |             |         |
| 4  | user_nicename       | varchar(50)         | utf8mb4       | utf8mb4_unicode_ci | NO          |                     |                |             |         |
| 5  | user_email          | varchar(100)        | utf8mb4       | utf8mb4_unicode_ci | NO          |                     |                |             |         |
| 6  | user_url            | varchar(100)        | utf8mb4       | utf8mb4_unicode_ci | NO          |                     |                |             |         |
| 7  | user_registered     | datetime            | NULL          | NULL               | NO          | 0000-00-00 00:00:00 |                |             |         |
| 8  | user_activation_key | varchar(255)        | utf8mb4       | utf8mb4_unicode_ci | NO          |                     |                |             |         |
| 9  | user_status         | int(11)             | NULL          | NULL               | NO          | NULL                |                |             |         |
| 10 | display_name        | varchar(250)        | utf8mb4       | utf8mb4_unicode_ci | NO          |                     |                |             |         |


wp_users 相關的函式定義在 wp-include/pluggable.php、wp-include/user.php，常用的如下：

**wp_insert_user( $userdata ) 新增使用者**

$userdata 為一個陣列，使用陣列來定義使用者的資料欄位，範例如下：

```php
// insert user
$userdata = array(
	'user_login'    => 'oberon',
	'user_pass'     => '!@oberon!@#$',
	'user_nicename' => 'oberon',
	'user_url'      => 'https://oberonlai.blog/',
	'user_email'    => 'm615926@gmail.com',
	'display_name'  => 'Oberon Lai',
	'nickname'      => 'Oberon',
	'first_name'    => 'Oberoni',
	'last_name'     => 'Lai',
	'description'   => 'This is a WordPress Administrator account.',
	'role'          => 'administrator',
);
wp_insert_user( $userdata );
```

**wp_create_user( $username, $password, $email ) 新增使用者簡易版**

相較於 wp_insert_user()，它只要提供帳戶名、密碼、電子郵件三個參數就可以新增使用者，範例如下：

```php
// create users
wp_create_user( 'oberon', '!@oberon!@#$', 'm615926@gmail.com' );
```

**wp_update_user( $userdata ) 修改 wp_users 與 wp_usermeta 裡面的欄位**

如果修改了使用者密碼，所有的 cookie 會被清除，該使用者會強制登出，$userdata 的欄位與 wp_insert_user() 相同，範例如下：

```php
// update user-change username fields and change role to admin
$userdata = array(
	'ID'         => $user->ID,
	'user_pass'  => '!@oberon!@#$',
	'first_name' => 'Oberon',
	'last_name'  => 'Lai',
	'user_url'   => 'https://oberonlai.blog',
	'role'       => 'administrator',
);
wp_update_user( $userdata );
```

如果要用這個函式來讓使用者在前台更新登入密碼，記得要在 init 執行，因為它會觸發登出再登入的動作，這個動作需要清除與設定 Cookie，因此必須放在頁面的最前面來執行，也就是要比 get_header() 還要前面。

此外，重設密碼後的登入是分三個動作：wp_set_password()、wp_set_auth_cookie()、wp_set_current_user()，wp_update_user() 把這些動作都封裝起來可以直接使用。

**get_user_by( $field, $value ) 根據使用者資料來取得使用者**

取得的使用者會以 WP_User 物件進行回傳，這支的作用等同於 Sql 語句下：SELECT * FROM wp_users WHERE \$field = \$value，\$field 是使用者資料欄位，\$value 是使用者資料值，範例如下：

```php
// get user by email
$user = get_user_by( 'email', 'm615926@gmail.com' );
echo 'username: ' . $user->user_login . ' / ID: ' . $user->ID . '<br>';
```

**wp_delete_user( $user_id, $reassign ) 刪除使用者並把該使用者文章移轉給其他人**

第一個參數為要被刪除的使用者 ID，第二個參數為要繼承文章的使用者 ID，要注意的是如果第二個參數沒有指定，則該使用者的文章都會被移除，範例如下：

```php
// delete user-delete the original admin and set their posts to our new admin
wp_delete_user( 1, $user->ID );
```

## wp_usermeta

記錄使用者更多更詳細的資料，也就是除了 wp_user 已經內建的欄位以外的使用者資料，可自行新增使用者欄位，相關函式常用的如下：

**get_user_meta( $user_id, $meta_key, $single=false ) 取得特定使用者的資料**

\$user_id 為使用者 id，\$meta_key 要取得的欄位，如果留空值，會回傳所有相同 \$meta_key 的 \$meta_value，\$single 為 true 的話會以字串回傳符合第一個 \$meta_key 的 \$meta_value，false 的話則會使用陣列回傳所有符合的 \$meta_value，範例如下：

```php
// get brian's id
$oberon_id = get_user_by( 'login', 'oberon' )->ID;
$oberons_wife = get_user_meta( $brian_id, 'oberon_wife', true); // Wife 可能有多個(?)，true 的話回傳第一個
echo "Oberon's wife: " . $oberons_wife . "<br>";
```

**update_user_meta( $user_id, $meta_key, $meta_value, $prev_value=”” ) 更新使用者的資料**

需要注意的是第四個參數，如果 \$meta_key 有多筆符合，在 \$prev_value 為空的情況下，\$meta_value 只會修改第一筆\ $meta_key，如果 \$prev_value 有符合多筆 \$meta_key 的 \$meta_value 其中一筆，則這一筆的 value 會被第三個 \$meta_value 給取代掉。範例如下：

```php
// update user meta - this will update oberon to oberon jr.
update_user_meta( $oberon_id, 'oberon_kid', 'Oberon Jr', 'Miffy' ); // Oberon 有多個小孩(多筆相同的 meta_key)，名字叫做 Miffy 的會被修改為 Oberon Jr
```

**add_user_meta( $user_id, $meta_key, $meta_value, $unique=false) 新增使用者資料欄位**

習慣上新增使用者資料會使用 update_user_meta()，因為它有判斷 $meta_key 是否存在的功能，會用 add_user_meta 的情境在於要新增多筆 \$meat_value 到相同的 \$meta_key。第四個參數 \$unique 為 true 時代表不允許有多筆相同的 \$meta_key，範例如下：

```php
// add user meta - 3rd parameter is a unique value
add_user_meta( $oberon_id, 'oberon_kid', 'Dalya' );
add_user_meta( $oberon_id, 'oberon_kid', 'Oberon Jr' );
add_user_meta( $oberon_id, 'oberon_kid', 'Nina' );
add_user_meta( $oberon_id, 'oberon_kid', 'Cam' );
add_user_meta( $oberon_id, 'oberon_kid', 'Aksel' );
```

**delete_user_meta( $user_id, $meta_key, $meta_value=””) 刪除使用者資料**

第三個參數當有指定 $meta_value 只會刪除符合該 value 的 $meta_key，範例如下：

```php
// delete oberon's user meta
delete_user_meta( $oberon_id, 'oberon_wife' );
delete_user_meta( $oberon_id, 'oberon_kid', 'Miffy' ); // 沒指定 Miffy 的話則所有小孩都會被刪除
```

關於 WordPress 資料庫還有很多部分可以介紹，像是自訂資料表以及全域變數 \$wpdb 等等，現階段我們把焦點先放在開發金流的設定頁面上，下一篇會介紹 WooCommerce API。