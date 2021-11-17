---
title: 【 書摘 】Building Web Apps with WordPress - 使用 WordPress 外掛
tags:
  - $global
  - $wpdb
  - buddypress
  - custom plugin
  - gpl
  - loop
id: '1370'
categories:
  - - WordPress
date: 2020-05-08 09:51:27
etoc: true
---

## 三、Using WordPress Plugin / 使用 WordPress 外掛

WordPress 外掛超強大！它可以讓你在不用懂任何程式碼的狀況下就可以使用許多功能，不管你使用的是免費、付費或是自行開發的外掛，透過外掛可以大幅度的延伸 WordPress 的既有功能。

再加上 WordPress 是開源軟體，許多開發者設計各式各樣的外掛來解決他們自己或是別人的問題，根據 WordPress Codex 的定義：「外掛是一套程式，或是使用 PHP 寫成的多組 Function，用來幫 WordPress 增加特定的功能或服務，它透過 WordPress 的 API，無縫整合到所有的 WordPress 網站中。」
<!--more-->
外掛可以把 WordPress 轉變成任何你想得到的網站，像是購物車、社群平台甚至是手機 App 。當 WordPress 完成安裝後會有兩個內建的外掛：Hello Dolly 與 Akismet。

Hello Dolly 會在後台的頂端工具列加入隨機的歌詞，它沒啥實際作用，但很好展現了如何組織一個外掛的架構。而 Akismet 主要是整合 Akismet.com 這個服務，它可以自動防止垃圾留言。

目前在 WordPress plugin repository 上面有超過五萬五千個外掛，但實際數字絕對不只這些，因為還有很多開發者寫好了外掛但沒有上架到這邊，他們多半會放在自己的個人或是商業網站上，其中也不少需要付費的。

> 上 Github 去搜尋 WordPress Plugin 可以找到超過九萬筆的結果，裡面包含了許多沒有上架到官方的外掛。

大部分的付費外掛都有提供開發者授權，只要付一次費用就可以裝在多個網站上面。需要注意的是，當在 Google 上面搜尋時，要確保是連結到外掛開發者的作者頁面或是官方網站，因為現在有非常多所謂的 GPL Club 提供付費外掛的破解版，而且價格通常比原價低很多甚至是免費。

大部分這類網站所提供的東西都有毒，裡面可能會埋後門程式或是對你網站有害的程式碼，絕對不要貪小便宜而去使用，因為未來造成的損失絕對遠遠超過因為買盜版而省下的那一點錢。

事實上正版外掛的售價已經是非常實惠的，再加上使用時有任何問題或是功能建議，大部分的開發團隊都非常樂於為用戶服務，更不用說程式碼漏洞的修補以及後續版本的即時更新，如果是有心要經營網站的朋友，千千萬萬不要去使用 GPL Club，即使他們說自己提供的軟體絕對乾淨沒問題...

> 曾經遇過一位朋友說他只是要做自己的個人部落格，流量小小的，不會有什麼人看，也沒有要商業用途或是拿來賺錢，所以他就用了從 GPL Club 載來的佈景主題來架站。
> 
> 如果去搶銀行只搶了 100 塊也算犯法嗎？廢話，重點不是你的站大站小，而是只要使用盜版軟體就必須要準備好承受未來會付出的代價，而且免費版型已經這麼多了，既然會在意自己的網站要用更進階的功能，代表他對這個站還是有一定的期待，
> 
> 說什麼只是做個小網站所以用 GPL Club 應該沒關係，這都是為了說服自己使用盜版軟體的正當性而已，別自欺欺人了，不想花錢請用免費的，或是自己花時間去學習怎麼寫程式，沒有任何的理由可以去使用 GPL Club，
> 
> 更不用說有在接案的朋友，對，就是在說你！如果一個案子的總價連一支幾千塊的付費外掛都買不起來，那還是別出來接案了，提供客戶安全無虞的網站是接案者最基本的責任，連使用正版軟體這點都做不到的話請不要跟人家說你在接案，如果正在看這篇文章的你是剛入行的新朋友，請你務必要牢牢記住這一點：
> 
> WordPress 社群之所以可以這麼龐大是因為太多開發者願意無償熱心投入，也因為有了這些社群使用者，開發者可以提供功能更多、更強的進階付費外掛，如果這些開發者無法從他們投入的產品中賺取應有的收入，結果就是開發者減少，開發者一減少我們就用不到這麼多好用的工具，那麼 WordPress 這個軟體就真的只能拿來寫部落格了...
> 
> 所以如果你覺得 WordPress 很好用、外掛很多、社群很熱心而且又完全免費，那麼就請你抱持著感恩的心來回報它，你可能沒辦法開發外掛，或是寫教學文來回饋社群，至少，用實際的購買行為來支持開發者，這樣就非常足夠了。

## **GPL V2 開源授權規範**

無論取得外掛的方式是什麼，所有的 WordPress 外掛都是採用 General Public License 版本 2 的授權協議，這表示當原始碼在需要被散佈的情況下（像是上架官方目錄、提供免費或付費下載），散播出去的原始碼也是必須遵循這樣的授權讓任何人都可以自由的修改、散佈。

有些開發者會把前端的靜態資源檔案像是 HTML、CSS、JavaScript 採取不同的授權方式，以便與 PHP 的 GPL 來進行區分，而有些外掛會極力否認甚至不採用 GPL 授權。

但 WordPress 的精神領袖 Matt Mullenweg 表示所有的佈景主題與外掛都一定、必須要採用 GPL 授權，所以如果你今天要開發的外掛是會在社群進行發佈，建議還是採取 GPL。

