---
title: 【 書摘 】Building Web Apps with WordPress - WordPress 基本介紹
tags:
  - Hook
  - localhost
  - schema
  - taxonomy
  - wp_comment
  - wp_option
  - wp_posts
  - wp_terms
  - wp_users
id: '1368'
categories:
  - - WordPress
date: 2020-05-07 10:03:25
etoc: true
---

## 二、WordPress Basic / WordPress 基本介紹

第二章的部分先從介紹 WordPress 主程式的資料夾結構以及資料庫結構與相對應的 API 開始。

## **WordPress 資料夾結構**

要開發 WordPress 之前，必須知道的第一條鐵律就是「永遠不要修改 WordPress」。這意思是說千萬不要去修改 WordPress 的核心程式，因為只要一更新所有的修改就會被覆蓋，而更新通常都會有修補漏洞的項目，所以為了要能讓網站維持在安全的狀態，更新到最新版本是必要的。

也因此不要修改核心程式碼，要修改之前先找看看有沒有相關的 Hook 可以使用，如果沒找到適合的可以回報給核心維護團隊，請他們把你需要的 Hook 加入，這樣最好的作法。

至於哪些是核心程式碼？除了 wp-content 資料夾以及 wp-config.php 以外，其他都是核心程式碼，你永遠、絕對、不要修改它！

<!--more-->

> Hook 的機制可以想像成是蓋房子的時候預留的管線，當今天房客要牽新的電話線或是水管時，不用把牆壁全部打掉，只要用之前準備好的管線來接就可以。
> 
> Hook 分為兩種，Action 是可以把一些特定的 Function 交由 Hook 來執行，Filter 是可以修改參數、顯示文字與設定值，Hook 可以放在你的佈景主題或是外掛來執行，這樣就不用修改到核心程式碼。
> 
> 現在還不了解 Hook 是什麼也沒關係，更多關於 Hook 的部分之後會一直反覆出現，反覆到你都嫌煩了它還是會一直出現XD，因為他是 WordPress 程式運作的核心要素之一。

（ㄧ）wp-admin - 所有 WordPress 後台的程式碼都放在這個資料夾裡面，每個檔案都對應一個後台的頁面或是功能。

> 如果想要客製 WordPress 後台，會需要知道一堆 Hook，有時候與其去爬 Google 查文件，個人覺得直接看檔案還比較快，有看到 do_action 就是可以掛 Action 的地方，有看到 apply_filter 就是可以掛 Filter 的地方，等找到 Hook 名稱再去 Google 看要怎麼用，不然官方列出來的 Hook 爆炸多，而且很多又長得超像，常常都是要不停用除錯法才能試到我想要的 Hook，非常花時間...

（二）wp-include - WordPress 所有的 API 、類別都是放在這個資料夾裡面，深入理解他們可以了解 WordPress 的工作方式。

> 裡面非常精彩，我最喜歡看他們的類別是怎麼設計的，或是常用的 function 是怎麼寫的，每次隨便逛逛都會有意想不到的收穫～

（三）wp-content - 我們可以修改的地方，也是放外掛、佈景主題與上傳檔案的地方。

（四）wp-content/themes - 放所有佈景主題的資料夾，要手動上傳佈景主題把東西放在這邊就對了。

（五）wp-content/plugins - 放所有外掛的資料夾，要手動上傳外掛把東西放在這邊就對了。

（六）wp-content/mu-plugins - MU 是 Must Use 必須使用的意思，當把外掛放在這邊的時候，WordPress 會直接自動啟用該外掛，並且不會出現在後台 > 外掛的清單之中，所以當今天如果你要停用所有外掛來進行網站除錯的時候，記得檢查一下有沒有 mu-plugins 這個資料夾，否則萬一問題是出在 mu-pluings 裡面的外掛時，會讓你以為自己在鬼打牆。

> 專門定義資料結構的外掛，也就是註冊 CPT、Taxonomy、Nav 的外掛，非常適合放在這個資料夾，因為這些資料是網站的基本元素，不會因為換了佈景主題而消失，也不應該被停用，所以有預設應該被啟用的外掛就放在這邊。
> 
> 如果要把外掛放在這個資料夾必須要有一隻主要的檔案 .php，然後要引入的檔案可以放在資料夾裡面，而主要檔案不可以放在資料夾裡面，不然會抓不到。

wp-content 裡面還有其他資料夾，有 WordPress 內建的，也有會因為你使用的外掛而自動產生的，當要搬移網站的時候，最簡單的方式就是把整個 wp-content 打包然後丟到新家去，就能移轉你的網站。當然，資料庫也要記得搬，接下來說明資料庫結構：

## **WordPress 資料庫結構**

這邊介紹了 WordPress 的資料表，以及存取寫入這些資料表相關的 Function，這些 Function 都可以寫在佈景主題裡面的 function.php 來使用它，或是放在你自己寫的外掛裡面。

資料表名稱的前綴使用預設的 wp_，這可以在 wp-config.php 裡面透過以下參數來修改：

$table_prefix = 'wp_'; // 把 wp 換成其他前綴

如果你的 WordPress 是使用主機面板安裝的，像是 cPanel、Plesk，因為安全性考量他們會自動幫你修改資料表前綴，所以你打開 PhpMyAdmin 要找的是 abc_options、abc_posts 這些資料表就好，前綴 abc 不用管他。

* * *

**（ㄧ）wp_option** - 該表存放全站設定相關的資料，也就是在後台左側選單 > 設定裡面的設定都會存在這張表裡面，如果你有自己開發外掛的話，也可以把相關的變數存在這個表中。為了節省效能，在寫入時會先把設定的值組成一個陣列再進行 INSERT。

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-25-at-08.54.29-1024x140.jpg)

wp_options 資料表結構

wp_options 相關的 Function 定義在 wp-include/option.php，常用的如下：

**add_option( $option_name, $option_value, autoload=yes) 新增設定**

它會先自動檢查 $option_name 是否存在，不存在的話新增一筆資料，已存在的話會把 $option_value 進行覆寫。autoload 預設為 yes，作用是設定該 $option 是否加入到快取之中。如果你確定每個頁面都會用到這個設定的話，就保持預設值 yes，反之為 no。範例如下：
```php
// add option
$twitters = array( '@bwawwp', '@bmess', '@jason_coleman' );// 用陣列來整理 option_value
add_option( 'bwawwp_twitter_accounts', $twitters );
```
**update_option( $option_name, $option_value ) 更新設定**

