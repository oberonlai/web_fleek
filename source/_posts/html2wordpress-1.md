---
title: 寫給網頁設計師-如何整合html頁面至Wordpress（一）
tags: []
id: '748'
categories:
  - - WordPress
date: 2011-10-05 14:27:53
etoc: true
---

身為一位視覺設計師與前端設計師，之所以會選擇wordpress來做為內容管理系統，基上有三個原因，第一是它很漂亮，第二是它非常漂亮，第三是它超級漂亮，

從事靠視覺的職業，先看外表是首要條件，雖然醜的東西有很多也好用，但既然有又漂亮又好用的東西，為何不選它？ 

wordpress最讓我驚豔的是，它已從早期blog管理系統的定位，逐漸走向真正的內容管理系統之路，許多改良過後的功能與介面，非常適合像我這種完全不會程式語言又只會看外表的設計師～
<!-- more -->
在wordpress中，光是免費又漂亮的主題就早已是[堆](http://www.smashingmagazine.com/2011/07/05/free-wordpress-themes-2011-edition/)‧[積](http://free4w.com/free-wordpress-themes/20-free-wordpress-cms-themes.html)‧[如](http://www.yourdigitalspace.com/2011/01/100-awesome-free-wordpress-themes-for-2011/)‧[山](http://www.webdesignerdepot.com/2011/03/50-new-wordpress-themes-march-2011/)，

而需要付費的就更不用講，[woothemes](http://www.woothemes.com/themes/)、[studiopress](http://www.studiopress.com/themes)、[themeforest](http://themeforest.net/)，還有我最愛的[elegantthemes](http://www.elegantthemes.com/gallery/)，它可以直接在前端畫面中，切換背景顏色、底圖、pattern，最么壽的是，他們將近百種主題，一年只要繳39塊美金就可以使用，在台灣要花2000塊請人設計網站，大概只能找學生吧。

[woothemes](http://www.woothemes.com/themes/) ![woothemes](https://oberonlai.blog/wp-content/uploads/2011/10/theme1.jpg "theme1") [studiopress](http://www.studiopress.com/themes) ![studiopress](https://oberonlai.blog/wp-content/uploads/2011/10/theme2.jpg "theme2") [themeforest](http://themeforest.net/) ![themeforest](https://oberonlai.blog/wp-content/uploads/2011/10/theme3.jpg "theme3") [elegantthemes](http://www.elegantthemes.com/gallery/) ![elegantthemes](https://oberonlai.blog/wp-content/uploads/2011/10/theme4.jpg "theme4") 

但回過頭來看，這些主題既漂亮又便宜，功能又完整，叫吃我們這行飯的人混屁啊???

好友阿毛寫了一篇[十個讓設計師不被取代的能力](http://ah-mow-studio.tumblr.com/post/7379064305)，就客觀的生存條件而言，他寫的非常好，適合所有剛踏進這一行的人學習；

但我就比較蠢了，寫不出這麼多大道理來，很單純的就只是覺得「案!我也要搞出這樣漂亮的網頁」，會讓我從事這行，也就是這麼簡單，腦海中的想法可以實現出來，那爽度絕對是比在草原上騎馬並「已經滿了」還要爽。

而能夠不用會程式，還能架出後臺管理系統，這實在是有一種當神的快感(事實證明，這樣非常危險，還是要有一位工程師朋友較為安全)，可以狂安裝模組完成各種功能，這爽度也絕對不會輸給在草原上騎馬並「漫出來」，當時就在想，如果能夠設計自己的主題不就爽翻天了，於是開始立刻動手了解wp(wordpress)的主題架構，結果東搞稿西稿稿，得到的成果是....死機。 

從無到有的自己生一套主題出來，實在是不符合網路世界中的分享精神，是地，沒錯，就是跟你想的一樣，去抓別人的來改，啟動修改大師機制，我找到了這一套主題-[SimpleFolio](http://coding.smashingmagazine.com/2010/02/07/simplefolio-a-free-clean-portfolio-wordpress-theme/)，使用說明「The theme is released under GPL. You can use it for all your projects for free and without any restrictions. You may modify the theme as you wish.」 馬都停在你眼前了，那有不騎的道理(是草原上的馬)，

我花了好大一番工夫(約十分鐘)才把裡面的樣式全部清光光，成為一個空白的主題web_templates，使用上非常簡單，與一般的主題安裝方法一樣，安裝完成後，我們就來開始整合設計好的html頁面與wordpress吧! 請享用 下載[web_templates](https://oberonlai.blog/wp-content/uploads/2011/10/web_templates.zip)

## 基本觀念

在我小時的印象，變形金鋼是可以合體的，不知為何現在的真人版不行，反正現在都是一隻隻獨幹，沒有合體變超強的觀念，小時候的機器人卡通沒有合體就不可能贏得了敵人，而且要合體才夠帥，不然只靠獨幹都是廢渣一群； 

網頁程式的架構也是如此，一頁頁的做雖然還不到是廢渣，但建置所有頁面的html著實非常累人，以前的我傻傻不懂，一頁一頁慢慢建，當要修改所有頁面的slogan，不多，一個30頁的小網站，改一次大概二十分鐘，但如果瘋狂的改來改去，這二十分鐘就和二十年一樣久，滿腹的委屈和怒氣隨時會發洩在草原的馬身上。 

這時就需要來個大無敵合體，頭龜歸頭、腳歸腳、手歸手，各自負責合體機器人的每個部份，萬一頭殼壞了，不用全部拆掉修理，只要修頭就好，這就是俗稱的頭痛醫頭，腳痛醫腳，但前題是你的身體體質要好，不然合在一樣沒祿用。 

網頁程式架構的概念就是這樣說的(亂凹)，先把統一會出現在所有頁面的元素抽離出來，像是選單、小功能等，然後有組織的安排，與原有的良好體質做結合，就是一臺超強的[六神無主](http://blog.yam.com/ken8286/article/35727295)了，當要修改時，只要針對架構中的選單做修改即可，再也不用每一頁都要改，一次搞定，又開始覺得很爽了嗎?

## 準備好你的基本html+css

這是廢話， 不然是鬼要跟wordpress整合嗎!?我目前都是使用dreamweaver的範本功能，只要用簡單的標籤，就可以把重覆的區塊獨立出來，相關的使用方式可以參考[官方說明](http://help.adobe.com/zh_TW/Contribute/4.1/help.html?content=con_pages_pg_28.html).....對，很難看，有需要的話我再寫一篇我是如何使用的，

但依照本人部落格的更新速度，很趕的你還是問谷姐的哥哥先。 總之在還沒認識wordpress之前，我都是用dreamweaver範本先做，但現在有wordpress也可以用它來發展頁面，只是要先定義清楚架構的元素，其餘部份就好辦事了。

基本上你至少要先做首頁和一頁內頁，首頁基本上版型都會長得跟內頁不同，所以要先獨立做出來，而內頁就是要定義你這網站中所有頁面共同的元素，像次選單、次功能之類的區塊。

## 認識資料夾結構

wordpress的資料夾結構是把所有關於主題的內容，放在wp-content\themes\web_templates，而在templates的根目錄中一定要有一份css來做對應， web_templates的內容很簡單，有兩個資料夾images和js，顧名思義的放就對了，整個模版最主要的檔案是index.php，

它是定義整個網站所有頁面的主架構，也就是合體金鋼的身體， 有了這架構，才可以把各區塊像頭-header.php、手-pages.php等給拉進來，index.php的程式不會太難懂，如果你是本格派的html編輯員，大概就可以了解它在幹麻，簡單說就是把各部份給呼叫進來，

![wordpress的index](https://oberonlai.blog/wp-content/uploads/2011/09/011.jpg "index.php") 

頭尾依序呼叫header.php、sidebar.php、footer.php，是整站部落格的首頁 至於其它部份我大概稍微解釋一下 header.php --- 控制所有頁面<head></head>之間的東西，而在這邊我是把主選單和logo的div放在這個檔案之中，

老話一句，只要你做網頁是手寫html的人，這部份不難懂，都可以自行再調整，如果不是的話，那就先去[w3school](http://www.w3schools.com/)上上課吧。

![控制所有頁面的title](https://oberonlai.blog/wp-content/uploads/2011/09/header011.jpg "header01") 

這邊是控制所有頁面的title ![控制連結檔案js、css](https://oberonlai.blog/wp-content/uploads/2011/09/header02.jpg "header02") 

這邊是控制外部檔案的連結路徑 ![自己寫的div header部份](https://oberonlai.blog/wp-content/uploads/2011/09/header03.jpg "header03") 

通常我都是把自己寫的header部份放在這邊，比較需要注意的部份，就是點LOGO返回首頁的路徑可採用這樣的寫法(日後會常用到)，還有就是我把主選單也是放在這邊，關於選單的做法wp有非常高明的解決辦法，稍後將會提到如何製作選單；

最後則是要記得該檔案不用把div標籤做結尾，它的結尾出現在footer.php裡面。 sidebar.php --- 控制側邊欄位的區塊，可以從後臺的 外觀 > 模組來做新增。

![](https://oberonlai.blog/wp-content/uploads/2011/09/sidebar.jpg "sidebar") 

在這頁面中除了可以看到Sidebar widget之外，還可以看到下方有一個Home widget，這跟我們待會要使用的模版有關係，稍後將會解釋。 footer.php --- 控制頁尾資訊。 functions.php --- 控制所有的功能，

這邊非常危險，沒事不要往裡面跑，會出人命的。 page.php --- wp內建的網誌分頁模版，通常我都直接當成"網頁"來理解它，因為所有的頁面都是由它構成，另外它有呼叫Sidebar，只要在模組中有增加功能，都會在page中呈現。

![page.php的內容](https://oberonlai.blog/wp-content/uploads/2011/09/page.jpg "page") 

以下兩個檔案是[SimpleFolio](http://coding.smashingmagazine.com/2010/02/07/simplefolio-a-free-clean-portfolio-wordpress-theme/)獨有的，而它們也是建立頁面的關鍵 template-fullwidth.php --- 網誌分頁的模版，稍待在新增頁面的時候會需要它，它是控制所有內頁區塊中的相同元素。聰明的你可能會想到它跟page.php有何不同，

簡單說，page.php有呼叫sidebar而它沒有，所以在一些不想要有sidebar或是想自行設計側邊欄的頁面上，就可以使用這套模版。

![沒有側邊欄](https://oberonlai.blog/wp-content/uploads/2011/09/nosidebar.jpg "nosidebar") 

沒有側邊欄 ![有側邊欄](https://oberonlai.blog/wp-content/uploads/2011/09/withsidebar.jpg "withsidebar") 

有側邊欄 template-home.php --- 這是[SimpleFolio](http://coding.smashingmagazine.com/2010/02/07/simplefolio-a-free-clean-portfolio-wordpress-theme/)的客製化首頁，包含一個slider banner和可以用模組控制的首頁區塊。

![客製化首頁](https://oberonlai.blog/wp-content/uploads/2011/09/template-home.jpg "template-home") 

首頁輪播橫幅以及區塊顯示 Home widget --- 此功能與Sidebar完全相同，差別在於它是控制template-home.php首頁的下方區塊。

![homewidget](https://oberonlai.blog/wp-content/uploads/2011/09/homewidget.jpg "homewidget") 

可以套用現成外掛功能或是自行新增html定義標籤。