> 這段作者講的很保守，因為這是很有爭議的地方，就我從開發者的角度來看，WordPress 主要的服務對象是使用者而非軟體開發業者，儘可能的讓使用者自由使用 WordPress 的所有工具是主要任務。
> 
> WordPress 的商業模式是靠服務客戶而非販售程式碼，所以同樣的概念運用在社群上，協助客戶架設 WordPress 網站、提供長期的售後服務才是維繫客戶關係的關鍵，與傳統販售一次性軟體商品的模式相比，這樣的合作方式讓雙方都能持續互惠，也是讓社群能茁壯的原因之一。
> 
> 有了這樣的背景知識後，回到 GPL Club，對！你想得沒錯，它是完全符合 GPL 的授權規範的，這些業者他們聲稱收取的費用是維護費，就是當軟體版本有更新時他們會重新上架商品，但他們不會維護程式碼，使用上遇到問題他們也不會理你，所以才可以賣得這麼便宜。
> 
> 但畢竟不是原始的開發商，在他們取得這些外掛的過程中會不會加料也很難說，免費的最貴這句話不是沒有道理的，羊毛出在羊身上，他們不要你的錢那要的會是什麼呢？未來當看到免費或低於一般市價的商品時先想想這個問題，就不會被免費二字給沖昏頭了。
> 
> 最後還是還給 GPL Club 一個清白，它們的軟體不能算是「盜版」，因為並沒有違反 WordPress 的授權條款，比較有問題的是 Themeforest 的商品，因為他們有他們自己的授權方式，這部分就是各說各話了...
> 
> 總之，身為使用者，去作者網站進行購買下載就 100% 沒問題了，除了最有保障之外，這也是對他們最大的鼓勵！

## **開發自己的外掛**

雖然已經有成千上萬個寫好的外掛了，但常常還是覺得那邊功能差一點、這邊要是再有什麼就好了，如果你有這樣的感覺，就來寫自己的外掛吧！

寫一支基本的外掛十分容易，只要在 wp-content/plugins 的目錄下，新增一個 .php 檔，檔名可以是任何名字，這邊示範新增一隻 my-plugin.php，打開編輯器，在 my-plugin.php 裡面加入以下註解：
```php
<?php
/**
 * Plugin Name: My Plugin
 * Plugin URI: https://bwawwp.com/my-plugin/
 * Description: This is my plugin description.
 * Author: messenlehner, strangerstudios
 * Version: 1.0.0
 * Author URI: https://bwawwp.com
 * License: GPL-2.0+
 * License URI: http://www.gnu.org/licenses/gpl-2.0.txt
 */
?>
```
輸入完後存檔，第一支外掛就完成了！你可以在後台的 外掛 > 已安裝的外掛看到並啟用它，雖然現在它還沒有任何的功能，以下範例使用之前介紹過的 Hook，在前台的頁尾加入一些文字：
```php
<?php
function my_plugin_wp_footer() {
    echo 'I read Building Web Apps with WordPress and now I am a WordPress Genius!';
}
add_action( 'wp_footer', 'my_plugin_wp_footer' );
?>
```
加入完成後存檔，應該就可以看到在網頁的最下方出現這段字。如果你很熟悉 PHP，就可以開始寫東西了，不熟也沒關係，找些功能單純的外掛像是 Hello Dolly，看它是怎麼運作的。

## **外掛的資料夾結構**

無論是什麼程式，有好的檔案結構才有辦法維護，如果外掛很單純，一支主要的 php 檔就可以解決是最好的，但通常為了要完成某些功能，程式碼會越來越多，像是控制前/後臺的 CSS、JS、圖片等等，這邊使用本書範例外掛 School Press 來說明資料夾結構：

**plugins/schoolpress/adminpages**/