更新已存在的 $optioni_value，如果該 $option_name 不存在的會自動新增一筆，範例如下：
```php
// update option
$twitters = array_merge(
         $twitters,
         array(
                 '@alphaweb',
                 '@pmproplugin'
         )
);
update_option( 'bwawwp_twitter_accounts', $twitters );
```
**get_option( $option_name ) 取得設定欄位的值**

範例如下：
```php
// get option
$bwawwp_twitter_accounts = get_option( 'bwawwp_twitter_accounts' );
foreach( $bwawwp_twitter_accounts as $account ){
echo $account.', '; // 輸出 @bwawwp, @bmewss, @jason_coleman
}
```
**delete_option( $option_name ) 刪除設定欄位**

範例如下：
```php
// delete option
delete_option( 'bwawwp_twitter_accounts' );
```
> 每次要修改網站管理員電子郵件的時候，WordPress 會發確認信到新的信箱來做確認，但我懶，所以都直接進 wp_options 這個表來修改管理員的 E-mail，找到 option_name 為 admin_email 的欄位，然後修改 option_value 為新的電子郵件即可。

* * *

**（二）wp_users** - 存放使用者帳密與基本資料，如果要從資料庫修改使用者相關資訊，像是客戶忘記密碼或是要新增一個管理員帳號，就可以在這個表進行處理。

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-25-at-09.54.54-1024x249.jpg)

wp_users 資料表結構

wp_users 相關的 Function 定義在 wp-include/pluggable.php、wp-include/user.php，常用的如下：

**wp_insert_user( $userdata ) 新增使用者**

$userdata 為一個陣列，使用陣列來定義使用者的資料欄位，範例如下：
```php
// insert user
$userdata = array(
    'user_login'    => 'brian',
'user_pass'     => 'KO03gT7@n*',
'user_nicename' => 'Brian',
'user_url'      => 'https://alphaweb.com/',
'user_email'    => 'brian@alphaweb.com',
'display_name'  => 'Brian',
'nickname'      => 'Brian',
'first_name'    => 'Brian',
'last_name'     => 'Messenlehner',
'description'   => 'This is a WordPress Administrator account.',
'role'          => 'administrator'
);
wp_insert_user( $userdata );
```
**wp_create_user( $username, $password, $email ) 新增使用者簡易版**

相較於 wp_insert_user()，它只要提供帳戶名、密碼、電子郵件三個參數就可以新增使用者，範例如下：
```php
// create users
wp_create_user( 'jason', 'YR529G%*v@', 'jason@schoolpress.me' );

**wp_update_user( $userdata ) 修改 wp_users 與 wp_usermeta 裡面的欄位**

如果修改了使用者密碼，所有的 cookie 會被清除，該使用者會強制登出，$userdata 的欄位與 wp_insert_user() 相同，範例如下：

// update user-change username fields and change role to admin
$userdata = array(
'ID'         => $user->ID,
    'user_pass'  => 'KO03gT7@n*',
'first_name' => 'Jason',
'last_name'  => 'Coleman',
'user_url'   => 'http://strangerstudios.com/',
'role'       => 'administrator'
);
wp_update_user( $userdata );
```
> 昨天用了這支來讓使用者在前台更新登入密碼，結果表單送出後一直噴「Cannot modify header information - headers already sent by...」這個惱人的錯誤，爬了[文件](https://developer.wordpress.org/reference/functions/wp_update_user/)才知道，登出再登入的動作需要清除與設定 Cookie，因此必須放在頁面的最前面來執行，也就是要比 get_header() 還要前面。
> 
> 另外找到了這篇 ---> https://stackoverflow.com/questions/47297101/issue-when-updating-the-password-with-wp-update-user
> 
> 讓我理解到原來重設密碼後的登入是分三個動作：wp_set_password()、wp_set_auth_cookie()、wp_set_current_user()，只是 wp_update_user() 把這些動作都封裝起來可以直接使用。

**get_user_by( $field, $value ) 根據使用者資料來取得使用者**

取得的使用者會以 WP_User 物件進行回傳，這支的作用等同於 Sql 語句下：SELECT * FROM wp_users WHERE $field = $value，$field 是使用者資料欄位，$value 是使用者資料值，範例如下：
```php
// get user by email
$user = get_user_by( 'email', 'jason@schoolpress.me' );
echo 'username: ' . $user->user_login . ' / ID: ' . $user->ID . '<br>';
```
**wp_delete_user( $user_id, $reassign ) 刪除使用者並把該使用者文章移轉給其他人**

第一個參數為要被刪除的使用者 ID，第二個參數為要繼承文章的使用者 ID，要注意的是如果第二個參數沒有指定，則該使用者的文章都會被移除，範例如下：
```php
// delete user-delete the original admin and set their posts to our new admin
wp_delete_user( 1, $user->ID );
```
> 忘記使用者密碼的話進入 wp_users 找到要修改密碼的使用者，然後找到 user_pass 欄位，在修改前記得先把你的新密碼用[這個工具](https://www.md5hashgenerator.com)來加密成 MD5，加密後才把 MD5 的字串填進 user_pass。因為安全性考量，WordPress 會把所有的使用者密碼都用 MD5 進行加密，如果直接填未加密的密碼進去是無效的。
> 
> 至於如何從資料庫新增一個管理員帳號可以參考這篇文章 ---> [https://wpengine.com/support/add-admin-user-phpmyadmin/](https://wpengine.com/support/add-admin-user-phpmyadmin/)

* * *

**（三）wp_usermeta** - 使用者自訂欄位

記錄使用者更多更詳細的資料，也就是除了 wp_user 已經寫死欄位以外的使用者資料，可自行新增使用者欄位

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-26-at-09.17.43-1024x126.jpg)

wp_usermeta 資料表結構

wp_usermeta 相關 Function 常用的如下：

**get_user_meta( $user_id, $meta_key, $single=false ) 取得特定使用者的資料**

$user_id 為使用者 id，$meta_key 要取得的欄位，如果留空值，會回傳所有相同 $meta_key 的 $meta_value，$single 為 true 的話會以字串回傳符合第一個 $meta_key 的 $meta_value，false 的話則會使用陣列回傳所有符合的 $meta_value，範例如下：
```php
// get brian's id
$brian_id = get_user_by( 'login', 'brian' )->ID;
$brians_wife = get_user_meta( $brian_id, 'bwawwp_wife', true); // Wife 可能有多個(?)，true 的話回傳第一個
echo "Brian's wife: " . $brians_wife . "<br>";
```
**update_user_meta( $user_id, $meta_key, $meta_value, $prev_value="" ) 更新使用者的資料**

需要注意的是第四個參數，如果 $meta_key 有多筆符合，在 $prev_value 為空的情況下，$meta_value 只會修改第一筆 $meta_key，如果 $prev_value 有符合多筆 $meta_key 的 $meta_value 其中一筆，則這一筆的 value 會被第三個 $meta_value 給取代掉。範例如下：
```php
// update user meta - this will update brian to brian jr.
update_user_meta( $brian_id, 'bwawwp_kid', 'Brian Jr', 'Briand' ); // Brian 有多個小孩(多筆相同的 meta_key)，名字叫做 Braind 的會被修改為 Brain Jr
```
**add_user_meta( $user_id, $meta_key, $meta_value, $unique=false) 新增使用者資料欄位**

習慣上新增使用者資料會使用 update_user_meta()，因為它有判斷 $meta_key 是否存在的功能，會用 add_user_meta 的情境在於要新增多筆 $meat_value 到相同的 $meta_key。第四個參數 $unique 為 true 時代表不允許有多筆相同的 $meta_key，範例如下：
```php
// add user meta - 3rd parameter is a unique value
add_user_meta( $brian_id, 'bwawwp_kid', 'Dalya' );
add_user_meta( $brian_id, 'bwawwp_kid', 'Brian' );
add_user_meta( $brian_id, 'bwawwp_kid', 'Nina' );
add_user_meta( $brian_id, 'bwawwp_kid', 'Cam' );
add_user_meta( $brian_id, 'bwawwp_kid', 'Aksel' );
```
**delete_user_meta( $user_id, $meta_key, $meta_value="") 刪除使用者資料**

第三個參數當有指定 $meta_value 只會刪除符合該 value 的 $meta_key，範例如下：
```php
// delete brian's user meta
delete_user_meta( $brian_id, 'bwawwp_wife' );
delete_user_meta( $brian_id, 'bwawwp_kid', 'Dalya' ); // 沒指定 Dalya 的話則所有小孩都會被刪除
```
* * *

**（四）wp_posts** \- 存放文章、頁面、選單、文章版本、以及自定義文章的資料表，而自定義文章就是用這個表裡面的 post_type 欄位來紀錄不同的文章類型

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-26-at-09.53.21-1024x515.jpg)

wp_posts 資料表結構

wp_options 相關的 Function 定義在 wp-include/post.php，常用的如下：

**wp_inset_post( $postarr, $wp_error=false) 插入新文章**

$postarr 是一個定義新文章內容的陣列，裡面有許多參數可以定義，$wp_error 為 true 時會在文章插入失敗時回傳一個 WP_Error 物件，範例如下：
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
'ID'  => $post_id,
'post_status' => 'publish'
);
wp_update_post( $args );
```
**get_post( $post=null, $output=OBJECT, $filter='raw') 取得文章資料**

第一個參數指定要取得的文章的 ID，如果是在 post loop 中的話留空會自動取得當前文章。$output 資料格式可以設定為 OBJECT 或是 ARRAY_A (關聯式陣列) 與 ARRAY_N(索引式陣列)。$filter 參數為設定要 sanitize 的模式，可選值有 raw、edit、db、display、attribute 與 js，這是與安全性有關的議題，會在第八章的時候提到。範例如下：
```php
// get post - return post data as an object
$post = get_post( $post_id );
echo 'Object Title: ' . $post->post_title . '<br>';

