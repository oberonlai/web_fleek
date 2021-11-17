---
title: 【 書摘 】Building Web Apps with WordPress - 使用 WordPress API、物件、與實用函式
tags:
  - crontab
  - dashborard
  - file header api
  - heartbeat api
  - rewrite rule
  - shortcode
  - wget
  - widget
  - wp_mail
  - wp-cron
id: '1396'
categories:
  - WordPress
  - WordPress Plugins
date: 2020-05-11 08:48:26
etoc: true
---

## Shortcode API

Shortcode 是在後台編輯器中用來產生動態內容的一段短代碼，它有三種形式：

*   直接顯示內容，像是 [myshortcode]
*   帶有屬性，像是 [myshortcode id="1" type="text"]
*   像 HTML 一樣有前後標籤包內文包住，像是 [myshortcode id="1"] 這邊是文字內容...[/myshortcode]

> Shortcode 的使用場景我通常是用來提供給不熟悉 HTML 與 CSS 的客戶，在上文章內容的時候可以使用我們幫他設計好的 UI 元件，我將 UI 元件的 HTML 包裝成 shortcode，所以只要打 [title text="標題文字"]，就會出現 h2 並帶有特定 class 的標題文字。

建立 Shortcode 的第一個步驟，先使用 add_shortcode() 來定義 callback function，需要使用在 Shortcode 裡面的屬性要以陣列的格式傳入 callback function 的第一個參數，第二個參數為要被 Shortcode 包住的內容，下面範例說明如何新增一個帶有屬性以及內容的 Shortcode：

<!--more-->

```php

<?php
/*
  shortcode callback for [msg] shortcode
  Example: [msg type="error"]This is an error message.[/msg]
  Output:
  <div class="message message-error">
      <p>This is an error message.</p>
  </div>
*/
function sp_msg_shortcode($atts, $content)
{
  // 屬性的預設值
  extract( shortcode_atts( array(
        'type' => 'information',
), $atts ) );
  $content = do_shortcode($content);//allow nested shortcodes
  $r = '<div class="message message-' .
    $type . '"><p>' . $content . '</p></div>';
  return $r;
}
add_shortcode('msg', 'sp_msg_shortcode');  // msg 為 Shortcode 名稱
```
在 callback 裡面要返回值使用 return 優於 echo，因為 Shortcode filter 會在還沒有任何內容產生前執行，如果是用 echo 的話，輸出的結果會跑到頁面最上方而非在你想要的位置。

### Shortcode 屬性

上面範例中使用 shortcode_atts() 來處理要帶入的屬性，它有三個參數傳入方式：$pairs、$atts 與 $shortcode。$pairs 是屬性預設值的陣列，使用 key / value 的方式來指定屬性名與預設值，範例中只有一個屬性，而預設值為 information。

而 $atts 則是定義屬性的陣列，shortcode_atts() 會把 $pairs 跟 $atts 合併為一個陣列，所以這兩個參數為必填。第三個參數 $shortcode 為選填，如果設定該 shortcode 名稱 ( 範例為 msg )，那麼 filter shortcode_atts_msg 就會被觸發，這樣其他的外掛就可以用這支 filter 去取代該 shortcode 的屬性預設值。

shortcode_atts() 的結果再使用 PHP 的 extract() 來處理，它可以自動把陣列中的 key 直接轉換成變數，之後就能用在 $content 裡面，因此 echo $type 會輸出 information。

### 巢狀 Shortocde

範例中的 $content = do_shortcode($content) 讓你可以在 Shortcode 裡面再放入另外一組 Shortcode，譬如有另外一個 Shortcode 叫做 [help_link]，則可以這樣寫：

[msg type="error"] 這裡放一些文字 [help_link] 這裡是連結 [/msg]

需要注意的是如果是把相同名稱的 Shortcode 放進裡面會爆炸，因為 WordPress 是用正規表示法來解析 Shortcode 的內容，它會把 Shortcode 裡面的資料做解析，如果是自己包自己，會大幅影響解析的速度進而產生問題，因此比較好的作法是不要用巢狀 Shortcode，或是使用別的 Shortcode 名稱來指定同一個 callback function，不然就是要自己寫正規法來判斷。