如果外掛有提供後台設定頁面的話，把頁面的 php 檔放在這個資料夾中，以下範例為新增後台設定頁面：
```php
// add a SchoolPress menu with reports page
function sp_admin_menu() {
    add_menu_page(
'SchoolPress',
'SchoolPress',
'manage_options',
'sp_reports',
'sp_reports_page'
);
}
add_action( 'admin_menu', 'sp_admin_menu' );

// function to load admin page
function sp_reports_page() {
  require_once dirname( __FILE__ ) . "/adminpages/reports.php";  // 設定頁模板
}
```
**plugins/schoolpress/classes/**

該資料夾放 php 的類別檔，習慣用法是一個類別一個獨立的檔案，檔案名稱命名方式為 class.className.php，className 替換為你的類別名稱。

**plugins/schoolpress/css/**

該資料夾放 CSS 檔案，可以把它拆分為控制前台 style 的 frontend.css 與後台 admin.css，也可以放第三方的樣式檔，以下範例說明如何掛載 CSS 檔到前台與後台：
```php
function sp_load_admin_styles() {
wp_enqueue_style(
'schoolpress-plugin-admin',
plugins_url( 'css/admin.css', __FILE__ ),
array(),
SCHOOLPRESS_VERSION,
'screen'
);
}
add_action( 'admin_enqueue_scripts', 'sp_load_admin_styles' );

function sp_load_frontend_styles() {
    wp_enqueue_style(
        'schoolpress-plugin-frontend',
        plugins_url( 'css/frontend.css', __FILE__ ),
        array(),
        SCHOOLPRESS_VERSION,
        'screen'
    );
}
add_action( 'wp_enqueue_scripts', 'sp_load_frontend_styles' );
```
要掛在前台使用 wp_enqueue_scripts 這個 Hook，而後台是 admin_enqueue_scripts。在寫前台的 CSS 時，必須要考慮到這些樣式是否該放在佈景主題的 style.css 而非外掛之中，因為前台的樣式通常是由佈景主題所控制的。

放在外掛的 CSS 代表就算切換佈景主題也不會變動，想像當你的佈景主題沒有任何樣式的時候畫面至少該呈現什麼樣子，這些樣式的 CSS 就適合放在外掛之中，並預期佈景主題有了樣式之後會覆蓋過去。

舉個例子：外掛的 frontend.css 不應該帶有控制顏色的屬性，而是控制大頭照的寬度與浮動比較適合放在這邊。

**plugins/schoolpress/js/**

該資料夾放所有的 JavaScript 檔案，與 CSS 一樣可以切分為前台用的 frontend.js 與後台 admin.js。也可以把第三方的 JS 檔用子資料夾放在這邊。以下範例說明如何掛載 JS 檔：
```php
function sp_load_admin_scripts() {
wp_enqueue_script(
'schoolpress-plugin-admin',
plugins_url( 'js/admin.js', __FILE__ ),
array( 'jquery' ),
SCHOOLPRESS_VERSION
);
}
add_action( 'admin_enqueue_scripts', 'sp_load_admin_scripts' );

function sp_load_frontend_scripts() {
wp_enqueue_script(
'schoolpress-plugin-frontend',
plugins_url( 'js/frontend.js', __FILE__ ),
array( 'jquery' ),
SCHOOLPRESS_VERSION
);
}
add_action( 'wp_enqueue_scripts', 'sp_load_frontend_scripts' );
```
與 CSS 一樣使用 wp_enqueue_scripts 與 admin_enqueue_scripts 這兩支 Hook，同時也要考慮哪些前台的 JS 應該要放在佈景主題而非外掛。像是控制 slideshow 的 JS 要放在佈景主題，而呼叫 Ajax 的 JS 就可以放在外掛。

因為掛載 CSS 與 JS 都是用同一個 Hook，所以可以把它們寫在同一支 function 裡面，要注意沒有 wp_enqueue_styles 與 admin_enqueue_styles 這樣的 Hook。

**plugins/schoolpress/images/**

該資料夾放外掛需要使用到的圖片。

**plugins/schoolpress/includes/**

該資料夾放支援外掛的 php 檔，通常是放非程式核心邏輯相關的輔主 function。因為在外掛資料夾的根目錄只能放一支主要的 php 檔，所以其他的檔案就用這個資料夾來進行組織管理，通常會有 functions.php、common.php 或是 helpers.php。

儘可能把每一個檔案維持在小規模並用引入的方式來載入，像是產生隨機文字、整理字串或是其他 WordPress 沒有內建的第三方框架都可以放在這邊。

**plugins/schoolpress/includes/lib**

該資料夾放第三方的函式庫。

**plugins/schoolpress/pages/**

該資料夾放顯示在前端頁面的 php 檔，通常會使用 Shortcode 來顯示外掛所產生的內容，以下範例說明如何建立一組 Shortcode：
```php
// preheader
function sp_stub_preheader() {
if ( !is_admin() ) {
global $post, $current_user;
if ( !empty( $post->post_content ) && strpos
   ( $post->post_content, '[sp_stub]' ) !== false ) {
/*
Put your preheader code here.
*/
}
}
}
add_action( 'wp', 'sp_stub_preheader', 1 );