// get post - return post data as an array
$post = get_post( $post_id, ARRAY_A );
echo 'Array Title: ' . $post['post_title'] . '<br>';
```
**get_posts( $args = null ) 取得文章列表**

當要取得文章清單時，像是最新文章列表、特定分類列表就可以使用這支 Function。它是根據 WP_Query 類別所設計的。$args 參數可以用陣列的方式來提供文章取得條件，範例如下：
```php
// get posts - return 100 posts
$posts = get_posts( array(
  'numberposts' => '100', // 顯示文章數量
  'category'         => 0,  // 文章分類 ID
        'orderby'          => 'date',  // 排序條件
        'order'            => 'DESC',  // 排序方式，DESC 為降冪，ASC 為升冪
        'include'          => array(1,2,3),  // 只顯示特定的文章
        'exclude'          => array(4,5,6),  // 排除指定文章
        'meta_key'         => '',  // 顯示帶有指定欄位的文章
        'meta_value'       => '',  // 顯示帶有指定欄位值的文章
        'post_type'        => 'post',  // 文章類型
        'suppress_filters' => true,  // 如果需要使用 filter 來修改 get_posts 的結果，這個參數要設為 false
)
);
// loop all posts and display the ID & title
foreach ( $posts as $post ) {
    echo $post->ID . ': ' .$post->post_title . '<br>';
}
```
**wp_delete_post( $postid=0, $force_delete =false ) 刪除指定文章**

第一個參數為要刪除的文章 ID，第二個參數如果為 true 的話，文章就不會進垃圾桶而直接被刪除，代表連救回來的機會都沒有，預設值為 false，範例如下：

// delete post - skip the trash and permanently delete it
wp_delete_post( $post_id, true );

* * *

**（五）wp_postmeta** - 文章自訂欄位

如果內建的文章欄位不夠用，想要新增其他欄位的話就會寫在這張表中。WordPress 提供很方便的作法讓你不用再自行增加資料表，每個欄位使用 post_id 來對應到文章。

如果你需要新增欄位但又不想讓使用者在文章編輯畫面看到該欄位，可以用下底線開頭的欄位名稱，這樣 WordPress 會自動在後台隱藏這個欄位而不被使用者看見。

> 寫到這張表的時候就想到一件事，每次使用 ACF 開了一堆欄位，當網站開始上大量的資料之後，這張表就會以飛快的速度開始爆肥，因為所有的自訂欄位全部都會塞在裡面，這對於在 DB Query 上會是不小的負擔。
> 
> 好奇的查了一下有沒有可以把 ACF 欄位獨立成一張表的外掛，想不到還真的有！一間澳洲的公司叫做 Hookturn，他們開發了一隻外掛叫做 [ACF Custom Database Tables](https://hookturn.io/downloads/acf-custom-database-tables/)，看影片介紹非常猛，完全就是我想像的功能，售價 139 鎂，對於有規模的網站來說，絕對比再多開兩台 DB 來得划算的多！
> 
> Hookturn 還有另外兩隻外掛商品，一隻很實用，直接幫你把欄位的 PHP 產好，另外一隻我覺得根本就是 ACF 應該要內建的功能：[前台表單](https://hookturn.io/downloads/advanced-forms-pro/)。它可以把 ACF 的欄位完整呈現在前台，這個拿來做 Web App 超級方便，就不用自己在那邊刻表單刻個老半天了，下一個 Web App 我一定會用它！

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-27-at-09.51.32-1024x126.jpg)

wp_postmeta 資料表結構

**get_post_meta( $post_id, $meta_key, $single=false ) 取得文章欄位值**

第一個參數為文章 ID，第二個為欄位名稱，如果留空值，會回傳所有相同 $meta_key 的 $meta_value，$single 為 true 的話會以字串回傳符合第一個 $meta_key 的 $meta_value，false 的話則會使用陣列回傳所有符合的 $meta_value，範例如下：
```php
// get post meta - get 1st instance of key
$student = get_post_meta( $post_id, 'bwawwp_student', true );
echo 'oldest student: ' . $student;
```
**update_post_meta( $post_id, $meta_key, $meta_value, $prev_value='') 更新文章欄位**

需要注意的是第四個參數，如果 $meta_key 有多筆符合，在 $prev_value 為空的情況下，$meta_value 只會修改第一筆 $meta_key，如果 $prev_value 有符合多筆 $meta_key 的 $meta_value 其中一筆，則這一筆的 value 會被第三個 $meta_value 給取代掉。範例如下：
```php
// update post meta - public metadata
$content = 'You SHOULD see this custom field when editing your latest post.';
update_post_meta( $post_id, 'bwawwp_displayed_field', $content );

