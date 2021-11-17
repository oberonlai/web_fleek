---
title: WordPress Starter 佈景主題，好的開始是成功的一半！
tags: []
id: '2698'
categories:
  - - WordPress
date: 2016-08-31 12:16:49
---

 

## 什麼是 WordPress Starter 佈景主題？

  以前在本站介紹過許多 [WordPress 付費佈景主題](https://oberonlai.blog/wordpress-also-can-do/)，不管是介面設計或是功能上，都非常的漂亮且完整，然而今天我們要介紹的是「什麼都沒有」的佈景主題，也就是空白的佈景主題。   可能有朋友會好奇空白的佈景主題可以拿來幹麻？簡單來說這類的佈景主題是提供給專業的網頁設計人員進行 WordPress 整合的工具，因為現成的 Theme 都已經被設計得的非常完整，如果要進行客製化，勢必就要研究程式碼，有時候如果設計好的版型難以找到類似的佈景主題，或是不想花時間修改別人的程式碼，空白的佈景主題也就是 Starter Theme 就非常好用了。
<!-- more -->
WordPress Starter 佈景主題的特色就是可能只包含一些基本的 CSS、JavaScript，以及內建現成的 PHP 函式讓設計師可以快速運用，只要頁面設計的部份已經請前端工程師切好版，使用 Starter Theme 就非常容易做 WordPress 整合。  

## 市面上的 Starter Theme

 

### Underscores

  ![](https://oberonlai.blog/wp-content/uploads/2016/08/wordpress-starter-theme-1.png) 目前最知名的應該是由 WordPress 母公司 Automatic 推出的 [Underscores](http://underscores.me)，它的特色在於因為是 WordPress 開發公司製做的，所以在更新上還算頻繁。內建兩種 layout、下拉選單效果、標籤模版等等的內容，最特別的是他們提供的線上工具，可以輸入 theme name、slug、author 等欄位，就可以下載帶有這些資訊的 Starter Theme。  

### Sage

  ![](https://oberonlai.blog/wp-content/uploads/2016/08/wordpress-starter-theme-2-1024x699.png) 第二套是常常致力於研究 WordPress 開發流程的公司 [Roots](https://roots.io/sage/) 他們所推出的 Starter Theme [Sage](https://roots.io/sage/)，有在使用套件管理工具如 Gulp、Bower 的朋友應該會很喜愛這套，因為他們就是根據該族群的使用習慣開發出這套佈景主題，並且還整合了 Bootstrap、HTML5 Boilerplate 等 Framework，能讓熟悉這些工具的朋友更容易上手。  

### FoundationPress

  ![](https://oberonlai.blog/wp-content/uploads/2016/08/wordpress-starter-theme-3-1024x608.png) 知名的 CSS Framework Foundation 所推出的 [FoundationPress](https://foundationpress.olefredrik.com)，對於習慣使用 Foundation 的朋友簡直是一大福音，這套 Starter Theme 除了包含四種 Layout 以外，還有方便的 UI Kit 可以直接使用，甚至還販售 Photoshop UI Kit 設計稿( 7 美元)，可說是很完整的解決方案。   其它還有許多各試各樣的 Starter Theme，有興趣的朋友可以參考這篇--->[http://www.hongkiat.com/blog/starter-wordpress-themes/](http://www.hongkiat.com/blog/starter-wordpress-themes/)  

## 挑選 Starter Theme 的原則

  以我自己的開發經驗，檔案到我手上都已經是完整切好的 html 檔，不管是引入的 CSS 或 JS，前端工程師都已經非常有條理的整理完成，所以我要做的就是不破壞它原本的架構並且整合到 WordPress 之中。所以我不需要有套件管理或是其它 Framework 的 Starter Theme，越空白越乾淨的越好。  

### HTML5 Blank WordPress Theme

  ![](https://oberonlai.blog/wp-content/uploads/2016/08/wordpress-starter-theme-4-1024x643.png) 當初花了一些時間進行實際安裝、評估比較，最後在經歷過一些專案實戰後，我發現 [HTML5 Blank WordPress Theme](http://html5blank.com) 這套是最符合自己的需求。它的優點在於它真的很白、非常白，沒帶有任何的 JS、CSS，內建的函式又非常實用，可惜已經好久沒更新了，所以乾脆整合實務中的專案經驗，把它修改成最符合自己需要的 Starter Theme，以下就我修改的部份做一個簡單的介紹：  

### 一、資料夾結構

  有些 Starter Theme 為了提供更多的基礎功能以及模組化，會分許多資料夾來做整理，但每個人整理的習慣都不太一樣，我自己是比較偏好越扁平、越能一眼看到所有檔案的結構。 ![](https://oberonlai.blog/wp-content/uploads/2016/08/wordpress-starter-theme-5.png)   這份 Starter Theme 包含六個資料夾，其中兩個是跟 WordPress 相關、另外四個是前端程式的資源。 languages 是該主題的語系檔，而 include 資料夾是從 function.php 所拆分出來的檔案，用意是避免 function.php 變得落落長而難以維護，主要分成以下六個部份：   ![](https://oberonlai.blog/wp-content/uploads/2016/08/wordpress-starter-theme-6.png)  

*   admin.php：與 WordPress 後臺有關的自定功能，像是關閉後臺的訊息、更改登入頁面等等
*   cpt.php：新增的自定義文章與自定義類別
*   nav.php：註冊選單、改變選單的 html 都放在這邊控制
*   shortcode.php：定義全站的 shortcode
*   tool.php：根據專案上常碰到的需求，撰寫一些實用的 PHP 函式
*   widget：註冊側邊欄小工具

  至於資料夾以外的則是最基本、WordPress 各頁面的模版檔案，唯一兩個例外是 loop.php 與 pagination.php。   loop.php 在做 wp query 的時候非常方便，要定義迴圈內的 html 時，可以直接引用 loop.php，寫法是 get\_template\_part("loop.php")，所以未來 html 要變動時，直接修改 loop.php 即可。而 pagination.php 是控制分頁選單，也常常會伴隨著 wp query 一起出現，使用方法跟 loop.php 一樣。   這包 Theme 所有的檔案就這樣，沒了，是不是非常的簡單易瞭？不管是要加入額外的模版檔、還是新增自己慣用的分類方式，都可以依照實際狀再行調整。  

### header.php

  ![](https://oberonlai.blog/wp-content/uploads/2016/08/wordpress-starter-theme-7.png)   在 header.php 的部份，宣告了資料結構 schema，如需導入環境已備妥，還有加入 Facebook 的 og tag，這是每個業主的必備需求:D，基本的 meta 資訊有搭配 API 做基本的判斷，像是 getDesp() 會判斷如果文章有摘要則顯示摘要、沒有的話則顯示全站的描述。   getKeyowrd() 則是判斷如果文章有設定標籤，則會出現 meta keyowrd 的資訊 ，雖然各家搜尋引擎已經對 keyword 不予重視，但相信做該做的還是會有影響力。   getImage() 則是為了解決客戶常常說文章沒有圖片而無資料可顯示的窘境，所以依照以下順序判斷來抓取圖片：特色圖片 -> 文章內的第一張圖 -> 預設圖。以上這些小功能都寫在 include/tool.php 裡面，有興趣的朋友可以自行研究或是依專案狀況進行調整。   另外其它的 meta 我是能省則省，有特殊需求可以參考這份文件，它把所有可能會用到的 meta 都整理出來： [https://github.com/joshbuchea/HEAD](https://github.com/joshbuchea/HEAD)  

### function.php

  ![](https://oberonlai.blog/wp-content/uploads/2016/08/wordpress-starter-theme-8.png)   因為已經把功能拆分開來了，所以原本的 function.php 只留下一些基本的設定與 hook，載入外部檔案的部份就是 include 中拆分出來的六個檔案，如果某些功能已經用外掛做了，就可以把它註解掉，以維持檔案的精簡。   載入網站資源官方建議使用 [wp\_register\_script](https://developer.wordpress.org/reference/functions/wp_register_script/) 而非直接寫在 header.php，好處是可以避免檔案衝突以及確保執行順序，但我個人習慣還是看狀況，有時候直接寫在 header.php 會比較好掌控。   function.php 最下方的一堆 hook，除了掛載函式以外，還有一些基本的 WordPress 防護措施，像是移除版本號、關閉 API 接口、在前臺登入狀態下隱藏工具列等等，有特別需求的話都可以再行自訂。  

## 小結

  Starter Theme 可以節省許多重覆開發的時間，不用費心從無到有建立或是去研究已經寫好的一堆程式碼，讓 WordPress 工程師可以專注在整合系統上，更有效增加整體專案開發的流暢度。  

## 主題下載

  非常歡迎大家在使用上的意見回饋，我相信這份主題一定還是有不足或是更好的地方，還請各位大大多多指教！   [Github](https://github.com/m615926/starter) [Zip 下載](https://github.com/m615926/starter/archive/master.zip)