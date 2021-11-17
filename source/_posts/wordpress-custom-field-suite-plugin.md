---
title: Wordpress Custom Field Suite 新增欄位外掛介紹
tags: []
id: '2438'
categories:
  - - WordPress
date: 2015-07-12 15:14:01
---

[![custom-field-suite-16](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-16-1024x620.jpg)](http://www.prowebdesign.ro/flat-ui-dashboard-free-psd-by-prowebdesign/) 有使用過客製化後臺更新網站的朋友可能會知道，一般後臺會顯示修改前臺內容的控制欄位，譬如說更新公司資訊時，會有欄位可以輸入標題、內容、上架日期、更新時間、維護人等等，如果今天要多增加一個顯示圖片的內容，那就必須再請工程師另外再新增一個欄位來插入圖片。  

## 會遇到什麼問題？

Wordpress 本身是部落格系統，所以針對文章標題、分類、作者、日期等項目均有內建欄位，對於沒有複雜內容的需求還可以勉強使用，但如果今天我們的公司資訊有「文章上架日期」與「活動日期」兩種日期欄位，Wordpress 內建的控制項就會不夠了。
<!-- more -->
  小弟我以前的做法很直接，就是在文字編輯器裡面多打一行叫作活動日期，這樣的做法會有三個壞處：

1.  維護不方便：活動日期假設統一放在內容最下方，如果這篇文章有八萬個字，維護人員會要花費額外的時間搜尋活動日期內容寫在什麼地方。
2.  易造成版面失控：維護人員使用文字編輯器修改內容時，很容易因為編輯器本身的問題而造成版面走位，雖然現在有非常方便的「[WordPress Visual Composer 視覺化編輯器](https://oberonlai.blog/wordpress-for-business-website-tool/)」，但如果只是需要多一個日期欄位，應該還不需要用到牛刀，話說 Visual Composer 也越來越龐大了...
3.  無法針對資料加以利用：如果是用編輯器製作活動日期的欄位，在資料庫裡面它會被紀錄為文章的內容，這代表著如果今天我們想以活動日期來做文章排序，或是只撈出某個日期之後的活動，很抱歉，因為活動日期不是一個單獨的欄位，所以沒辦法針對它進行篩選或排序。

 

## Wordpress 的解決方案

當然，Wordpress 早就想到這個問題，並且提供了許多函式來處理這種狀況，最主要的就是 add\_meta\_box，引用台灣 Wordpress 界大神等級 Audi Lu 的文章來解釋如何運用 php 來新增自定欄位：[http://www.mrmu.com.tw/2011/03/30/wp-dev-meta-box-custom-fields/](http://www.mrmu.com.tw/2011/03/30/wp-dev-meta-box-custom-fields/)  

> 文章一開始提到的「英文選單標題」的例子，很適合當成範例說明，我們就來看看怎麼應用Meta Box吧，要試試看的話，首先打開您佈景中的functions.php，加入以下程式： <?php /\* 小範例：建立英文選單標題 \*/ add\_action(‘admin\_init’, ‘admin\_init\_fn’); // 指定後台初始化時要執行我們自訂的函式admin\_init\_fn function admin\_init\_fn() { // 使用add\_meta\_box來增加一個「自訂區塊」，在callback的參數位置，放上我們自訂的函式 EnTitleCB\_fn add\_meta\_box(‘EnTitle’, ‘英文標題’, ‘EnTitleCB\_fn’, ‘page’, ‘normal’, ‘high’, null); } // 當WP要畫出自訂區塊時，就會執行add\_meta\_box指定的這個callback函式 function EnTitleCB\_fn() { // 需要取得目前編輯的post資訊，如id global $post; // 把我們自訂的Post Meta從資料庫取出，待會放進欄位 // 看你需要多少欄位，就要執行幾次get\_post\_meta()來取值 $entitle\_cnt = get\_post\_meta( $post->ID, ‘EnTitle’, true ); ?> <th scope="row">主選單英文標題：</th> <td> <!– 開始畫出UI，看你需要多少欄位，就畫多少input出來 –> <label for="EnTitle"> <input id="EnTitle" type="text" size="75" name="EnTitle" value="<?php echo $entitle\_cnt;?>" /> <br /><em>說明：設定分頁出現在選單時的英文翻譯。</em> </label> </td> </tr> <?php } ?>

  上面的 code 寫完後還只是新增輸入欄位的介面而已，真正要能存進資料庫還要另外再寫程式碼，這篇文章寫得非常清楚，有興趣的朋友可以自行研究，而本文要介紹的是更快速的方法：使用外掛。  

## 就是它，Custom Field Suite

  [![custom-field-suite-1](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-1-1024x511.jpg)](https://wordpress.org/plugins/custom-field-suite/)   這一套外掛已經是我每個專案中必備的工具之一，原因是它可以非常方便的加入自訂欄位，而且還可以選擇十三種不同的欄位介面，像是日期選擇器、顏色選擇器、加入超連結等等，   除此之外，它還可以針對不同的自定義文章類型（custom post type）、管理者權限、頁面模版來顯示特定的欄位，譬如說新增的活動日期這個欄位，如果只想讓網站管理員看到就好，這支外掛也可針對這樣的需求進行設定。   使用的方法也非常簡單，只要先在後臺設定好欄位，然後加一段 php 的語法即可取得這個欄位的值，以下就上述所提到新增活動日期的例子進行簡單的說明。  

## 如何使用 Custom Field Suite

 

### 一、新增欄位群組

Custom Field Suite 是以群組為概念，也就是說一個群組裡面可以有無數個控制項，因此在規劃群組的時候盡量以功能來命名，而非網頁的內容，這樣才能方便重覆使用。   安裝好外掛後點選新增，即會出現下面的畫面。先輸入欄位群組的名稱，再點選增加欄位。   ![custom-field-suite-2](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-2-1024x580.jpg)  

### 二、編輯欄位

點選增加欄位後會出現以下的控制項，依序輸入欄位名稱與變數名稱，欄位名稱可任意輸入中英文，而變數名稱英數限定，因為這是稍後要給程式辨識的名稱，最後再從欄位類型的下拉選單選擇需要什麼類型的欄位，因為這個範例中我們的資料是活動日期，所以選擇日期即可。 ![custom-field-suite-3](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-3-1024x621.jpg)  

### 三、選擇該顯示於何種內容

設定好欄位後，再來選擇要把這個欄位群組決定顯示在那些頁面設定中，它可以針對圖中的方式進行設定，以這個例子中活動日期是屬於公司資訊的欄位，而假設公司資訊我們是用文章做的，然後只限定管理員才看的到這個欄位，可依照下圖進行設定。 ![custom-field-suite-4](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-4-1024x607.jpg)  

### 四、設定順序與位置

在其它設定中，可以決定欄位群組的順序與位置，還可以隱藏 Wordpress 內建的文字編輯器，如果要完全使用我們自行新增的欄位，這功能就非常好用，因為 Custom Field Suite 也有提供文字編輯器，這樣我們就可以指定特定的區塊才使用編輯器，而不會整頁的內容都被內建編輯器所控制。 ![custom-field-suite-5](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-5-1024x540.jpg)   完成上述設定後，可以再自行新增其它欄位，完成後點選發表即可。 ![custom-field-suite-6](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-6-1024x596.jpg)   接下來去文章編輯的頁面，就可以看到多了一個活動日期的欄位。 ![custom-field-suite-7](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-7-1024x763.jpg)  

### 五、將程式碼加入檔案中

最後，用你熟悉的網頁編輯器，打開 single.php，找到想要顯示活動日期的位置，把這段程式碼寫入即可：

> <php echo CFS()->get('my-date'); ?>

  記住，get 裡面的字串就是當初增加欄位的變數名稱，這名稱一定要符合，不然不會出現任何的結果。更多欄位的輸出方式可以參考這篇官方文件--->[http://customfieldsuite.com/projects/cfs/documentation/](http://customfieldsuite.com/projects/cfs/documentation/)  

## Custom Field Suite 的進階用法

Custom Field Suite 除了提供基本的文字欄位、下拉選單、日期選擇器等等十幾種好用的方便欄位外，還提供進階又實用的功能來做較複雜的需求，以下就我專案中常用的兩種功能進行介紹。  

### 一、相關文章

如果要製作相關文章或是其它推薦閱讀時，Wordpress 最常見的邏輯就是採用相同分類或是含有相同標籤的來顯示，但如果是要讓一篇文章顯示頁面的內容，就必須要手動加連結，如果只有一兩篇就還好，但萬一超過一定數量的話，維護起來便會造成麻煩。   Custom Field Suite 有提供顯示相關文章的欄位，它可以設定要顯示那種類型的內容。 ![custom-field-suite-8](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-8-1024x606.jpg)   新增設定完成後，在文章編輯的欄位上提供非常方便的介面做內容關連，除了可以使用關鍵字搜尋現有內容外，還可以用拖曳的方式來進行編排，這樣可以減低作業錯誤的機率。 ![custom-field-suite-9](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-9-1024x524.jpg)   後臺設定好之後，可以使用下列的語法把相關的文章顯示出來。

> <?php $values = CFS()->get('my-related'); foreach ($values as $post\_id) { $the\_post = get\_post($post\_id); echo $the\_post\->post\_title; } ?>

 

### 二、設定輪播廣告

當我們使用一些 jQuery 的 Slider 插件或自己寫的效果來整合至 Wordpress 的時候，如何讓維護人員方便更新這些輪播廣告就變得非常重要，運氣好的時候某些 jQuery Slider 會出 Wordpress 外掛，但大多時候是沒有的，所以除非我們一開始就直接採用有設計 Wordpress 外掛的 jQuery Slider，不然整合上會是個麻煩的問題。   好佳在 Custom Field Suite 提供了一種循環的功能，簡單說就是可以在欄位底下設定子欄位，然後重複這些父欄位，最後透過 php 把它呈現出來。   實際範例如下，假設我們的輪播廣告需要的欄位有上傳圖片、圖說、點擊連結，第一步我們新增一個欄位叫輪播廣告。   ![custom-field-suite-10](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-101-1024x578.jpg)   接著依序新增子欄位：上傳圖片、圖說、點擊連結，然後用拖曳的方式把這三個欄位移到輪播廣告下，移動成功後它們會呈現縮排的效果。因為輪播廣告通常只會出現在首頁，所以此欄位可以設定成只顯示在特定的文章或頁面即可。 ![custom-field-suite-11](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-11-1024x465.jpg) ![custom-field-suite-12](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-12-1024x632.jpg) ![custom-field-suite-13](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-13-1024x673.jpg) ![custom-field-suite-14](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-14-1024x416.jpg)   在前往文章的編輯頁面查看，會多了如圖示的區塊，每點擊一次新增就會出現輪播廣告所需要的欄位，即可完成新增。 ![custom-field-suite-15](https://oberonlai.blog/wp-content/uploads/2015/07/custom-field-suite-15-1024x798.jpg)   最後在需要顯示輪播廣告的地方，插入以下程式碼，在此以 [Flexslider](http://www.woothemes.com/flexslider/) 做為範例：

> <ul class="flexslider"> <php $fields = CFS()->get('my-loop'); foreach ($fields as $field) { echo "<li><a href='".$field\['my-link'\]."'><img src='".$field\['my-banner'\]."' /><br/>".$field\['my-desp'\]."</a></li>" } ?> </ul>

這樣就可以把輪播廣告跟 Wordpress 後臺做整合了。  

## 結語

Custom Field Suite 提供了許多方便實用的自訂欄位功能，讓不熟悉語法的朋友也可以做出功能非常完整的客製化後臺，另外也提供了相關文章、輪播廣告的整合，更可以針對內容、使用者、文章分類來顯示自訂欄位，作者也非常勤勞的在更新，是一支非常優質又免費的外掛。  

## 附記

該外掛的中文語系檔小弟已提交給作者，在作者尚未正式加入前，可從下方連結進行下載，解壓縮後放到 wp-content/plugin/custom-field-suite/languages 裡面即可，使用上有任何問題歡迎交流！ [中文化下載](https://oberonlai.blog/wp-content/uploads/2015/07/Wordpress-custom-field-suite-zh-TW.zip)