// update post meta - hidden metadata
$content = str_replace( 'SHOULD', 'SHOULD NOT', $content );
update_post_meta( $post_id, '_bwawwp_hidden_field', $content );
```
**add_post_meta( $post_id, $meta_key, $meta_value, $unique=false)**

習慣上新增文章欄位會使用 update_post_meta()，因為它有判斷 $meta_key 是否存在的功能，會用 add_post_meta 的情境在於要新增多筆 $meat_value 到相同的 $meta_key。第四個參數 $unique 為 true 時代表不允許有多筆相同的 $meta_key，範例如下：
```php
add_post_meta( $post_id, 'bwawwp_students', $students, true );
```
**delete_post_meta( $post_id, $meta_key, $meta_value='' ) 刪除文章欄位**

第三個參數當有指定 $meta_value 只會刪除符合該 value 的 $meta_key，範例如下
```php
// delete post meta
delete_post_meta( $post_id, 'bwawwp_student' );
```
* * *

**（六）wp_comments** - 紀錄文章留言的資料表

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-30-at-10.05.06-1024x349.jpg)

wp_comments 資料表結構

wp_comments 相關的 Function 定義在 wp-include/comment.php，常用的如下：

**get_comment( $comment, $output=OBJECT ) 根據留言 ID 取得留言內容**

第一個參數可以為留言 ID 或是 OBJECT，第二個為返回格式，可以為 OBJECT、ARRAY_A 與 ARRAY_n。範例如下：
```php
// make comments array
$comments[] = 'ICE CREAM!!!!';
$comments[] = 'Taco Bell';
$comments[] = 'Get a good night sleep';

//loop comments array
foreach ( $comments as $key => $comment ) {
    // insert comments
    $commentdata = array(
        'comment_post_ID' => $post_id,
        'comment_content' => $comments[$key],
    );
    $comment_ids[] = wp_insert_comment( $commentdata );  // 插入留言
}