do_shortcode 還可以拿來顯示自訂欄位、自訂資料表的資料或是其他還沒有被 the_content filter 所執行的內容。通常情況下，會用 apply_filters( "the_content" , $content ) 來做比較適合，它會執行所有在 the_content filter 上的 hook，當然也包含 Shortcode 的 filter。
```php
<?php
global $post;
$sidebar_content = $post->sidebar_content;
?>
<div class="post">
  <?php the_content(); ?>
</div>
<div class="sidebar">
  <?php
    //echo do_shortcode($sidebar_content);
    echo apply_filters('the_content', $sidebar_content); // 通常用這個
  ?>
</div>
```
> do_shortcode() 最常用到的地方就是當需要把 Shortocde 寫在 php 裡面的時候。依照作者的範例，如果要在 php 顯示 msg 這個 Shortcode 可以這樣寫：<?php echo do_shortcode("[msg type='error']這裡是一些文字[/msg]"); ?>
> 
> 至於何時該用 apply_filters 還是 do_shortcode 來顯示 Shortcode，[這一篇](https://wordpress.stackexchange.com/questions/173844/apply-filtersthe-content-content-vs-do-shortcodecontent)有很好的解釋，簡單說看你是要單純顯示 Shortcode 就好，還是要把除了 Shortcode 以及其他掛在 the_content 的 filter 都顯示出來，需要後者的話就使用 apply_filters( 'the_content', $content) 吧～

### 移除 Shortcode

使用 remove_shortcode() 移除單一已註冊的 Shortcode，使用 remove_all_shortcodes() 移除所有 Shortcode。在執行 remove_shortcode() 的時候，要確保它是放在已註冊 Shortcode 完成後才執行，不然會抓不到，使用 hook 的 priority 參數來進行設定。

已註冊的 Shortcode 會被放在全域變數 $shortcode_tags 裡面，可以直接操作這個變數來改變 Shortcode 狀態。譬如說，如果你想要讓某些內容不能停用某些 Shortcode，可以先把 $shortcode_tags 裡面的值做備份，然後判斷在特定頁面下存新的值進去，範例如下：
```php
<?php 
// 製作 Shortcode 備份
global $shortcode_tags;
$original_shortcode_tags = $shortcode_tags;

// 移除 Shortcode msg 
unset($shortcode_tags['msg']);

// 輸出內容，這時候 msg 是無法使用的
$content = do_shortcode($content);
echo $content;

// 還原所有 Shortcode
$shortcode_tags = $original_shortcode_tags;
```
### 其他實用的 Shortcode Function

**shortcode_exists( $tag )**  
判斷特定 Shortcode 有沒有被註冊

**has_shortcode( $content, $tag )**  
判斷特定 Shortcode 有沒有在 $content 裡面被使用

**shortcode_parse_atts( $text )**  
當要取得 Shortcode 的屬性時，可以把整段 Shortcode 帶入這支 function 的參數，就可以得到 key/value 形式的 Shortcode 屬性陣列，範例如下：
```php
<?php
$atts = shortcode_parse_atts(
  '[soundcloud url="http://api.soundcloud.com/tracks/67658191" params="" width=" 100%" height="166" iframe="true" /]'
);

echo $atts['url']; // 輸出 http://api.soundcloud.com/tracks/67658191
```
**strip_shortcodes( $content )**  
把內文裡面的 Shortcode 全部移除

## Widget API

Widget 就是在後台 > 外觀 > 小工具 裡面的元件，但在開發 Widget 前，可以仔細思考一下是否真的需要另外開發它，因為 WordPress 本身已經有許多內建的 Widget，研究一下它們，也許你的需求都能透過它們來解決。

### 新增 Widget

要新增 Widget 你必須要繼承一支 class 叫做 WP_Widget，它被定義在 wp-includes/widgets.php 裡面，有四個 methods 需要被覆寫，程式碼如下：
```php
<?php
/*
  Taken from the Widgets API Codex Page at:
  http://codex.wordpress.org/Widgets_API
*/
class My_Widget extends WP_Widget {

public function __construct() {
// widget actual processes
}

public function widget( $args, $instance ) {
// outputs the content of the widget
}

 public function form( $instance ) {
// outputs the options form on admin
}

public function update( $new_instance, $old_instance ) {
// processes widget options to be saved
}
}
add_action( 'widgets_init', function(){
     register_widget( 'My_Widget' );
});
```
add_action 第二個參數可以帶匿名函式是 PHP 5.3 之後才有支援，而 WordPress 最低安裝需求為 PHP 5.2.4。另一個方法是用 create_function，可以相容較舊的 PHP 版本。
```php
<?php
/*
  Taken from the Widgets API Codex Page at:
  http://codex.wordpress.org/Widgets_API
*/
add_action('widgets_init',
     create_function('', 'return register_widget("My_Widget");')
);
```
作者外掛 SchoolPress 使用 Widget 來顯示所有班級或是指定班級 ( BuddyPress 的 Group )中的筆記，可以讓社團管理者 ( 老師 ) 自行設定：
```php
<?php
/*
  Widget to show the current class note.
  Teachers (Group Admins) can change note for each group.
  Shows the global note set in the widget settings if non-empty.
*/
class SchoolPress_Note_Widget extends WP_Widget
{
  public function __construct() {
    parent::__construct(
    'schoolpress_note',
'SchoolPress Note',
array( 'description' => 'Note to Show on Group Pages' );
  }

  public function widget( $args, $instance ) {
global $current_user;

//saving a note edit?
if ( !empty( $_POST['schoolpress_note_text'] )
&& !empty( $_POST['class_id'] ) ) {
  //make sure this is an admin
  if(groups_is_user_admin($current_user->ID,intval($_POST['class_id']))){
//should escape the text and possibly use a nonce
update_option(
'schoolpress_note_' . intval( $_POST['class_id'] ),
$_POST['schoolpress_note_text']
);
  }
}

//look for a global note
$note = $instance['note'];

//get class id for this group
$class_id = bp_get_current_group_id();

//look for a class note
if ( empty( $note ) && !empty( $class_id ) ) {
$note = get_option( "schoolpress_note_" . $class_id );
}

//display note
if ( !empty( $note ) ) {
?>
<div id="schoolpress_note">
<?php echo wpautop( $note );?>
</div>
<?php

//show edit for group admins
if ( groups_is_user_admin( $current_user->ID, $class_id ) ) {
?>
<a id="schoolpress_note_edit_trigger">Edit</a>
<div id="schoolpress_note_edit" style="display: none;">
<form action="" method="post">
<input type="hidden"
name="class_id"
value="<?php echo intval($class_id);?>" />
<textarea name="schoolpress_note_text" cols="30" rows="5">
<?php echo esc_textarea(get_option('schoolpress_note_'.$class_id))
;?>
</textarea>
<input type="submit" value="Save" />
<a id="schoolpress_note_edit_cancel" href="javascript:void(0);">
Cancel
</a>
</form>
</div>
<script>
jQuery(document).ready(function() {
jQuery('#schoolpress_note_edit_trigger').click(function(){
jQuery('#schoolpress_note').hide();
jQuery('#schoolpress_note_edit').show();
});
jQuery('#schoolpress_note_edit_cancel').click(function(){
jQuery('#schoolpress_note').show();
jQuery('#schoolpress_note_edit').hide();
});
});
</script>
<?php
}
}
  }

  public function form( $instance ) {
if ( isset( $instance['note'] ) )
$note = $instance['note'];
else
$note = "";
?>
<p>
<label for="<?php echo $this->get_field_id( 'note' ); ?>">
<?php _e( 'Note:' ); ?>
</label>
<textarea id="<?php echo $this->get_field_id( 'note' ); ?>"
name="<?php echo $this->get_field_name( 'note' ); ?>">
<?php echo esc_textarea( $note );?>
</textarea>
</p>
<?php
  }

  public function update( $new_instance, $old_instance ) {
$instance = array();
$instance['note'] = $new_instance['note'];

return $instance;
  }
}
add_action( 'widgets_init', function() {
  register_widget( 'SchoolPress_Note_Widget' );
} );
```
### 定義 Widget 區塊

要讓你的 Theme 可以支援顯示 Widget 要做兩件事：第一個先要註冊 Widget 區塊，然後才能讓它顯示在前端頁面。要註冊 Widget 區塊使用 register_sidebar( $args )，只能接收一個陣列參數，參數內容說明如下：
```php
<?php
/**
 * Add a sidebar.
 */
function wpdocs_theme_slug_widgets_init() {
  register_sidebar( array(
    'name'          => __( 'Main Sidebar', 'textdomain' ),    // 區塊名稱，重複名字會+1
    'id'               => 'sidebar-1',                           // 區塊 ID
    'description'   => __( 'Description', 'textdomain' ),     // 區塊說明
    'before_widget' => '<li id="%1$s" class="widget %2$s">',  // 包住區塊的開頭 HTML
    'after_widget'  => '</li>',                               // 包住區塊的結尾 HTML
    'before_title'  => '<h2 class="widgettitle">',            // 包住區塊標題的開頭 HTML
    'after_title'   => '</h2>',                               // 包住區塊標題的結尾 HTML
  ) );
}
add_action( 'widgets_init', 'wpdocs_theme_slug_widgets_init' );
```
> 可能會有些朋友把新增 Widget 跟新增 Widget 區塊搞混，Widget 區塊是定義要顯示 Widget 的地方，一個區塊可以有很多個 Widget 放在裡面，而 Widget 則是單一小工具，是要被放在 Widget 區塊裡面的東西。

[![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-18-at-09.40.44-1024x372.jpg)](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-18-at-09.40.44.jpg)

Widget 區塊註冊完成之後，接下來就是要來顯示它，使用的是 dynamic_sidebar( $arg ) 這個 function，$arg 參數帶的是 Widget 區塊 name 或 ID，在呼叫前一樣可以用它來判斷 Widget 區塊是否有放入 Widget，沒有的話就可以顯示預設的內容：
```php
<?php
if(!dynamic_sidebar('schoolpress_student_status'))
{
  //fallback code in case my_widget_area sidebar was not found
}
```
如果要判斷 Widget 區塊是否有被註冊，可以使用 is_active_sidebar( $arg )，$arg 參數帶的是 Widget 區塊 name 或 ID：
```php
<?php
//from twenty-thirteen/sidebar.php
if ( is_active_sidebar( 'sidebar-2' ) ) : ?>
<div id="tertiary" class="sidebar-container" role="complementary">
<div class="sidebar-inner">
<div class="widget-area">
<?php dynamic_sidebar( 'sidebar-2' ); ?>
</div><!-- .widget-area -->
</div><!-- .sidebar-inner -->
</div><!-- #tertiary -->
<?php endif; ?>
```
### 不在 Widget 區塊顯示 Widget

如果你已經明確知道你想要把某個 Widget 放在特定的位置，那麼就不需要使用 Widget 區塊來顯示 Widget，可以用 the_widget( $widget, $instance, $args ) 這支 function 來直接顯示它，並且可以透過參數來指定 Widget 的設定值，參數說明如下：

**$widget** - 要顯示的 Widget 類別名稱

**$instance** - 要顯示的 Widget 設定的資料，以陣列表示

**$args** - 要顯示的 Widget 的 HTML 標籤設定，有 before_widget、after_widget、before_title、after_title

以下為作者外掛 SchoolPress 顯示筆記 Widget 的範例：
```php
<?php
//show note widget, overriding global note
the_widget('SchoolPress_Note_Widget',  //classname
    array('note'=>''),                //instance vars
    array(                       //widget vars
    'before_widget' => '',
        'after_widget' => '',
        'before_title' => '',
        'after_title' => ''
    )
);
```
## Dashboard Widget API

控制台 Widget 是當管理者登入 WordPress 後台後第一眼所看到的各種小工具，預設狀態下會有網站概況、網站活動、快速草稿等等的 Widget，想要管理這些 Widget 就必須使用 Dashboard Widget API。

### 移除控制台小工具

控制台小工具主要是用 meta box 來進行設定，下面是每個內建小工具名稱：
```php
<?php
// Main column (left): 
// 更新瀏覽器通知
$wp_meta_boxes['dashboard']['normal']['high']['dashboard_browser_nag']; 
// 更新 PHP 版本通知
$wp_meta_boxes['dashboard']['normal']['high']['dashboard_php_nag']; 
 
// 網站概況
$wp_meta_boxes['dashboard']['normal']['core']['dashboard_right_now'];
// 多站網站概況
$wp_meta_boxes['dashboard']['normal']['core']['network_dashboard_right_now'];
// 網站活動
$wp_meta_boxes['dashboard']['normal']['core']['dashboard_activity'];
// 網站狀態
$wp_meta_boxes['dashboard']['normal']['core']['health_check_status'];
 
// Side Column (right): 
// WordPress 活動及新聞
$wp_meta_boxes['dashboard']['side']['core']['dashboard_primary'];
// 快速草稿
$wp_meta_boxes['dashboard']['side']['core']['dashboard_quick_press']; 
```
如果要移除它們的話，使用 remove_meta_box( $id, $screen, $context ) 這支 function，參數 $id 為小工具的名稱，像是 dashboard_activity、health_check_status，$screen 為要移除的 meta box 的所在頁面，所以是 dashboard，$context 為小工具的位置，可能為 normal、side、advanced。

如果要移除所有預設的控制台小工具，可以使用 wp_dashboard_setup 這支 action，範例如下：
```php
function wporg_remove_all_dashboard_metaboxes() {
    // Remove Welcome panel
    remove_action( 'welcome_panel', 'wp_welcome_panel' );
    // Remove the rest of the dashboard widgets
    remove_meta_box( 'dashboard_primary', 'dashboard', 'side' );
    remove_meta_box( 'dashboard_quick_press', 'dashboard', 'side' );
    remove_meta_box( 'health_check_status', 'dashboard', 'normal' );
    remove_meta_box( 'dashboard_right_now', 'dashboard', 'normal' );
    remove_meta_box( 'dashboard_activity', 'dashboard', 'normal');
}
add_action( 'wp_dashboard_setup', 'wporg_remove_all_dashboard_metaboxes' );
```
> 因為書中範例比較舊，有些 Widget 已經不存在了，所以這邊的範例是使用官方 Handbook 的說明，新版的可以參考這邊--->[https://developer.wordpress.org/apis/handbook/dashboard-widgets/](https://developer.wordpress.org/apis/handbook/dashboard-widgets/)

如果有啟用多站網路功能的話，會有一些不同的控制台小工具，要全部移除的話參考下面範例：
```php
<?php
//Remove network dashboard widgets
function sp_remove_network_dashboard_widgets()
{
  remove_meta_box('network_dashboard_right_now', 'dashboard-network', 'normal');
  remove_meta_box('dashboard_plugins', 'dashboard-network', 'normal');
  remove_meta_box('dashboard_primary', 'dashboard-network', 'side');
  remove_meta_box('dashboard_secondary', 'dashboard-network', 'side');
}
add_action('wp_network_dashboard_setup', 'sp_remove_network_dashboard_widgets');
```
使用 remove_meta_box( $id, $screen, $context ) 也可以移除其他後台頁面的顯示，只要在 $screen 參數這邊指定不同的位置即可，$screen 可以是字串或是用陣列來指定多個頁面，$screen 可選參數如下：

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-27-at-09.08.42.jpg)

Contributed by [Rami Yushuvaev](https://profiles.wordpress.org/ramiy/)

### 新增控制台小工具

可以使用 wp_add_dashboard_widget() 來新增控制台小工具，它利用了 add_meta_box() 來建立，wp_add_dashboard_widget() 帶有五個參數：

**$widget_id** - 必填，小工具 ID，可以提供給 CSS 作為樣式選擇的類別名稱

**$widget_name** - 必填，小工具名稱，會顯在在小工具的視窗的標頭

**$callback** - 必填，輸出小工具的顯示內容

**$control_callback** - 選填，設定小工具的控制功能

**$callback_args** - 選填，要提供給 callback function 的參數

下面範例作者使用 wp_add_dashboard_widget() 來顯示學生作業的提交狀態：
```php
<?php
/*
    Add dashboard widgets
*/
function sp_add_dashboard_widgets() {
wp_add_dashboard_widget(
'schoolpress_assignments',
'Assignments',
'sp_assignments_dashboard_widget',
'sp_assignments_dashboard_widget_configuration'
);
}
add_action( 'wp_dashboard_setup', 'sp_add_dashboard_widgets' );

/*
Assignments dashboard widget
*/
//widget
function sp_assignments_dashboard_widget() {
$options = get_option( "assignments_dashboard_widget_options", array() );

if ( !empty( $options['course_id'] ) ) {
$group = groups_get_group( array(
'group_id'=>$options['course_id']
) );
}

if ( !empty( $group ) ) {
echo "Showing assignments for class " .
$group->name . ".<br />...";
/*
get assignments for this group and list their status
*/
}
else {
echo "Showing all assignments.<br />...";
/*
get all assignments and list their status
*/
}
}
//configuration
function sp_assignments_dashboard_widget_configuration() {
//get old settings or default to empty array
$options = get_option( "assignments_dashboard_widget_options", array() );

//saving options?
if ( isset( $_POST['assignments_dashboard_options_save'] ) ) {
//get course_id
$options['course_id'] = intval(
$_POST['assignments_dashboard_course_id']
);

//save it
update_option( "assignments_dashboard_widget_options", $options );
}

//show options form
$groups = groups_get_groups( array( 'orderby'=>'name', 'order'=>'ASC' ) );
?>
<p>Choose a class/group to show assignments from.</p>
<div class="feature_post_class_wrap">
<label>Class</label>
<select name="assignments_dashboard_course_id">
<option value="" <?php selected( $options['course_id'], "" );?>>
All Classes
</option>
<?php
$groups = groups_get_groups( array( 'orderby'=>'name',
'order'=>'ASC' ) );

if ( !empty( $groups ) && !empty( $groups['groups'] ) ) {
foreach ( $groups['groups'] as $group ) {
?>
<option value="<?php echo intval( $group->id );?>"
<?php selected( $options['course_id'], $group->id );?>>
<?php echo $group->name;?>
</option>
<?php
}
}
?>
</select>
</div>
<input type="hidden" name="assignments_dashboard_options_save" value="1" />
<?php
}
```
呈現效果如下：

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-27-at-09.29.38.jpg)

> 這個範例個人覺得不太好，一次做太多事了，可以參考這篇 [https://www.sitepoint.com/wordpress-dashboard-widgets-api/](https://www.sitepoint.com/wordpress-dashboard-widgets-api/) 裡面的 Displaying an RSS Feed in a Dashboard Widget 會比較單純的多。

### 設定頁面 API

如果你是在開發外掛的話，有時候會需要提供一些設定項目來讓使用者改變外掛的功能，這個時候就可以用設定頁面 API ( Settings API ) 來設計外掛的設定選單與表單。

但在使用它之前，可以先思考是否有必要另外新增頁面來處理這些設定，如果是沒有要讓使用者自行修改的話，直接使用全域變數 global 來存放設定值是比較好的方法：
```php
<?php
global $schoolpress_settings;
$schoolpress_settings = array(
  'info_email' => 'info@schoolpress.me',
  'info_email_name' => 'SchoolPress'
);
```
以下四種情境可能不需要用到設定頁面：

1.  外掛只會在團隊內部使用
2.  外掛設定的修改者是開發者
3.  外掛設定的內容不會因為主機環境不同而有所不同
4.  外掛設定的內容只是為了前一版本的修改

除了把設定項存在 global 外，也可以考慮新增 Hook 或 filter 來改變設定內容。舉例：有一支可以產出 Word 檔的外掛需要限制特定的管理權限才可以使用，如果是用設定頁面來做的話，可能會有所有權限 ( Capability ) 的 Checkbox 來提供給管理者勾選，被勾選到的才能使用產出 Word 檔。

在這種情況下自訂一個 apply_filter 來讓開發者可以把允許的權限 Hook 到這個 filter 上會更方便，自訂 filter 的範例如下：
```php
<?php
//don't require any caps by default, but allow developers to add checks
$caps = apply_filters('wpdoc_caps', array());

if(!empty($caps))
{
  //guilty until proven innocent
  $hascap = false;

  //must be logged in to have any caps at all
  if(is_user_logged_in())
  {
    //make sure the current user has one of the caps
    foreach($caps as $cap)
    {
      if(current_user_can($cap))
      {
        $hascap = true;
        break;  //stop checking
      }
    }
  }

  if(!$hascap)
  {
    //don't show them the file
    header('HTTP/1.1 503 Service Unavailable', true, 503);
    echo "HTTP/1.1 503 Service Unavailable";
    exit;
  }
}
```
接下來使用者 ( 開發者 ) 就可以使用 wpdoc_caps 這個 filter 來寫入允許的 capability：
```php
<?php
 //require any user account
add_filter('wpdoc_caps', function($caps) { return array('read'); });

//require admin account
add_filter('wpdoc_caps', function($caps) { return array('manage_options'); });

//authors only or users with a custom capability (doc)
add_filter('wpdoc_caps', function($caps) { return array('edit_post', 'doc'); });
```
### 新增原生設定頁面

在經過審慎評估後，我們就可以來新增設定頁面，通常有兩種作法，一種是使用 WordPress 提供的 Settings API，這樣做好處是可以讓其他開發者比較好理解，甚至針對你的外掛寫附加套件。

使用 Settings API 同時也確保在介面的呈現上與 WordPress 後台有一致的風格，你不會希望當使用者要使用你的外掛時還必須要學習新的操作介面。

### 新增客製化設定頁面

如果設定頁面單純，用 WordPress 原生設定介面就可以處理的話當然最好，但有時候外掛功能很多，設定項目包含許多不同的面向，需要把他們進行分類，這時候就必須考慮客製化設定頁面。

雖然 Settings API 很靈活，可以針對個別區塊、欄位來顯示設定內容，在只需要一兩個設定區塊的狀況下是非常好用的，但如果是要開發一套應用程式的設定頁面，你可能需要不同的邏輯來顯示它們，客製化設定頁面是唯一辦法。

客製化設定頁面不會太困難，只要新增選單並且使用自訂設定頁面的 PHP 檔後，再透過 callback function 來呼叫它們，以及照著下方的原則來處理即可：

*   即使你的設定頁面是自己手刻 Layout，也還是可以使用原生的設定區塊與項目
*   絕對要記得處理表單資料的 sanitize 與同源驗證的 nonces 來確保設定頁的安全性
*   適時的加入 Hook 來提供給其他開發者使用
*   使用與 WordPress 相同的 HTML 標籤與 Class 名稱，以確保在介面上風格可以一致

客製化設定頁面的設計可以參考購物車外掛的介面，因為它們是功能很多的外掛，設定項目也相對複雜許多，可以餐考 WooCoomerce 與 Paid Memberships Pro 等同類型的外掛。

> Settings API 的具體用法可以參考 Plugin Handbook 裡面的 Settings 章節 ---> [https://developer.wordpress.org/plugins/settings/settings-api/](https://developer.wordpress.org/plugins/settings/settings-api/)

## Rewrite API

通常伺服器會有覆寫網址的功能來改變網址的結構，如果是 Apache 的話是使用 mod_rewrite 模組，設定覆寫規則的檔案是 .htaccess，WordPress 標準的 Rewrite 規則如下：
```php
\# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
# END WordPress
```
如果想要理解每一行在表示什麼可以參考 David Walsh 的這一篇文章：[https://davidwalsh.name/wordpress-htaccess](https://davidwalsh.name/wordpress-htaccess)

一般來說，這些規則主要是在負責把所有使用者瀏覽我們網站所造訪的網址，統一轉到 index.php 這個檔案，然後 WordPress 會透過內建的路由機制，去解析造訪網址然後找到對應的模板後顯示出來。

譬如說當使用者輸入網址 https://abc.com/about 的時候，會先去到 https://abc.com/index.php，然後 WordPress 會去尋找頁面名稱帶有 about 的頁面或文章，找到後再返回給使用者進行瀏覽。

在預設情況下 WordPress 就會完成這樣的路由機制，所以當你想要指定一些特定網址去做某些事，就是 Rewrite API 派上用場的時候了。

### 新增 Rewrite Rule

可以透過 add_rewrite_rule( $rule, $rewrite, $position ) 來直接新增，參數說明如下：

**$rule** - 使用正規表達式來找尋網址中符合條件的目標

**$rewrite** - 找到目標後覆寫的網址內容，可以使用 $matches 陣列帶入 $rule 找到的字串

**$position** - 設定該 Rewrite Rule 的權重是要在 WordPress 內建的之上 ( top ) 或之下 ( bottom )

> 要注意 Rewrite 跟 Redirect 是不一樣的：Rewrite 是把同個頁面的網址做結構上的改變，處理完之後不會去其他頁面，還是會停留在同一頁，而 Redirect 是改變網址後跳轉到其他頁面。

如果你想讓你的聯絡表單網址，可以使用目錄的方式直接帶入表單主旨，像是 https://abc.com/contact/special-offer，當使用者造訪這個網址的時候，進入表單頁面時主旨欄位就會自動輸入 special-offer 這幾個字，範例如下：
```php
<?php
add_rewrite_rule(
  '/contact/([^/]+)/?',
  'index.php?name=contact&subject=' . $matches[1],
  'top'
);
add_rewrite_rule(
flush_rewrite_rules();
```
> 正規表達式是使用 Rewrite API 的必備技能，如果你跟我一樣對它很苦手的話，我都用這個軟體作弊XD ---> [https://www.apptorium.com/expressions](https://www.apptorium.com/expressions)，它超級好用，只要輸入條件就會立即顯示是否有符合的字串，我常常就是亂湊一通就慢慢能湊到我要的寫法，大推！

### 刷新 Rewrite Rule

WordPress 預設會把 Rewrite rule 加到快取機制裡面，所以當新增的時候記得要刷新 Rewrite rule 才會產生作用，如果是開發外掛的話，要保存自己寫的 rule 通常會以下面三種方式來新增：

1.  啟用外掛時新增 rule，同時間使用 flush_rewrite_rules() 來刷新它
2.  使用 Hook init 來新增 rule，以防被後台的永久連結設定或是其他的外掛所刷新
3.  在停用外掛時使用 flush_rewrite_rules() 來移除 rule

具體範例如下：
```php
<?php
//Add rule and flush on activation.
function sp_activation()
{
add_rewrite_rule(
  '/contact/([^/]+)/?',
  'index.php?name=contact&subject=' . $matches[1],
  'top'
);
flush_rewrite_rules();
}
register_activation_hook(__FILE__, 'sp_activation');

/*
  Add rule on init in case another plugin flushes,
  but don't flush cause it's expensive
*/
function sp_init()
{
add_rewrite_rule(
  '/contact/([^/]+)/?',
  'index.php?name=contact&subject=' . $matches[1],
  'top'
);
}
add_action('init', 'sp_init');

//Flush rewrite rules on deactivation to remove our rule.
function sp_deactivation()
{
flush_rewrite_rules();
}
register_deactivation_hook(__FILE__, 'sp_deactivation');
```
### 其他 Rewrite 相關 function

除了上面介紹的 add_rewrite_rule() 以外，WordPress 還提供了其他 function 來改寫 URL：

**add_rewrite_tag()** - 在網址結構中加入特定資料

**add_custom_endpoint()** - 在網址最後加入資料

**add_feed()** - 自訂 RSS 網址的結構

> 推薦 Envato Tuts+ 的這篇文章 [https://code.tutsplus.com/articles/the-rewrite-api-the-basics--wp-25474](https://code.tutsplus.com/articles/the-rewrite-api-the-basics--wp-25474)，每個用法都有具體的範例跟說明，而且解釋的比書中以及 Codex 來得清楚許多。

## WP-Cron

Cron 是作業系統內建的定時器，只要設定好時間就能自動執行指定的任務，通常會用在像是每天自動發送業績報表給管理員、同步從第三方 API 獲取的資料、CPU 的系統監控回報等等的情境上。

而 WP-Cron 是 WordPress 內建的定時器，要使用 WP-Cron 有三個步驟，第一個是建立定時器並設定執行時間，第二是把要執行的 function 掛到這個定時器裡面，最後是把實際要執行的任務寫入這個 function 之中，範例如下：
```php
<?php
// 當外掛啟用時建立定時器
function sp_activation()
{
//do_action('sp_daily_cron'); 定時器名稱叫做 sp_daily_cron
   if ( ! wp_next_scheduled( 'sp_daily_cron' ) ) {
      wp_schedule_event( strtotime( '12:00:00' ), 'daily', 'sp_daily_cron' );
   }
}
register_activation_hook(__FILE__, 'sp_activation');

// 停用外掛時移除定時器
function sp_deactivation()
{
wp_clear_scheduled_hook('sp_daily_cron');
}
register_deactivation_hook(__FILE__, 'sp_deactivation');

// 要執行的任務
function sp_daily_cron()
{
//do this daily
}
add_action("sp_daily_cron", "sp_daily_cron");  // 把任務放到定時器裡面
```
建立定時器使用 wp_schedule_event( $timestamp, $recurrence, $hook, $args ) 這支 function，參數 $timestamp 為第一次執行的時間，通常使用 time() 即可，$recurrence 為多久後再執行一次的間隔，可以是每小時 hourly、每天 daily、每兩天 twicedaily，如果要自訂間隔可以使用來指定週期。

參數 $hook 是定時器的 hook 名稱，讓執行任務的 function 可以掛在這個定時器裡面，$args 為當 hook 發生時可以傳給任務 function 的參數。

為了易於辨識，通常定時器的名稱會以執行週期來命名。以 sp_daily_cron，可以很直覺的知道這是每天會執行一次的定時器，便於讓其他任務也能使用它。

> 要查詢已經註冊過的定時器可以使用 WP Control [https://wordpress.org/plugins/wp-crontrol/](https://wordpress.org/plugins/wp-crontrol/)，它會幫你列出所有的定時器，另外還可以用 Cron Logger [https://wordpress.org/plugins/cron-logger/](https://wordpress.org/plugins/cron-logger/) 來查看定時器的工作紀錄。

### 自訂排程間隔時間

預設情況下，wp_schedule_event() 只有提供每小時、每天與每兩天這三種，如果要自訂新的間隔時間，可以使用 cron_schedules，範例如下：
```php
<?php
//add a monthly interval to use in cron jobs
function sp_cron_schedules($schedules)
{
$schedules['monthly'] = array(
'interval' => 60*60*24*30, //really 30 days
'display' => 'Once a Month'
);
}
add_filter( 'cron_schedules', 'sp_cron_schedules' );
```
如果要取得是一週當中第幾天來執行排程，可以使用 date() 日期函式來判斷，以下範例為每週的第一天執行：
```php
<?php
//run on Mondays
function sp_monday_cron()
{
//get day of the week, 0-6, starting with Sunday
$weekday = date("w");

//is it Monday?
if($weekday == "1")
{
//execute this code on Mondays
}
}
add_action("sp_daily_cron", "sp_monday_cron");
```
上述範例為每隔一段時間執行任務，另外也可以使用 wp_schedule_single_event() 來預約在未來的某個時間點來完成任務的執行，譬如說可以設定當作者發布文章過後的一個小時自動發信給審核者，讓作者有時間可以進行潤稿的作業。
```php
<?php
 function do_this_in_an_hour() {
 
    // do something
}
add_action( 'my_new_event','do_this_in_an_hour' );
 
// put this line inside a function, 
// presumably in response to something the user does
// otherwise it will schedule a new event on every page visit
 
wp_schedule_single_event( time() + 3600, 'my_new_event' );
 
// time() + 3600 = one hour from now.
```
### 使用主機排程來執行 wp-cron

wp-cron 是當頁面有載入時才會開始檢查是否有排程任務需要執行，所以當半夜網站沒人或是頁面被快取不需重新載入的情況下，wp-cron 就很有可能不會被觸發，因此需要確實執行的排程任務最好還是要透過主機的 crontab 來處理。

要使用主機的 crontab 首先要再 wp-config.php 加入停用 wp-cron 的設定：
```php
disable( "DISABLE_WP_CRON", true );
```
接下來在主機設定 crontab 並使用 wget 來造訪 wp-cron.php 頁面：
```bash
0 0 * * * wget -O - -q -t 1 https://yoursite.com/wp-cron.php?doing_wp_cron=1
```
0 0 * * * 五個參數代表分、時、日、月、星期幾，所以這個範例表示的是 0 時 0 分，也就是午夜十二點，後面的星號則代表不指定，wget 則是讓主機前往特定網址進行下載動作，-O 是設定下載後的檔案名稱，-q 是啟用 quite 模式，這會關閉每次排程執行完成後的通知。

\-t 代表如果 wget 執行失敗的的話重新執行的次數，範例中設定 -t 1，則代表只會執行一次，萬一失敗的話就不會再重複執行。如果 wp-cron.php 無法順利造訪，就代表你的網站已經停止運作了，就不用再重複執行了。

網址最後接的參數 ?doing_wp_cron 是 wp-cron.php 執行前會先去檢查的參數，要注意的地方是記得把 wp-cron.php 排除在被快取的檔案之中，才能有效的被觸發。

> 關於 crontab 與 get 可以參考 Wang 大的兩篇好文：
> 
> [https://blog.gtwang.org/linux/linux-crontab-cron-job-tutorial-and-examples/](https://blog.gtwang.org/linux/linux-crontab-cron-job-tutorial-and-examples/)  
> [https://blog.gtwang.org/linux/linux-wget-command-download-web-pages-and-files-tutorial-examples/](https://blog.gtwang.org/linux/linux-wget-command-download-web-pages-and-files-tutorial-examples/)

### 不使用 wp-cron 只用 crontab

如果你的外掛沒有要發佈或是使用對象是熟悉 Linux 指令的工程師，那麼就不需要用到 wp-cron 而直接使用 crontab 去執行排程任務即可，只要透過帶有參數的 URL 去觸發 callback function，範例如下：
```php
<?php
//run on Mondays
function sp_monday_cron()
{
//check that cron param was passed in
if(empty($_REQUEST['sp_cron_monday']))
return false;

//execute this code on Mondays
}
add_action("init", "sp_monday_cron");
```
先把要執行的任務 hook 到 init 上面，當 URL 帶有 sp_cron_monday 這個參數的時候就會觸發，然後 crontab 設定如下：

0 0 * * 1 wget -O  - q -t 1 https://yoursite.com?sp_cron_monday=1

這邊執行設定 0 0 * * 1 最後面的 1 代表是星期幾，0 的話是星期日，1 的話是星期一，其他以此類推。

## WP_MAIL 發信

WordPress 使用 wp_mail 來取代 php 原生的 mail() 函式，用法為 wp_mail( $to, $subject, $message, $headers, $attachments )。

參數 $to 為收件人的 email，可以同時寄送給多筆信箱，中間以「,」區隔，或是使用陣列來存放多筆 email address，$subject 為信件主旨，$message 為信件內容，預設格式為純文字，如果需要在信件中寫 HTML 則需要另行設定。

$headers 為信件標頭，可以寫入 email 的相關資訊，像是郵件副本、密件副本等等，$attachments 為附加檔案，可以是單一檔案或是用陣列夾帶多筆。

相較於 PHP 的 mail()，wp_mail() 多了幾個方便的功能：

*   帶有 hook，wp_mail filter 可以傳送值給 wp_mail()，或是統一修改全站發送郵件的寄件人 email 與寄件人名稱
*   可附加一個或多個檔案，使用 mail() 會很複雜，wp_mail() 使用 PHPMailer 來簡化附加檔案的作業

### 優化電子郵件

在預設情況下，WordPress 發送的電子郵件寄件人是用管理員的信箱，以及使用 WordPress 當作寄件人名稱，我們可以透過 wp_mail_from 與 wp_mail_from_name 這兩個 filter 來進行修改。

另外也可以使用 wp_main_content_type filter 來修改郵件格式，讓內文可以支援 HTML，還可以使用 wp_mail filter 來加入郵件內容的頁首與頁尾，讓 email 整體看起來更加專業：
```php
<?php
// 修改寄件人電子郵件與名稱
function sp_wp_mail_from($from_email)
{
  return 'info@schoolpress.me';
}
function sp_wp_mail_from_name($from_name)
{
  return 'SchoolPress';
}
add_filter('wp_mail_from', 'sp_wp_mail_from');
add_filter('wp_mail_from_name', 'sp_wp_mail_from_name');

// 修改電子郵件格式
function sp_wp_mail_content_type( $content_type )
{
  if( $content_type == 'text/plain')
  {
    $content_type = 'text/html';
  }
  return $content_type;
}
add_filter('wp_mail_content_type', 'sp_wp_mail_content_type');

// 自訂頁首與頁尾
function sp_wp_mail_header_footer($email)
{
  //get header
  $headerfile = get_stylesheet_directory() . "email_header.html";
  if(file_exists($headerfile))
    $header = file_get_contents($headerfile);
  else
    $header = "";

  //get footer
  $footerfile = get_stylesheet_directory() . "email_footer.html";
  if(file_exists($footerfile))
    $footer = file_get_contents($footerfile);
  else
    $footer = "";

  //update message
  $email['message'] = $header . $email['message'] . $footer;

  return $email;
}
add_filter('wp_mail', 'sp_wp_mail_header_footer');
```
## 檔案 Header API

在 WordPress 的佈景主題或是外掛之中，都會有一段註解來說明這支程式的資訊，而用 get_plugin_data()、wp_get_theme()、get_file_data() 這三個 API 就可以取得這些資訊。

像是外掛的檔案 Header 資訊如下：
```php
/**
 * Plugin Name: Paid Memberships Pro
 * Plugin URI: https://www.paidmembershipspro.com
 * Description: The most complete member management and membership subscriptions plugin for WordPress.
 * Version: 2.3.4
 * Author: Stranger Studios
 * Author URI: https://www.strangerstudios.com
 * Text Domain: paid-memberships-pro
 * Domain Path: /languages
 */
```
要取得裡面的資訊使用 get_plugin_data( $plugin_file, $markup=true, $translate=true )，參數 $plugin_file 是要取得外掛的路徑，$markup 為 true 時，會把有連結的資料轉為 HTML 的連結，$translate 為 true 時，會依據 WordPress 語系的設定把 Header 資訊進行翻譯。

下面的範例會檢查 wp-content/plugins 裡面所有的外掛，然後把每支外掛的資訊放到一個陣列裡面：
```php
<?php
//must include this file
require_once(ABSPATH . "wp-admin/includes/plugin.php");

//remember current directory
$cwd = getcwd();

//switch to plugin directory
$plugins_dir = ABSPATH . "wp-content/plugins";
chdir($plugins_dir);

echo "<pre>";

//loop through plugin directories and print plugin info
foreach(glob("*", GLOB_ONLYDIR) as $dir)
{
$plugin = get_plugin_data($plugins_dir .
        "/" . $dir . "/" . $dir . ".php", false, false);
print_r($plugin);
}

echo "</pre>";

//switch back to current directory just in case
chdir($cwd);
```
同樣的，如果要取得佈景主題的檔案資訊，可以使用 wp_get_theme( $stylesheet, $theme_root )，參數 $stylesheet 為佈景主題名稱，留空的話預設值為目前使用的佈景主題，$theme_root 為佈景主題的資料夾路徑。

下面範例會取得佈景主題的資訊後存進一個陣列：
```php
<?php
//remember current directory
$cwd = getcwd();

//switch to themes directory
$themes_dir = dirname(get_template_directory());
chdir($themes_dir);

echo "<pre>";

//loop through theme directories and print theme info
foreach(glob("*", GLOB_ONLYDIR) as $dir)
{
$theme = wp_get_theme($dir);
print_r($theme);
}

echo "</pre>";

//switch back to current directory just in case
chdir($cwd);
```
### 使用 get_file_data() 取得檔案 Header

get_plugin_info() 與 wp_get_themes() 都是使用 get_file_data() 函式所寫成的，如果要取得檔案 Header 也可以直接使用它，當要開發擴充外掛需要取得主外掛的資訊時，get_file_data() 就非常好用。

get_file_data( $file, $default_headers, $context='' ) 參數 $file 為要取得的檔案資訊的路徑或檔名，$default_headers 使用陣列來指定要取得的資訊，$context 為當有不同 Header 時的辨識標籤，標籤名稱取決於 hook extra_{context}_headers 中間的 context：
```php
<?php
//set headers for our files
$default_headers = array(
"Title" => "Title",
"Slug" => "Slug",
"Version" => "Version"
);

//remember current directory
$cwd = getcwd();

//change to reports directory
$reports_dir = dirname(__FILE__) . "/reports";
chdir($reports_dir);

echo "<pre>";

//loop through .php files in reports directory
foreach (glob("*.php") as $filename)
{
$data = get_file_data($filename, $default_headers, "report");
print_r($data);
}

echo "</pre>";

//change back to the current directory in case someone expects the default
chdir($cwd);
```
### 幫佈景主題與外掛加入新的檔案 Header

要新增外掛或佈景主題的 Header，可以使用 extra_plugin_headers 與 extra_theme_headers 這兩個 filter，或是也能使用 extra_{context}_headers 來讓 get_file_data() 取得自訂的 Header。
```php
<?php
/*
Plugin Name: Stop Plugin Updates
Plugin URI: http://bwawwp.com/plugins/stop-plugin-updates/
Description: "Allow Updates: No" i a plugin's header keeps it from updating.
Version: .1
Author: Stranger Studios
Author URI: http://www.strangerstudios.com
*/

//add AllowUpdates header to plugin
function spu_extra_plugin_headers( $headers ) {
    $headers['AllowUpdates'] = "Allow Updates";
return $headers;
}
add_filter( "extra_plugin_headers", "spu_extra_plugin_headers" );
```
## Heartbeat API

Heartbeat API 使用 Ajax 的方式實現即時的資料庫讀寫動作，當網站頁面載入時，會以每 15~30 秒的頻率觸發一次，也可以手動設定觸發頻率為 60 秒。可以使用外掛 [Heartbeat Control](https://wordpress.org/plugins/heartbeat-control/) 來設定觸發頻率以及開啟或關閉 Heartbeat API。

Heartbeat API 通常是後台會用到的功能，雖然在前台也可以用，但要謹慎的使用，因為以這麼高的頻率去存取資料庫，對於伺服器而言勢必會造成一定程度的負擔。

它被應用的場景最常見的就是當同時有兩位管理者要編輯同一篇文章時，後編輯者會跳出詢問是否要接管的小視窗，這個判斷機制就是使用 Heartbeat API 來進行觸發，它會定時去偵測目前文章的編輯者，所以都有另外編輯者進入時，可以很快的知道這篇文章正被其他人在編輯。

另外還有像是自動儲存草稿、或是有需要即時更新資料的外掛如 Google Analytics，都是它大顯身手的地方。

Heartbeat API 會依照以下的順序執行：

1.  客戶端透過 jQuery 傳送資料給伺服器，事件名稱是 heartbeat-send
2.  伺服器接收到資料後透過 PHP 來處理回應，使用 heartbeat_rceived filter
3.  客戶端收到伺服器回應後使用 jQuery 來處理，事件名稱為 heartbeat-tick

下面作者示範如何使用 Heartbeat API，來協助老師處理家長接送小孩時車流量的控管，透過 Heartbeat API 可以即時更新資訊以掌握家長的位置，讓小孩不用在車道上等太久以減少意外事故發生的機率。
```php
<?php
/**
* Plugin Name: The Teacher Life Saver
* Plugin URI:  https://schoolpress.me/
* Description: Alert teachers of parent arrivals
* Version:     0.0.1
*/

// 在後台首頁加入家長位置訊息
function ttls_dashboard_widget() {
global $wp_meta_boxes;

// widget ID, widget name, callback function
wp_add_dashboard_widget('ttls_widget', 'Student Pick Ups', 'ttls_dashboard');
}
add_action('wp_dashboard_setup', 'ttls_dashboard_widget');

// Markup for the dashboard widget
function ttls_dashboard() {
echo "<div id='ttls_message'></div>";
}

// 修改觸發頻率
function ttls_heartbeat_settings( $settings ) {
    $settings['interval'] = 15; // Anything between 15-60 seconds
    return $settings;
}
add_filter( 'heartbeat_settings', 'ttls_heartbeat_settings', 1 );

// Enqueue heartbeat.js and our JavaScript functions
function ttls_heartbeat_init()
{
  // Only run this on the dashboard page (index.php)
  global $pagenow;
  if( $pagenow != 'index.php' )
  return;

  // Enqueue the Heartbeat API
  wp_enqueue_script('heartbeat');

  // Load our JavaScript functions in the footer
  add_action("admin_footer", "ttls_js_wp_footer");
}
add_action("admin_init", "ttls_heartbeat_init");

// JavaScript functions ran in the footer
function ttls_js_wp_footer()
{
?>
<script>
jQuery(document).ready(function() {

// 送資料給 server
jQuery(document).on('heartbeat-send', function(e, data) {
data['client'] = 'check-for-parents';
});

// 接收從 server 回傳的資料
jQuery(document).on('heartbeat-tick', function(e, data) {
if(data['server'])
document.getElementById("ttls_message").innerHTML = data['server'] 
+ document.getElementById("ttls_message").innerHTML;
});
});
</script>
<?php
}

// 處理從 hearbeat-send 接收到的資料然後傳給 hearbeat-tick
function ttls_heartbeat_received($response, $data)
{
// Look for whatever data was passed from JS heartbeat-send function
if($data['client'] == 'check-for-parents')
{
  // Build whatever response you want
  $r = '<p>';
  $r .= date( 'm/j/y g:i a', current_time( 'timestamp', 0 ) );
  $r .= " - Nina Messenlehner's father Brian Messenlehner ";
  $r .= "has arrived in a 2007 White Hummer H2.";
  $r .= '</p>';

  return $r;
}
add_filter('heartbeat_received', 'ttls_heartbeat_received', 10, 2);
```