// shortcode [sp_stub]
function sp_stub_shortcode() {
ob_start();
?>
Place your HTML/etc code here.
<?php
$temp_content = ob_get_contents();
ob_end_clean();
return $temp_content;
}
add_shortcode( 'sp_stub', 'sp_stub_shortcode' );
```
這邊的 preheader 指的是放入比 wp_head() 還要更早執行的程式碼，!is_admin 判斷是在前台，if ( !empty( $post->post_content ) && strpos( $post->post_content, '[sp_stub]' ) !== false ) 是判斷該頁面是否帶有 [sp_stub] 這個 Shortcode，有的話條件就成立。

Hook wp 指的是會在 WordPress 建立完 $post 全域變數後、並在尚未渲染任何標頭與 HTML 內容之前觸發。preheader 這隻 function 可以用來檢查權限、表單提交的狀態或是任何在頁面還沒有被載入之前需要執行的功能。

在 MVC 的架構下，preheader 放的是 model 或是 controller 的程式碼，因為頁面內容還沒有產生，你可以安全地把使用者導引至其他頁面，像是可以使用 wp_redirect() 來跳轉到註冊或是登入頁，避免讓他們造訪到會員限定的頁面。

Shortcode 的部分使用 PHP 的 function：ob_start()、ob_get_contents()、ob_end_clean() 來把程式碼輸入到緩衝區而非瀏覽器之中。在 ?> 與 <?php 之間的字串 Place your HTML/etc code here. 會被放在 $temp_content 這個變數裡面，這樣做的用意是讓程式碼比較好閱讀，還可以混搭 HTML 與 PHP。

> 這個範例有點難懂，簡單說就是當頁面帶有 [sp_stub] 的時候，可以做些判斷上的應用，像是使用者是否登入的判斷 is_user_logged_in 可以寫在 preheader 裡面，然後 sp_stub_shortcode 可以把「您尚未登入」 的提示存到緩衝區之中，這樣既可以顯示提示給使用者，又不會讓頁面開始渲染 HTML，因為 PHP 的頁面跳轉 [header('Location:url')](https://www.php.net/manual/en/function.header.php) 必須在頁面還沒有任何內容的時候觸發。

**plugins/schoolpress/services/**

該資料夾放 Ajax 相關的 php 檔。

**plugins/schoolpress/scheduled/**

該資料夾放與排程相關的 php 檔。

**plugins/schoolpress/schoolpress.php**

外掛最主要的 php 檔，在比較大型的外掛中這支檔案主要負責引入其他的 php、常數的宣告、以及外掛的檔案說明註解。

## **開發現有外掛的附加套件**

除了可以開發自己的外掛，還可以幫其他人寫的外掛再開發附加套件 ( Add-Ons )。如果你發現了一支外掛可以滿足你 95% 的需求，剩下的 5% 不用把整個外掛載下來改寫，只要設計專屬於這支外掛的附加套件即可。

大部分組織良好的外掛都有完善的 Hook 可供使用，所以只要利用這些 Hook 就可以再把程式碼掛到外掛裡面去，就像是寫外掛的時候把程式掛進 WordPress 內建的 Hook 一樣。

如果遇到需要的 Hook 但該外掛並未提供，整包載回來自己動手修改也行，但比較好的做法是開 issue 給作者，請他們把你需要的 Hook 加入，讓其他開發者也可以受惠。

## **SchoolPress 使用場景**

所以應該開發免費的還是進階付費的外掛？以下透過 SchoolPress 這個有社群功能的外掛來舉例：每位老師是社團的管理員，並且可以把學生帳號加入社團。學生可以瀏覽社團動態或是班級的動態牆。

使用 [BuddyPress](https://buddypress.org) 學生可以與其他學生建立朋友關係，追蹤他們的朋友或是老師，並且還可以發私訊與老師聯繫。

使用 [BadgeOS](https://badgeos.org) 與 [BadgeOS Community](https://wordpress.org/plugins/badgeos-community-add-on/) 套件，老師可以創造有趣的獎勵徽章給他的學生，學生只要完成作業或報告就可以獲得徽章，並且分享在他們的動態牆上。

然後使用 [Gravity Forms](https://www.gravityforms.com) 來設計表單讓學生提交他們的作業。

## **WordPress Loop**

Loop 是除了 Hook 以外另一個在 WordPress 裡面非常重要的機制，它的主要功能是根據模板檔案來顯示文章的內容，大部分的 Theme 會在以下檔案包含 Loop 的運用：index.php、archive.php、category.php、tag.php、single.php、page.php。

打開這些檔案會看到如下的程式碼：
```php
<?php
if ( have_posts() ) {
while ( have_posts() ) {
the_post();
the_title( '<h2>', '</h2>' );
the_content();
}
} else {
// show a message like sorry no posts!
}
```
have_posts() 檢查是否有文章可以被顯示，如果有的話就使用 while 迴圈把文章撈出來，先呼叫 [the_post()](https://developer.wordpress.org/reference/functions/the_post/) 來設置文章相關的全域變數，讓文章相關的資料可以被正確的取得。

為了方便寫 HTML 以及閱讀性，可以使用一行的方式來寫 loop：
```php
<?php if(have_posts()): ?>
<?php while(have_posts()): the_post(); ?>
<div>在 content 之前的 HTML</div>
<?php the_content(); ?>
<div>在 content 之後的 HTML </div>
<?php endwhile; ?>
<?php else: ?>
<p>沒有可以顯示的文章</p>
<?php endif; ?>
```
> 實務上我自己是比較習慣用一行的寫法，當要處理比較多的 HTML 的時候會比較好寫，閱讀上也比較直覺，注意 if 跟 while 結尾是用冒號、else 也是用冒號，endwhile 跟 endif 是用分號結尾。

## **WordPress 全域變數**

全域變數是可以自訂定義並用在所有地方的變數，WordPress 有內建了一些全域變數，讓你可以立即使用不用再自己花時間額外寫。內建的所有全域變數可以用 print_r 把它們印出來，或是參考 [https://codex.wordpress.org/Global_Variables](https://codex.wordpress.org/Global_Variables)
```php
echo '<pre>';
print_r( $GLOBALS );
echo '</pre>';
```
如果要在程式內使用全域變數的話，可以這樣寫：
```php
<?php
global $global_variable_name;
?>
```
有些全域變數只能在特定的場景下才有值可以取得，下面幾個是很常用到的全域變數：

**$post** - 在 loop 中取得資料表 wp_posts 內所有關於 post 的欄位。

**$authordata** - 在 loop 中取得文章作者的相關欄位。

## **$wpdb**

$wpdb 是在 WordPress 中直接用來操作資料庫的類別，可以用它來進行資料庫的 select、update、insert 以及 delete。如果你不熟悉 WordPress 的 function，那麼 $wpdb 是你的好朋友。

如果需要一些很複雜的資料庫查詢條件，而且是內建 function 找不到的，$wpdb 就可以派上用場了。但請別認為 WordPress 內建 function 的查詢速度很慢，除非你很熟悉資料庫，不然使用內建 function 來取得文章、使用者及其他資料是比較好的作法。

因為 WordPress 核心維護團隊都已經把這些 function 優化過了，並且增加了快取機制來加速資料查詢，也可以在所有的外掛之中保持良好的運作，所以除非是在某些特殊狀況下，再來用 $wpdb 來做資料操作。

## **自訂資料表**

在 SchoolPress 這支外掛中，作者新開了一個資料表來紀錄學生作業與學生對應的關係，這可以讓資料庫比較清楚些，並且可以直接查詢每個學生的所有作業。

通常新增資料表要使用 CREATE TABLE 語句，然後使用 WordPress 內建的 dbDelta() 來執行新增資料表，在新增資料表前有些地方要注意：

在 option 中新增一個變數 $db_version，這是為了讓外掛知道目前使用的自訂資料表是第幾版，如果資料表欄位有變更，就可以使用 db_version 來做紀錄，也可以用來判斷當外掛第一次啟用時，不存在這個版本號才新增資料表。

另一個常見作法是把自訂資料表的名稱作為 $wpdb 的一個屬性，可以讓之後查詢時比較省事，以下範例為 SchoolPress 的資料庫設定：
```php
<?php
// set up the database for the SchoolPress app
function sp_setupDB() {
global $wpdb;

// shortcuts for SchoolPress DB tables
$wpdb->schoolpress_assignment_submissions = $wpdb->prefix .
'schoolpress_assignment_submissions';

$db_version = get_option( 'sp_db_version', 0 );

// create tables on new installs
if ( empty( $db_version ) ) {
global $wpdb;

$sqlQuery = "
CREATE TABLE '" . $wpdb->schoolpress_assignment_submissions . "' (
  \`assignment_id\` bigint(11) unsigned NOT NULL,
  \`submission_id\` bigint(11) unsigned NOT NULL,
UNIQUE KEY \`assignment_submission\` (\`assignment_id\`,\`submission_id\`),
UNIQUE KEY \`submission_assignment\` (\`submission_id\`,\`assignment_id\`)
)
";

require_once ABSPATH . 'wp-admin/includes/upgrade.php';
dbDelta( $sqlQuery );

$db_version = '1.0';
update_option( 'sp_db_version', '1.0' );
}
}
add_action( 'init', 'sp_dbSetup', 0 );
?>
```
sp_dbSetup() 要在最早的時候執行，所以 hook add_acton 的第三個參數設為 0 ( 數字越小越先執行 )。然後因為資料表都會帶有前綴，預設雖然是 wp_，但因為外掛是要給其他人使用的，前綴通常都不會是預設值，所以使用 $wpdb->prefix 來取得資料表前綴。

然後把資料表前綴加上資料表名稱存進 $wpdb 裡面，這樣在下面要 CREATE TABLE 帶入資料表名稱時就比較方便且容易理解。

$db_version 會在為空值時才會給值並且存在 option 裡面，這樣在外掛第一次啟用時才會新增資料表。然後CREATE TABLE 語句指定要開的欄位，在寫 SQL 語句時，記得要先在 MySQL 裡面測過沒問題後才丟到外掛裡面去執行。

最後使用 dbDelta 來執行資料表新增的動作，dbDelta 會判斷沒有該表才會新增，或是當有同名稱的表存在時，會幫你 ALTER TABLE 來確保已存在的表與新表有相同的 Schema。

另外要注意的是，要使用 dbDelta 時必須先引入 wp-admin/includes/upgrade.php，然後才把 SQL 語句作為參數傳給 dbDelta。

你的 SQL 語句與一般的有點不太相同，需要注意下面幾個地方

1.  每一個新增的欄位要寫在獨立的一行
2.  在 PRIMARY KEY 與定義主鍵的欄位之間要有空格
3.  使用關鍵字 KEY 而不用 INDEX，並且至少包含一個 KEY
4.  欄位名稱不要使用「'」與「\`」這兩個符號

參考 [https://codex.wordpress.org/Creating_Tables_with_Plugins](https://codex.wordpress.org/Creating_Tables_with_Plugins)

使用 dbDelta() 來新建資料表是比較好的選擇，因為它會幫你自動更新較舊的資料庫版本，其他的用法還可以使用 $wpdb->query($sqlQuery) 來直接新增。

$wpdb->query() 可以直接執行任何經過驗證的 SQL 語法，其中的 query() 帶有很多實用的屬性協助開發者除錯以及追蹤查詢結果：

*   $wpdb->result - 顯示查詢結果
*   $wpdb->num_queries - 每次查詢會自動增加
*   $wpdb->last_query - 顯示最後一筆的查詢
*   $wpdb->last_error - 顯示最後一筆的錯誤訊息
*   $wpdb->insert_id - 顯示最後一筆成功 INSERT 的資料 ID
*   $wpdb->rows_affected - 顯示影響的資料筆數
*   $wpdb->num_rows - 顯示查詢結果比數
*   $wpdb->last_result - 顯示透過 mysql_fetch_object() 函式產出的物件

$wpdb->query() 返回的值是根據查詢條件是否成功，返回 false 代表查詢失敗，你可以使用下面的程式碼來驗證查詢狀態：
```php
<?php
if($wpdb->query($query) === false){ wp_die("it failed"); }

返回的結果是從 CREATE、ALTER、TRUNCATE 以及 DROP 這些查詢所產出。影響的資料筆數是包含 SELECT、INSERT、UPDATE、DELETE 以及 REPLACE 這些查詢。

## **資料庫查詢的跳脫字元**

要注意的地方是 $wpdb->query() 並不會自動處理跳脫字元的問題，在 WordPress 中有兩個方法來處理，分別是 esc_sql() 與 $wpdb->prepare()，下面是 esc_sql() 的使用範例：

<?php
global $wpdb;
$user_query = $_REQUEST['uq'];

$sqlQuery = "SELECT user_login FROM $wpdb->users WHERE
user_login LIKE '%" . esc_sql($user_query) . "%' OR
user_email LIKE '%" . esc_sql($user_query) . "%' OR
display_name LIKE '%" . esc_sql($user_query) . "%'
";
$user_logins = $wpdb->get_col($sqlQuery);

if(!empty($user_logins))
{
echo "<ul>";
foreach($user_logins as $user_login)
{
echo "<li>$user_login</li>";
}
echo "</ul>";
}
```
至於 $wpdb->prepare() 用法類似是 sprint() 與 printf()，相較於 esc_sql()，它除了會處理跳脫字元外，還可以避免語句中單引號的問題。prepare 方法定義在 wp-includes/wp-db.php 中，可以接受兩個以上的參數。

**$query** - SQL 陳述語句的字串

**$args** - 一個或多個的變數來替代 SQL 語句中的 placeholder 所代表的值

下面列出的 Placeholder 可以用在 SQL 的陳述語句中：

*   %d - integer 整數
*   %f - float 浮點數
*   %s - 字串
*   %% - 顯示百分比符號用

$d、$f、$s 在語句中不加引號，然後要使用 %% 把 placeholder 包住，這樣才能找到精準比對的資料，每個 placeholder 都會對應到一個變數，使用範例如下：
```php
<?php
$sqlQuery = $wpdb->prepare("SELECT user_login FROM $wpdb->users WHERE
user_login LIKE '%%%s%%' OR
user_email LIKE '%%%s%%' OR
display_name LIKE '%%%s%%'", $user_query, $user_query, $user_query);
$user_logins = $wpdb->get_col($sqlQuery);
```
需要注意的地方是如果使用 $wpdb->prepare() 時沒有附上第二個參數會噴錯，因此如果不需要用動態資料來做 SQL 操作的話就不用使用 $wpdb->prepare()。

## **使用 $wpdb 做 SELECT 查詢**

$wpdb 類別有很多方法可以來做資料庫查詢，針對 row 的資料查詢可以使用 $wpdb->get_results( $query, $output_type ) ，它會回傳查詢結果的陣列，在預設的情況下，回傳的是索引式陣列，下面列出回傳的資料型別：

**OBJECT** - 回傳索引式陣列的物件

**OBJECT_K** - 回傳關聯式陣列的物件，並會使用第一欄的值作為陣列的 Key，重複的值會被捨棄

**ARRAY_A** - 回傳帶有索引的關聯式陣列，使用欄位名稱作為 Key

**ARRAY_N** - 回傳帶有索引的索引式陣列

使用範例如下：
```php
<?php
global $wpdb;
$sqlQuery = "SELECT * FROM $wpdb->posts
WHERE post_type = 'assignment'
AND post_status = 'publish' LIMIT 10";
$assignments = $wpdb->get_results( $sqlQuery );

// rows are stored in an array, use foreach to loop through them
foreach ( $assignments as $assignment ) {
// each item is an object with property names equal to the SQL column names?>
<h3><?php echo $assignment->post_title;?></h3>
<?php echo apply_filters( "the_content", $assignment->post_content );?>
<?php
}
?>
```
針對 col 的查詢使用 $wpdb->get_col( $query, $column_offset = 0 )，它會返回第一欄的查詢結果。$column_offset 可以抓取其他欄位的資料來做顯示，第一欄是 0，第二欄是 1，其他以此類推。$wpdb->get_col() 方法常常被用來抓取文章的 ID，範例如下：
```php
<?php
global $wpdb;
$sqlQuery = "SELECT ID FROM $wpdb->posts
WHERE post_type = 'assignment'
AND post_status = 'publish'
LIMIT 10";
// getting IDs
$assignment_ids = $wpdb->get_col( $sqlQuery );

// result is an array, loop through them
foreach ( $assignment_ids as $assignment_id ) {
// we have the id, we can use get_post to get more data
$assignment = get_post( $assignment_id );
?>
<h3><?php echo $assignment->post_title;?></h3>
<?php echo apply_filters( "the_content", $assignment->post_content );?>
<?php
}
?>
```
如果要針對其中一列作查詢可以使用 $wpdb->get_row( $query, $output_type, $row_offset )，$row_offset 可以指定抓取搜尋結果的其他資料，0 是第一筆，1 是第二筆，其他以此類推。

## **Insert、Replace 與 Update**

資料寫入的部分可以使用 $wpdb->insert( $table, $data, $format )，相較於自己寫 INSERT 語句，只要參數傳資料表名稱與要寫入的 row data，剩下的 $wpdb 就會自動幫你搞定。

需要注意的地方是 $data 這個參數，必須與資料表的欄位名稱符合，$data 陣列中的值則代表要寫入的資料，範例如下：
```php
<?php
// 讓學生提交的資料與作業做關連
global $wpdb, $current_user;

// create submission
$assignment_id = intval( $_REQUEST['assignment_id'] );
$submission_id = wp_insert_post(
array(
'post_type'    => 'submission',
'post_author'  => $current_user->ID,
'post_title'   => sanitize_title( $_REQUEST['title'] ),
'post_content' => sanitize_text_field( $_POST['submission'] )
)
);

// connect the submission to the assignment
$wpdb->insert(
  $wpdb->schoolpress_assignment_submissions,
  array( "assignment_id"=>$assignment_id, "submission_id"=>$submission_id ),
  array( '%d', '%d' )
);

/*
This insert call will generate a SQL query like:
INSERT INTO
'wp_schoolpress_assignment_submissions'
('assignment_id','submission_id' VALUES (101,10)
*/
?>
```
在範例中，先使用 wp_insert_post() 讓學生提交的資料先寫入文章類型為 submission 的 post，然後使用 $wpdb->insert 在 schoolpress_assignment_submissions 資料表新增一筆關聯的資料。

需要注意的是第三個參數 array( '%d', '%d' )，這邊指定寫入資料的資料類型，$d 是整數，$s 還是字串，$f 是浮點數。如果沒有指定資料類型的話，一律會被當作是字串寫入。

在大部分的情況下，要做文章類型的關聯可以直接使用 wp_posts 資料表裡面的 post_parent 欄位，但如果是要做多對多的關聯，像是一個 submission 可以對應到多個 assignment，這時候另外開一張表來紀錄會是比較清楚的作法。

> 之前做過一個站有五種 Post Type，而這五種彼此都要互相關聯，而且是多對多的關係，我是用 ACF Pro 的 Relationship 欄位來處理，所以要從 A 文章類型去撈到關聯 B 文章類型、再從 B 去撈關聯 C 文章的欄位時，使用 ACF 的 get_field() 要繞好大一圈，而且會產生一堆為了拿 Post ID 的多重迴圈，這讓程式的維護變得異常複雜。
> 
> 如果依照作者的作法只要把互為關聯的 Post ID 獨立出來一張表來做存放，然後用 $wpdb 去拿關聯的文章 ID，也許事情會單純的多，但就是一開始資料表的欄位就要先規劃好。

$wpdb->replace( $table, $data, $format ) 用法與 insert() 很類似，如果需要取代既有資料只要在 $data 陣列中指定相同的 key 既可進行取代。

$wpdb->update( $table, $data, $where, $format, $where_format ) 則是用來修改整列的資料，跟自己寫 UPDATE 語句相比，用它比較容易且安全，使用範例如下：
```php
<?php
global $wpdb;
// just update the status
$wpdb->update(
'ecommerce_orders',   //table name
array( 'status' => 'paid' ),  //data fields
array( 'id' => $order_id )  //where fields
);

// update more data about the order
$wpdb->update(
'ecommerce_orders',   //table name
array( 'status' => 'pending',  //data fields
'subtotal' => '100.00',
'tax' => '6.00',
'total' => '106.00'
),
array( 'id' => $order_id )  //where fields
);
?>
```
使用 $where 來指定要修改的對象，然後用 $data 來傳入新的資料來修改。

## **$wp_query**

$wp_query 是專門用來顯示文章內容的 class，下一章會有完整的介紹。

## **$current_user**

$current_user 是專們用來顯示目前已登入使用者資訊的物件，它不僅會回傳 wp_users 裡面的欄位，還可以得到已登入使用的角色與權限，範例如下：
```php
<?php
//welcome the logged-in user
global $current_user;
if ( !empty( $current_user->ID ) ) {
echo 'Howdy, ' . $current_user->display_name;
}
?>
```
以上是 WordPress 常用到的 Global 變數，你也可以把自己常用到的全域變數存在 Global 裡面，這樣之後要重複取用就很方便。

## 實用免費外掛介紹

很多免費的外掛功能都很強大，如果想開發某些功能，可以先找找有沒有類似的外掛，萬一有的話當然最好，或是有但可能沒有 100% 符合你的需求也沒關係，直接改寫它們在某些情況下可以省下很多時間。

**Admin Columns**  
[https://wordpress.org/plugins/codepress-admin-columns/](https://wordpress.org/plugins/codepress-admin-columns/)

專門做後台文章列表欄位管理的外掛，可以很方便的把各個文章欄位放到列表中，並且進行排序與位置編排，付費版還能直接整合第三方外掛的欄位，並支援進階搜尋、報表匯出，如果客戶對於後台的資料顯示有特殊需求，Admin Columns 可以幫上你很多忙！

> 以前我很習慣用它，尤其是它的進階搜尋功能可以滿足客戶對於資料檢視的需求，但缺點就是報表 csv 匯出中文會亂碼，有回報給原廠但還未獲得解決，所以匯出功能還是要自己另外寫。
> 
> 但如果只是要單純欄位，特別是下面會介紹到的 ACF 欄位，必須要升級為付費版才能使用，再加上它的方案有限制網站數量，所以在客戶不需要進階搜尋功能的情況下，自己手刻會比較划算XD
> 
> 新增欄位不難寫，主要會用到兩支 Hook：manage_{post type}_columns 與 manage__{post type}_posts_custom_column。第一個是設定欄位名稱，第二個是抓取欄位值，具體範例參考：[https://ods.oberonlai.blog/category/wordpress-admin/edit-php/](https://ods.oberonlai.blog/category/wordpress-admin/edit-php/)

**Advanced Custom Fields**  
[https://wordpress.org/plugins/advanced-custom-fields/](https://wordpress.org/plugins/advanced-custom-fields/)

開發 WordPress 不能沒有的一支外掛，它可以用視覺化的介面幫各種文章類型增加多種不同的欄位。新增的介面也可以讓後台使用者直接在編輯畫面進行資料的修改，省去超多額外開發後台操作介面的時間。付費版提供更多欄位選擇，甚至還可以作為簡易的頁面編輯器來使用！

**BadgeOS**  
[https://wordpress.org/plugins/badgeos/](https://wordpress.org/plugins/badgeos/)

BadgeOS 讓你的網站可以提供不同的成就徽章給站內會員，它的主要功能是可以設計徽章，並且設定觸發獲得條件，另一支遊戲化成就機制的外掛叫做 GamiPress，就是從 BadgeOS 衍伸來的，作者認為 [GamiPress](https://wordpress.org/plugins/gamipress/) 維護的比較好並且擁有更多功能。

**Posts 2 Posts**  
[https://wordpress.org/plugins/posts-to-posts/](https://wordpress.org/plugins/posts-to-posts/)

Posts 2 Posts 可以讓站內所有文章擁有多對多的關聯，除了有方便的設定介面外，最重要的是它另外開了一張資料表來紀錄所有的對應關係，這在大型的網站中是非常有幫助的。

> 可惜這支外掛已經四年沒更新，實驗過裝在 WordPress 5.4 上面會爆炸，也沒找到什麼其他替代方案，看來還是只能用 ACF 的 Relationship，或是找時間來修理它了...

其他免費外掛介紹如下：

[Members](https://tw.wordpress.org/plugins/members/) - 管理使用者角色與權限，這部分會在第六章介紹如何使用程式碼來進行設定  
[W3 Total Cache](https://tw.wordpress.org/plugins/w3-total-cache/) - 老牌快取外掛，會在第十四章介紹關於 WordPress 的快取機制  
[Yoast SEO](https://tw.wordpress.org/plugins/wordpress-seo/) - 功能強大的 SEO 外掛  

付費外掛介紹：

[Gravity Forms](https://www.gravityforms.com) - 設計表單的外掛，還可以整合為數眾多的第三方服務  
[BackupBuddy](https://ithemes.com/backupbuddy/) - 備份還原外掛  
[WP All Import](https://www.wpallimport.com) - 使用 XML 或 CSV 匯入網站資料

## **BuddyPress**

[BuddyPress](https://tw.wordpress.org/plugins/buddypress/) 一套專門建置社群平台的外掛，只要安裝啟用後，立刻可以完成類似 Facebook 的社群網站。  
第一版在 2009 年四月推出，已經跟 WordPress 走過數十個年頭。

BuddyPress 必須運作在 Multisite 多站網絡的環境下，而它可以跟大部分的佈景主題相容，如果使用有針對 BuddyPress 所開發的佈景主題相容性會更好。

有別於 WordPress 內建的資料表，BuddyPress 新增了許多表來紀錄資料，因為社群網站的內容格式與傳統的文章不大相同，如果要用自定義文章來客製的話會花不少功夫。所以使用 BuddyPress 的機制來開發會方便許多。

以下為 BuddyPress database schema 說明：

**wp_bp_activity** - 記錄貼文資料

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-07-at-09.36.01.jpg)

**wp_bp_activity_meta** - 記錄貼文自訂資料

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-07-at-09.37.34.jpg)

**wp_bp_invitations** - 記錄使用者邀請資訊

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-07-at-09.38.26.jpg)

**wp_bp_notifications** - 記錄使用者通知訊息

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-07-at-09.39.20.jpg)

**wp_bp_notifications_meta** \- 記錄使用者通知訊息的自訂資料

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-07-at-09.40.28.jpg)

**wp_bp_xprofile_data** - 記錄使用者資料

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-07-at-09.42.44-1024x148.jpg)

**wp_bp_xprofile_fields** - 記錄使用者欄位

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-07-at-09.43.56-1024x295.jpg)

**wp_bp_xprofile_groups** - 記錄使用者群組資訊

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-07-at-09.44.51-1024x140.jpg)

**wp_bp_xprofile_meta** - 記錄使用者自訂資料

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-07-at-09.45.37.jpg)

BuddyPress 有許多功能可以讓使用者決定是否要啟用：

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-07-at-09.46.55-1024x648.jpg)

在頁面設定的部分可以選擇 BuddyPress 相關頁面的指定，也可以使用頁面名稱來重新命名預設的顯示標題，另外要開放註冊的話還需要新增註冊與登入頁面，但前提是要先啟用讓任何人都可以註冊的選項。

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-07-at-09.50.55.jpg)

啟用 BuddyPress 後，可以在後台左側的使用者選單中，看到多出一個「簡介欄位」的選項，它可以讓你自行定義給使用者的資料欄位，像是地理位置、生日、喜愛演員、喜愛的顏色等等。這功能可以很方便的透過 UI 介面來管理使用者欄位，而不用寫程式碼。

![](https://oberonlai.blog/wp-content/uploads/2020/05/CleanShot-2020-05-07-at-09.57.13-1024x625.jpg)

> 上一次我玩 BuddyPress 大概是三年前的事了，當時還沒有這麼多功能，大部分都要自行客製化，更不用說自訂使用者欄位這功能，如果可以把它內建在 WordPress 裡面就好了～
> 
> 自訂使用者欄位以前用過這支 ---> [https://wordpress.org/plugins/user-meta/](https://wordpress.org/plugins/user-meta/)，現在多半統一用 ACF 來處理。

## **BuddyPress 外掛介紹**

[BuddyPress Media](https://wordpress.org/plugins/buddypress-media/)  
讓使用者可以在貼文中上傳圖片、聲音與影片，並且可以自行管理已上傳的內容

[BuddyPress Registration Options](https://wordpress.org/plugins/bp-registration-options/)  
控管瀏覽權限，新會員必須要管理員審核才可以瀏覽內容

[AppPresser Add-on](https://apppresser.com/introducing-appcommunity/)  
把 BuddyPress 變成手機應用程式

[BadgeOS Community Add-on](https://wordpress.org/plugins/badgeos-community-add-on/)  
增加徽章成就功能

[bbPress](https://wordpress.org/plugins/bbpress/)  
加入論壇功能