// get comment - get best comment
$comment = get_comment( $comment_ids[0] );
echo 'best comment: ' . $comment->comment_content;
```
**get_comments( $args='' ) 取得特定文章的留言列表**

該 Funciton 使用 WP_Coomen_Query 類別來取得留言，$args 為選填的陣列或是用 & 符號連結的字串來取留言，常用參數有：

*   author_email 留言者的電子郵件
*   ID 留言 ID
*   karma 留言評價
*   number 顯示留言筆數
*   offset 跳過顯示留言比數
*   orderby 排序條件，可選的值有 'comment_agent','comment_approved','comment_author','comment_author_email','comment_author_IP','comment_author_url','comment_content','comment_date','comment_date_gmt','comment_ID','comment_karma','comment_parent','comment_post_ID','comment_type','user_id','comment__in','meta_value','meta_value_num'
*   order 排序方式，可選值為 DESC 與 ASC
*   parent 父留言的 id (comment 有階層)
*   post_id 特定文章的留言
*   post_author 文章作者
*   post_name 文章名稱
*   post_parent 父文章
*   post_status 文章狀態
*   post_type 文章類型
*   status 留言狀態，可選值有 hode, approve, spam, trash
*   type 留言類型，可選值有 pingback, trackback
*   user_id 留言者的 id
*   search 可針對留言的欄位進行關鍵字搜尋，搜尋範圍有：comment_author, comment_author_email, comment_author_url, comment_author_IP, comment_content
*   count 當為 true 時返回符合 query 條件的留言筆數，false 時返為留言 Object，預設為 false
*   meta_key 留言 meta 的 key
*   meta_value 留言 meta 的 value

範例如下：
```php
$comment_count = get_comments( 'post_id= ' . $post_id . '&count=true' );
echo 'comment count: ' . $comment_count . '<br>';
```
> 一直對 Karma 那個參數很在意，因為從來沒看過 WP 留言內建有什麼評價機制，查了很多都沒有具體的使用範例，最後查到[這一篇](https://wordpress.stackexchange.com/questions/199950/what-is-comment-karma)是說資料表裡面有一個叫 comment_karma 的欄位，是用來做防止垃圾留言機制的，但現在都沒人會用這個欄位來做留言評價的功能，所以我想應該是早期 WordPress 留下來的產物，而為了要相容舊版所以就擺在那邊了，真要做留言評分的話還是用 comment meta 來處理。

**wp_insert_comment( $commentdata ) 新增留言**

$commentdata 為新增留言需要提供的參數，新增完成後會返回留言 ID，範例如下：
```php
// insert comment - sub comment from parent id
$commentdata = array(
'comment_post_ID' => $post_id, // 指定文章
'comment_parent' => $comment_ids[0], // 指定父留言
'comment_content' => 'That is a pretty good idea...', // 留言內容
);
wp_insert_comment( $commentdata );
```
其他參數還有：comment_author, comment_author_email, comment_author_url, comment_author_IP, comment_date, comment_date_gmt, comment_karma, comment_approved, comment_agent, comment_type, comment_parent 與 user_id。

**wp_delete_comment( $comment_id, $force_delete=false) 刪除留言**

需要注意的是第二個參數，為 true 的話就會直接被永久刪除而無法從垃圾桶找回，範例如下：
```php
// delete comment
wp_delete_comment( $comment->comment_ID, true );
```
* * *

**（七）wp_commentmeta** - 留言自訂欄位

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-31-at-10.15.35-1024x124.jpg)

wp_commentmeta 資料表結構

wp_commentmeta 相關的 Function 定義在 wp-include/comment.php，常用的如下：

**get_comment_meta( $comment_id, $meta_key, $single = false ) 取得留言自訂欄位**

第一個參數為留言 ID，第二個為欄位名稱，如果留空值，會回傳所有相同 $meta_key 的 $meta_value，$single 為 true 的話會以字串回傳符合第一個 $meta_key 的 $meta_value，false 的話則會使用陣列回傳所有符合的 $meta_value，範例如下：
```php
// get comment meta - all keys
$comment_meta = get_comment_meta( $comment_id );
echo '<pre>';
print_r( $comment_meta );
echo '</pre>';
```
**add_comment_meta( $comment_id, $meta_key, $meta_value, $unique=false ) 新增留言自訂欄位**

第四個參數 $unique 為 true 時代表不允許有多筆相同的 $meta_key，範例如下：
```php
// add comment meta - meta for view date & IP address
$viewed = array( date( "m.d.y" ), $_SERVER["REMOTE_ADDR"] );
$comment_meta_id = add_comment_meta( $comment_id, 'bwawwp_view_date', $viewed, true );
echo 'comment meta id: ' . $comment_meta_id;
```
**update_comment_meta( $comment_id, $meta_key, $meta_value, $prev_value='') 更新留言自訂欄位**

第四個參數跟其他 xxx_meta 一樣，如果 $meta_key 有多筆符合，在 $prev_value 為空的情況下，$meta_value 只會修改第一筆 $meta_key，如果 $prev_value 有符合多筆 $meta_key 的 $meta_value 其中一筆，則這一筆的 value 會被第三個 $meta_value 給取代掉。範例如下：
```php
// update comment meta - change date format to format like
// October 23, 2020, 12:00 am instead of 10.23.20
$viewed = array( date( "F j, Y, g:i a" ), $_SERVER["REMOTE_ADDR"] );
update_comment_meta( $comment_id, 'bwawwp_view_date', $viewed );
```
**delete_comment_meta( $comment_id, $meta_key, $meta_value='' ) 刪除留言欄位**

第三個參數當有指定 $meta_value 只會刪除符合該 value 的 $meta_key，範例如下：
```php
// delete comment meta
delete_comment_meta( $comment_id, 'bwawwp_view_date' );
```
* * *

**（八）wp_temrs** - 該表放了文章分類 ( Category ) 名稱或是自訂分類項目 ( Term ) 的名稱，每一個分類項目都會綁定一個主分類 ( Taxonomy )。常用的文章分類與文章標籤都是一種主分類，主分類底下新增的叫做分類項目。

> 這邊很容易讓人搞不清楚，因為不管是 Catergory 還是 Taxonomy 中文都叫「分類」，然後中間還多一個叫 term 的東西 Orz...
> 
> 後來我的理解方式是只要記得**主類別 Taxonomy**、**分類項目 Term** 就好，譬如主類別是水果 ( Taxonomy )，分類項目 ( Term ) 有溫熱性水果、涼性水果、中性水果等等，至於 Category 則是文章的主類別，Tag 也是文章的主類別，一個文章類型可以有多個主類別。
> 
> 順帶一提，Category 與 Tag 在技術面上的差異是前者可以有階層關係，而後者只會有一層。

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-31-at-10.53.05.jpg)

文章的分類與標籤

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-31-at-10.56.14-1024x123.jpg)

wp_terms 資料表結構

wp_terms 相關的 Function 定義在 wp-includes/taxonomy.php，常用的如下：

**get_terms( $arg='' ) 取得單一或多個主類別的分類項目**

> 書中說明為 get_terms( $taxonomies, $args= '' )，但查[官方文件](https://developer.wordpress.org/reference/functions/get_terms/)指出這是 WP 4.5 版之前的寫法，之後的寫法要把 $taxonomies 放在 $args 裡面，該出以官方文件為準。

$arg 參數為帶入篩選屬性，相關屬性有：

*   taxonomy - 主分類名稱，可以用 array 帶入多筆主分類
*   orderby - 排序條件，可以是 name、count、term_group、slug，預設是 name
*   order - 排序方式，可選值為 ASC 升冪與 DESC 降冪預設為 ASC
*   hide_empty - 隱藏所屬文章篇數為 0 的分類項目
*   exclude - 排除分類項目
*   exclude_tree 排除分類項目以及它的子分類項目
*   include - 只顯示指定的分類項目
*   number - 設定顯設筆數
*   offset - 設定跳過筆數
*   fields - 選擇返回分類項目 ID 或是名稱，預設是兩個都返回
*   slug - 只顯示符合指定代稱的分類名稱
*   hierarchical - 顯示所以分類項目的子分類項目，預設為 true
*   search - 搜尋符合指定字串為分類項目名稱的結果，英文字母不區分大小寫
*   name_like - 搜尋符合開頭為指定字串的分類項目名稱結果，英文字母不區分大小寫
*   pad_counts - 預設為 true，會返回每個分類項目帶有多少子項目的數量
*   get - 設定為 all 時，會返回所有父項目或是空的分類項目
*   child_of - 取得特定分類項目的子項目
*   parent - 取得子項目的父分類項目
*   cache_domain - 啟用的話會把查詢結果存在 object cache 之中，以節省重複查詢的時間

範例如下：
```php
$terms = get_terms( array(
    'taxonomy' => array('post_tag','category'),
    'hide_empty' => false,
) );
```
**get_term( $term, $taxonomy, $outpug= OBJECT, $filter='raw' ) 取得分類項目的所有資訊**

第一個參數可以是 term ID 或是 term Object，預設值為 0，會回傳所有的項目，第二個參數是該項目所屬的主分類，

$output 資料格式可以設定為 OBJECT 或是 ARRAY_A (關聯式陣列) 與 ARRAY_N(索引式陣列)。$filter 參數為設定要 sanitize 的模式，可選值有 raw、edit、db、display、attribute 與 js，這是與安全性有關的議題，會在第八章的時候提到。範例如下：
```php
$term = get_term( 1 , 'category' );
echo $term->name;  // 顯示分類項目名稱
```
**wp_insert_term( $term, $taxonomy, $args=array() ) 新增分類項目**

第一個參數為分類項目名稱，如果已經存在的話會更新既有的，第二個參數為所屬主分類，第三個為分類項目的資料，可用參數如下：

*   alias_of - 分類項目的別名
*   description - 分類項目的描述
*   parent - 指定所屬父分類項目 ID
*   slug - 分類項目代稱

範例如下：
```php
$parent_term = term_exists( 'fruits', 'product' ); // array is returned if taxonomy is given
$parent_term_id = $parent_term['term_id'];         // get numeric term id
wp_insert_term(
    'Apple',   // the term 
    'product', // the taxonomy
    array(
        'description' => 'A yummy apple.',
        'slug'        => 'apple',
        'parent'      => $parent_term_id,
    )
);
```
**wp_update_term( $term_id, $taxonomy, $args=array() ) 更新分類項目資料**

更新現有分類項目的資料，範例如下：
```php
$update = wp_update_term( 1, 'category', array(
    'name' => 'Non Catégorisé',
    'slug' => 'non-categorise'
) );
```
**wp_delete_term( $term, $taxonomy, $args=array() ) 刪除分類項目**

第三個參數帶有兩個屬性，default 以及 force_default，來決定預設的分類項目。

* * *

**（九）wp_termmeta** - 分類項目的自訂欄位

從 WordPress 4.4 版之後就可以自訂分類項目的欄位，運用的場景可以像是決定分類項目的排列順序，或是其他任何需要擴充分類項目欄位的應用上。

![](https://oberonlai.blog/wp-content/uploads/2020/04/CleanShot-2020-04-01-at-10.04.09-1024x117.jpg)

wp_termmeta 資料表結構

相關的 Function 與其他 user meta、post meta、comment meta 很像，說明如下：

**get_term_meta( $term_id, $meta_key='', $single=false ) 取得分類項目自訂欄位**

第一個參數為留言 ID，第二個為欄位名稱，如果留空值，會回傳所有相同 $meta_key 的 $meta_value，$single 為 true 的話會以字串回傳符合第一個 $meta_key 的 $meta_value，false 的話則會使用陣列回傳所有符合的 $meta_value。

**update_term_meta( $term_id, $meta_key, $meta_value, $prev_value='' ) 更新分類項目自訂欄位**

第四個參數跟其他 xxx_meta 一樣，如果 $meta_key 有多筆符合，在 $prev_value 為空的情況下，$meta_value 只會修改第一筆 $meta_key，如果 $prev_value 有符合多筆 $meta_key 的 $meta_value 其中一筆，則這一筆的 value 會被第三個 $meta_value 給取代掉。

**add_term_meta( $term_id, $meta_key, $meta_value, $unique=false ) 新增分類項目自訂欄位**

第四個參數 $unique 為 true 時代表不允許有多筆相同的 $meta_key。

**delete_term_meta( $term_id, $meta_key, $meta_value='') 刪除分類項目自訂欄位**

第三個參數當有指定 $meta_value 只會刪除符合該 value 的 $meta_key。

* * *

**（十）wp_term_taxonomy** - 存放主分類 Taxonomy 的資料表

當有分類項目 Term 新增的時候，一定會綁定一個主分類 Taxonomy，WordPress 預設的主分類有 category 與 post_tag。

![](https://oberonlai.blog/wp-content/uploads/2020/04/CleanShot-2020-04-02-at-09.21.44-1024x165.jpg)

wp_term_taxonomy 資料表結構

wp_term_taxonomy 相關 Function 定義在 wp-includes/taxonomy.php，常用如下：

**get_taxonomies( $args=array(), $output='names', $operator='and' ) 取得主分類清單**

$args 可以帶入參數來篩選清單，詳細用法第五章會說明，$output 為設定返回格式，預設 names 會返回主分類名稱，或是 objects 返回主分類物件。

$operator 參數可以選擇 and 或是 or，差別在於是否完全符合或是部分符合第一個傳入參數的內容，範例如下：
```php
$args = array(
  'public'   => true,
  '_builtin' => false
   
); 
$output = 'names'; // or objects
$operator = 'and'; // 'and' or 'or'
$taxonomies = get_taxonomies( $args, $output, $operator ); 
if ( $taxonomies ) {
    echo '<ul>';
    foreach ( $taxonomies  as $taxonomy ) {
        echo '<li>' . $taxonomy . '</li>';
    }
    echo '</ul>'; 
}
```
**get_taxonomy( $taxonomy ) 取得特定主分類**

$taxonomy 為主分類名稱，範例如下：
```php
$rental_features = get_taxonomy( 'features' );
print_r( $rental_features );
```
> 這邊提到的主分類名稱或是 name 是指定義 Taxonomy 的英文名，而非顯示在後台的中文名字。

**register_taxonomy( $taxonomy, $object_type, $args=array() ) 註冊新的主分類**

如果你覺得文章只有 category 跟 post_tag 這兩個主分類不夠用，或是自定義文章需要主分類時，就可以用這支來做新增，$taxonomy 為主分類名稱，必須是英文，中文名字會在 $args 裡面的 label 來定義。

$object_type 是定義要把這個主分類綁定到哪一個文章類型上面，$args 有一堆參數來設定主分類的資料，像是上面提到的中文名字就是在這邊設定，更多詳細內容會在第五章介紹，或是看 [Codex 文件](https://codex.wordpress.org/Function_Reference/register_taxonomy)有完整的說明。基本範例如下：
```php
add_action( 'init', 'create_book_tax' );

function create_book_tax() {
    register_taxonomy(
        'genre',  // 主分類名稱
        'book',   // 文章類型
        array(
            'label' => __( '書的分類' ), // 主分類顯示名稱
            'rewrite' => array( 'slug' => 'genre' ),
            'hierarchical' => true,
        )
    );
}
```
> 書中提到之前有在討論是否該移除 wp_terms 這張表，然後把裡面的欄位移到 wp_terms_taxonomy，但後來還是為了相容性而作罷，還好沒移掉，不然手上一堆網站會爆炸@@

* * *

**（十一）wp_term_relationships** - 紀錄 post 與 Taxonomy 綁定關係的資料表

當使用 register_taxonomy() 的時候，第二個參數會指定文章類型，指定後就會紀錄在這張表之中。

![](https://oberonlai.blog/wp-content/uploads/2020/04/CleanShot-2020-04-02-at-09.50.41.jpg)

wp_term_relationships 資料表結構

wp_term_relationships 常用 Function 如下：

**get_object_taxonomies( $object, $output='names' ) 取得特定文章類型所擁有的主分類**

第一個參數為文章類型名稱，第二個參數為設定返回格式，可以是 names 或 objects，範例如下：
```php
$taxonomy_names = get_object_taxonomies( 'post' );
print_r( $taxonomy_names);
```
**wp_get_object_terms( $object_ids, $taxonomies, $args= array() ) 取得特定文章類型特定主分類的分類項目**

第一個參數為文章 ID，可以是字串或陣列。第二個參數是主分類名稱，可以是字串或陣列，第三個是選填的篩選參數，可以用的屬性有：

*   orderby - 排序條件，可依照 slug、term_group、term_order 來進行排序
*   order - 排序方式，可設定 ASC 升冪與 DESC 降冪
*   fields - 分類項目欄位，可選值有 ids、names、slugs 以及 all_with_object_id

範例如下：
```php
$product_terms = wp_get_object_terms( $post->ID,  'product' );
 
if ( ! empty( $product_terms ) ) {
    if ( ! is_wp_error( $product_terms ) ) {
        echo '<ul>';
            foreach( $product_terms as $term ) {
                echo '<li><a href="' . esc_url( get_term_link( $term->slug, 'product' ) ) . '">' . esc_html( $term->name ) . '</a></li>'; 
            }
        echo '</ul>';
    }
}
```
**wp_set_object_terms( $object_id, $terms, $taxonomy, $append=false ) 新增特定文章特定主分類的分類項目**

需要注意的是第四個參數，在預設情況下如果設定的分類項目名撐已經存在，則會自動取代，反之則會保留既有的在新增一筆分類項目。範例如下：
```php
// An array of IDs of categories we want this post to have.
$cat_ids = array( 6, 8 );
 
/*
 * If this was coming from the database or another source, we would need to make sure
 * these were integers:
 
$cat_ids = array_map( 'intval', $cat_ids );
$cat_ids = array_unique( $cat_ids );
 
 */
 
$term_taxonomy_ids = wp_set_object_terms( 42, $cat_ids, 'category' );
 
if ( is_wp_error( $term_taxonomy_ids ) ) {
    // There was an error somewhere and the terms couldn't be set.
} else {
    // Success! The post's categories were set.
}
```
## **Hook，Action 與 Filter**

Hook 說是 WordPress 最重要的機制也不為過，不管是 Theme 還是 Plugin 開發都會需要用到大量的 Hook。什麼是 Hook？簡單說就是先在程式碼裡面貼上一個無痕掛鉤，然後要修改 WordPress 只需把你寫的程式「掛」在這個預先貼好的無痕掛鉤上就可以執行了，外「掛」就是這樣運作的。

跟感冒藥一樣，Hook 也有兩種：Action 與 Filter。

**（ㄧ）Action** - 設計新功能的掛鉤

貼上掛鉤的 Function 叫做 do_action( $tag, $arg )，它可以帶入兩個參數，第一個是掛鉤的名字，第二個是資料的參數，可以有很多個，主要作用是讓新功能掛在這邊的時候可以取得資料。

掛的動作叫做 add_action()，第一個參數是掛鉤名字，第二個是要執行新功能的 Function，下面的例子示範要如何在你的 Theme 或是 Plugin 裡面判斷使用者的登入狀態：
```php
add_action( 'init', 'my_user_check' );

function my_user_check() {
    if ( is_user_logged_in() ) {
        echo "已登入！";
    }
}
```
在 WordPress 核心裡面有一個叫做 init 的掛鉤，它是被貼在當程式初始化的地方，所以只要當程式一載入，就會執行 my_user_check 這隻 Function，而這段程式碼可以放在 Theme 的 function.php 或是 Plugin 的主程式裡面，這樣要判斷會員登入狀態就不用去改 WordPress 核心程式碼。

> 所有可以用的 Action 可參考 ---> [](https://adambrown.info/p/wp_hooks/hook/actions)[https://adambrown.info/p/wp_hooks/hook/actions](https://adambrown.info/p/wp_hooks/hook/actions)，數量足足有七百多個，但常用的大概就那幾個：init、wp_head、wp_footer 等等，其他的就等到需要的時候再去查就好。

**（二）Filter** - 修改內建資料的掛鉤

跟 Actions 一樣的運作邏輯，但與 Actions 的差異可以從名稱上看出來，Filter 是過濾器的意思，也就是把原本會顯示的內容多裝一個過濾器，去修改資料的最後呈現，相關 Function 說明如下：

**apply_filters( $tag, $value) - 裝上過濾器**

第一個參數為過濾器的名字，第二個參數決定什麼資料要被過濾，可以有很多個，同樣的，在核心程式碼中一樣有[一大堆](https://adambrown.info/p/wp_hooks/hook/filters)已經寫好的 Filter 等著你來用。

**add_filter( $tag, $function, $priority, $accepted_args ) 使用過濾器**

第一個參數為要用哪個過濾器，第二個為要修改資料的 Function，第三個是執行順序，預設值是 10，數字越小越先執行，當有很多人同時在用過濾器的時候，就用這個數字來決定使用順序。

第四個參數是可接受的參數數量，預設值是 1，實際上會根據 apply_filters 的 $value 數量來決定這個數字，下面例子說明如何修改每一篇文章標題的格式：
```php
add_filter( 'the_title', 'my_filtered_title', 10, 2 );

function my_filtered_title( $value, $id ) {
    $value = '[' . $value . ']';
    return $value;
}
```
add_filter 第一個是過濾器名字，第二個是要執行的 Function，在這範例中是把文章標題左右各加入中括號，my_filtererd_title 要給兩個參數，因為過濾器 the_title 長這樣：apply_filters( 'the_title', $title, $id )，然後執行順序是預設值 10，最後的 2 是指定過濾器的 $title 與 $id。

$id 這個參數在這範例中沒用到，它可以拿到文章的 ID，所以可以針對文章來進行標題的過濾，範例補充如下：
```php
add_filter( 'the_title', 'my_filtered_title', 10, 2 );

function my_filtered_title( $value, $id ) {
  if( $id === 1)
    $value = '[' . $value . ']';
    return $value;
}
```
這樣就只會修改文章 ID 為 1 的標題帶有中括號。

在實務上為了程式碼的可讀性，要根據使用目的來選擇 Hook，如果是要修改資料的 return 結果，那就是用 Filter，如果是要在某個特定的時間點來執行你寫的功能，就使用 Action。

> 何時要用 Action 還是 Filter 我有時候也搞不清楚，像是移除 WordPress 版本號的顯示是用 Filter，但要關閉後台首頁 Dashboard 預設 Widget 顯示的時候是用 Action。
> 
> 後來我的理解方式是如果要執行某個具體的「動作」就是用 Action，要「修改」某些內容就是用 Filter，萬一還是搞不清楚就是爬原始碼看它這邊是埋 do_action 還是 apply_filters，久了就會慢慢記起來了。

## **本機與主機環境**

WordPress 需要伺服器才能運作，通常在開發上會先使用本機的伺服器環境進行開發，完成後才會放到遠端的主機上讓人瀏覽，以下就本機與主機環境進行說明：

**（一）本機開發環境**

在開發階段使用本機環境來進行會比較有效率，除了可以離線作業以外，在進行新功能的測試時也不會影響到在正式運作的站，等確認無誤後，再透過 FTP 或是版本控管把程式碼上傳到遠端主機中。

網路上有很多適合 WordPress 在本機開發的軟體，像是 [MAMP](https://www.mamp.info/en/mac/) ( Mac / Apache / MySQL / PHP)、[WAMP](http://www.wampserver.com/en/) ( Windows / Apache / MySQL / PHP )，另外還有 [XAMPP](https://www.apachefriends.org/index.html)、[AMPPS](https://www.ampps.com)，他們是跨平台的建置本機開發環境軟體。

此外還有很多是針對 WordPress 的，像是 [DesktopServer](https://serverpress.com)、[Local](https://localwp.com) 都是非常受歡迎的本機開發環境，這些都有圖形化介面提供使用者操作。

其他還有必須透過終端機指令安裝的軟體，像是 VirtualBox、Vagrant 與 Docker，這些工具適合拿來做自動化測試與部署，在 WordPress.org 有詳細的文件來說明 [Installing Varying Vagrant Vagrants](https://make.wordpress.org/core/handbook/tutorials/installing-a-local-server/installing-vvv/) (VVV)。

> 要用哪套本機開發軟體主要看個人的使用習慣以及需求，像我是使用 Mac，最早用 MAMP，中間用過 VVV、Docker，後來改用 Local，但因為我的電腦比較舊、容量又小，這些軟體都會把每一個站開出獨立的伺服器環境，所以害我硬碟常常滿載，直到最近終於找到一套非常輕量的本機軟體，速度也很不錯，還可以把我在本機建立 WordPress 站的流程自動化，有興趣的朋友可以參考這篇--->[Mac 限定 WordPress 本機開發環境 Valet + ValetPress](https://oberonlai.blog/valtpress-setup/)。

（二）網站主機

簡單來說，網站主機就是當訪客連到你的網站時所造訪的主機，網站的運作效能、安全性、承載能力很大一部分是取決於使用的主機商，世界上有爆炸多的網站主機服務商，而他們也都可以運作 WordPress，但並非全部的主機商都有針對 WordPress 來進行系統的優化。

代管式主機商 ( Managed WordPress Hosting ) 才會有提供優化過的主機環境，特別是針對大型的 WordPress 網站，會包含許多系統底層的快取機制，進而加快資料庫的存取，如果網站規模大硬體資源不足，很容易就會造成網站斷線的情況。

WordPress 有內建的快取機制來優化內容的呈現，如果主機商沒有幫你做快取的話，可以再搭配快取外掛來加快瀏覽速度，關於快取第十四章會有更深入的說明。

另外還有一些代管式的主機商會提供更多附加功能，像是自動更新外掛、內容傳遞服務 ( CDN ) 等等，這類的廠商有 WP Engine、WordPress VIP、Pagely 以及其他超多的選擇。

> 我自己的習慣是租 VPS 然後[安裝主機管理面板 Plesk](https://hellowp.cc/how-to-host-wordpress-with-plesk/) 來管理 WordPress，一來比較便宜，二來擁有系統權限可以做的事情比較多，缺點是系統優化、快取那些東西要自己研究，以及網站掛掉的時候要想辦法處理，這樣可以學到很多，對於 WordPress 也會有更深層的認識。

## **開發、測試與正式環境**

通常在網站開發的過程中，會有三種不同階段的環境：開發環境 ( Development )、測試環境 ( Staging ) 與正式環境 ( Production )。理想上這三種環境會是在不同的主機上、有獨立的 WordPress 與資料庫。

開發環境又稱為 Dev 或是 DEV，通常是用來寫程式的本機環境；測試環境又稱為 Testing 或是 TEST，主要用來測試上機後的網站，網站的資料與正式環境盡量保持同步；正式環境又稱為 Live 或是 PRD，是你的訪客會造訪的環境。

如果你沒有辦法建立這三種環境，最起碼要有開發與測試兩種，然後絕對不要在正式環境直接進行修改與測試。最好的做法是所有開發都在本機完成，然後使用版本控管系統來把程式碼推到開發環境上，這樣當有多位工程師協同工作的時候就會有版本紀錄，以避免程式碼被覆蓋。

然後如果公司內部要進行測試時，先把正式環境的資料複製到測試環境，然後再把開發環境的程式碼部署到測試環境，等到都測試無誤後，再把程式碼部署到正式環境。

在部署到正式環境前記得要有備份，萬一網站爆炸時才能立即還原，千千萬萬不要以為寫好的程式碼不用測試就可以正常運作，一定都要經過測試才行。

有些代管式的主機商有提供測試環境，像是 WP Engine，他們會把每一個在上面的 WordPress 站都自動建立測試站，是非常方便的功能。

> 新的專案我的實務做法是開發在本機，然後用 Github Private Repository 做版控，主機上面使用 Plesk 的 Git 套件，先開測試站與正式站，然後用 Plesk 設定當 Git Push 的時候會自動 Deploy 到測試站裡面，待測試無誤後，再使用 Plesk 的 WordPress Clone 功能把測試站完整複製到正式站去，Plesk 會自動把網址做替換。
> 
> 網站上線後的修改，我會先用 Plesk 的同步功能，先把正式站的資料庫跟測試站同步，然後再把開發環境的修改 Deploy 到測試站進行測試。
> 
> 萬一外掛有更新的話，我會統一先在測試站進行更新，確認無誤後再把檔案的部份 ( 不含資料庫 ) 同步到正式站，但有些外掛更新也會動到資料庫，所以檔案同步過去後還是要進正式站的後台去點選資料庫更新。
> 
> 但我常常還是會偷懶，只有我一個人弄的小專案就還是開個 SFTP 然後搭配 VSCode 的 SFTP 套件，直接在存檔時自動上傳，或是用 [Remote Development](https://code.visualstudio.com/docs/remote/remote-overview) 直接 SSH 進主機修改，如果檔案有在本機 Git 一定會做一下，不然就是 Time Machine 備份硬碟隨時插著，以防萬一。

第二章介紹了 WordPress 的基礎知識，包含如何建立環境、資料的存放方式，以及一些操作資料的 Function，另外還有最重要的 Hook。接下來進入 WordPress 最強大的部分：外掛 Plugin。