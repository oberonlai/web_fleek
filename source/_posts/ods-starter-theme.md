---
title: 【 工具 】使用 ODS starter theme 開發 WordPress 佈景主題
tags:
  - f2e
  - html5
id: '1176'
categories:
  - - F2E
  - - HTML5
  - - JavaScript
  - - PHP
  - - saas
  - - WordPress
date: 2020-04-18 10:39:25
etoc: true
---

## 前言

坦白說做了這麼多年的專案，始終覺得沒有找到一套適合自己的 Starter Theme，不管是官方的還是很多社群朋友推薦的，用起來都還是有卡卡的感覺。

後來看到現在很多網路公司都會開發自己的 [Design System](https://designsystemsrepo.com/design-systems-recent)，驚覺到要有一套自己的系統才能讓整個工作流程更加順暢，進而建立自己的標準化規格，讓未來的自己好過些。

以往在進行 A 專案的時候，都會去爬以前寫過的程式碼，它們可能散落在 BCD 專案裡面，但時間一久再加上年紀變大，常常想不起來之前寫過的東西放在哪個專案裡面。

另一方面，自己在寫的時候很少遵循什麼 Code Standard，所以就算找到之前的程式碼，也會先碎碎念自己以前怎麼會把 Code 寫成這副德行，然後真的要用的時候都還要再花時間進行整理。

而且雖然是自己寫的東西，但不少的寫法都是 Google 來的，為什麼要這樣寫？這樣寫的用意是什麼？當專案結束拿到尾款後，這些問題就也隨風而去了，當下次再碰到類似的狀況要修改成不同的功能，又要再花很多時間來重新研究。

基於這些理由，[ODS - Oberon Design System](https://ods.obeornlai.blog) （歐迪賽系統XD）就誕生了～～～

<!--more-->

[![](https://oberonlai.blog/wp-content/uploads/2020/04/screenshot-1024x768.jpg)](https://ods.oberonlai.blog)

## 什麼是 Oberon Design System？

歐迪賽系統分為兩個部分， UI 元件與 WordPress PHP Function。我把近期的專案整理成一個一個獨立的 UI 元件，然後在做 WordPress 的時候也把常用到的 WP\_Query、Function 根據範本收集起來，並且做了截圖以及功能的說明。

![](https://oberonlai.blog/wp-content/uploads/2020/04/CleanShot-2020-04-18-at-09.53.58.jpg)

每個 UI 元件會有四個不同的頁籤，分別是不同開發階段的程式碼，有前端的 Pug、Scss 與 JS，以及在 WordPress 之中要如何實現這個元件的 PHP 寫法。

![](https://oberonlai.blog/wp-content/uploads/2020/04/CleanShot-2020-04-25-at-12.10.16.jpg)

針對 WordPress 的部分，依照開發 Theme、Plugin、WP-Admin 與 WooCommerce 來區分，然後把註解附上，幫助自己之後要用的時候能理解程式的寫法。

PHP 的部分可以直接使用，但前端的部分因為我是根據自己的工作流程所整理的，因此會包含一些打包工具的設定，像是 Pug、Scss 的編譯、ES6 的 Module 與 Babel，這部分就是 ODS Starter Theme 派上用場的時候了。

## ODS Starter Theme 介紹

以前我在開始新專案的時候，會直接把上一個專案的資料夾直接複製一份出來，然後把裡面用不到的東西刪一刪，這樣做的問題是很多東西不敢刪，因為怕會刪到之後可能要用到的東西，搞得還是要去上一個專案找程式碼來複製貼上。

但有了 ODS 之後，我可以把 Starter Theme 精簡到不能再精簡，只留下最必要的基本功能與範本，遇到要用的東西再去 ODS 複製貼上就好，這樣就再也不用擔心會漏掉什麼必要的功能而要把所有東西都塞進去了。

ODS Starter Theme 從這邊下載---> [](https://github.com/oberonlai/ods)[https://github.com/oberonlai/ods](https://github.com/oberonlai/ods)**，**以下就資料夾結構做說明：

### **themes/ods/src**

該資料夾放的是所有前端開發需要用到的檔案，包含 Pug、Scss、JS 與圖片。

### **themes/ods/src/\_layouts**

該資料夾放的是 Pug 的核心模板 temp.pug，它定義了所有頁面的基本框架，以及需要引入的模板。

### **themes/ods/src/\_partials**

該資料夾放的是 Pug 的區塊模板，像是 Header、Footer 以及其他可以模組化的元件。

### **themes/ods/assets**

執行 npm run build 之後會產生的靜態資源檔，也就是編譯 img、js、css 的資料夾，這些是 Theme 需要引入的檔案。

### **themes/ods/dev**

執行 npm run start 之後產生的靜態資源檔，通常為前端開發時所用的資源，因此除了 img、css、js 以外，還會包含編譯 Pug 後的 Html 檔。

其他根目錄中的 PHP 檔就是 WordPress Theme 的基本檔案，排除各種範本只留下最基本的 index.php，以及 header 與 footer，functions.php 只放了引入靜態資源檔的 Action 以及一些版本資訊移除的 Filter，和根據案件需求啟用或關閉 Gutenberg 編輯器的 Filter。

就這樣，沒有預設任何資料夾的整理方式，只留下必要的檔案，要找 Code 去 ODS 找，要找範本去 [Theme Hierarchy](https://developer.wordpress.org/themes/basics/template-hierarchy/) 找，用久用熟了很自然的就能針對需要的內容開相對應的範本檔案，就不用再留下一堆可能用不到的東西了。

至於註冊 Custom Post Type、Taxonomy、WordPress API 一律放在外掛來處理，Theme 只留下最基本的外觀樣式。

## ODS Starter Theme 使用說明書

接下來用我在實際專案上的工作流程來說明如何使用 ODS Starter Theme：

### **前置作業**

主要是安裝 Node 與 NPM，以及最重要的 WordPress 本機開發環境，Node 與 NPM 根據使用的作業系統有不同的安裝方法，我是 Mac 我用 [Homebrew](https://www.itread01.com/content/1550405718.html)。

本機開發環境我用 Valet，可以參考我之前的整理的文章 - [Mac 限定 WordPress 本機開發環境 Valet + ValetPress](https://oberonlai.blog/valetpress-setup/)，也可以使用 [Local](https://localwp.com) ，安裝上方便很多而且 PC 跟 Mac 都可以使用。

環境都搞定之後，先把 [ODS Starter Theme](https://github.com/oberonlai/ods) clone 到本機 WordPress wp-content/themes 的目錄底下，如果是用 ValetPress 的話更方便，把 git clone https://github.com/oberonlai/ods.git 寫在腳本裡面，這樣新開一個站的時候就可以自動把 Theme 安裝好並且啟用，甚至把下面相關的指令一併變成腳本。

接下來先安裝一個很方便的套件 npm 叫做 [npm-upgrade](https://www.npmjs.com/package/npm-upgrade)，它可以偵測再 package.json 裡面的套件是否有新的版本，輸入以下指令進行安裝：

$ npm i -g npm-upgrade

接下來開啟 Terminal 把目錄切換到 wp-content/themes/ods 底下，先跑一次 npm-update

wp-content/themes/ods$ npm-upgrade

Checking for outdated dependencies for "/Users/oberonlai/Sites/Hellowp/wp-content/themes/ods/package.json"...
\[====================\] 6/6 100%

New versions of active modules available:

  uikit                                 ^3.4.0   →   ^3.4.1 
  parcel-plugin-custom-dist-structure   ^1.1.3   →   ^1.1.9 

? Update "uikit" in package.json from ^3.4.0 to ^3.4.1? Yes

? Update "parcel-plugin-custom-dist-structure" in package.json from ^1.1.3 to ^1.1.9? Yes


These packages will be updated:

  uikit                                 ^3.4.0   →   ^3.4.1 
  parcel-plugin-custom-dist-structure   ^1.1.3   →   ^1.1.9 

? Update package.json? Yes

它會逐一問你是否要更新現有的 Node 套件，跟 WordPress 外掛一樣，為了安全性考量，每個新的專案我都會習慣先更新，接下來才會安裝它們：

wp-content/themes/ods$ npm install

npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN ods No description
npm WARN ods No repository field.
npm WARN ods No license field.

removed 1 package, updated 3 packages and audited 10259 packages in 12.748s

23 packages are looking for funding
  run \`npm fund\` for details

found 0 vulnerabilities

到這邊 Node 的環境就準備完成了，接下來是開發工具的介紹。

### **Parcel**

[![](https://oberonlai.blog/wp-content/uploads/2020/04/CleanShot-2020-04-18-at-11.13.47.jpg)](https://parceljs.org)

前端開發的流程越來越複雜，常常有時候光設定環境半天就去了而且還搞不定 (怒)，於是有了 Parcel 的出現來拯救大家。

這套打包工具我從前年就注意到了，它號稱不用寫任何設定檔，只要裝上之後，它就會根據你寫的語言來自動安裝相對應的套件，十分方便，而且編譯速度飛快，再搭配快取的話一整個爽度十足。

![](https://oberonlai.blog/wp-content/uploads/2020/04/CleanShot-2020-04-18-at-11.51.03.jpg)

但也因為這樣的方便性，當想要做一些客製化調整的時候讓我完全不知道怎麼下手，再加上東西太新相關討論太少，所以遲遲無法把它用在正式的專案上。

去年年中推出了 Parcel 2，非常興奮的立馬來測試，結果還是無法解決其中一個我最在意的問題：那就是編譯後根據檔案格式來分類資料夾，因為預設的狀況是所有編譯好的檔案全都會在同一層目錄，看了超礙眼。

直到今年年初想說來看看有沒有解決這個問題了，但 Parcel 2 目前還有上百個 issue 還沒解決，更不用說我在意的問題被排在好後面，準備放棄之餘，碰巧逛到神人出來拯救大眾了，這位大大叫 Vladimir Mikulic，他寫了一支 [parcel-plugin-custom-dist-structure](https://github.com/VladimirMikulic/parcel-plugin-custom-dist-structure) 可以完美分類 dist 資料夾，而且設定十分簡單，

只要在 package.json 裡面定義好某種格式的檔案會去到哪一個資料夾名稱，剩下的它會自動幫你搞定。

"customDistStructure": {
    "config": {
      ".js": "js",
      ".css": "css",
      "img": \[
        ".jpg",
        ".png",
        ".svg"
      \]
    },
    "options": {
      "development": true
    }
  }

.js 檔會去 js 資料夾，.jpg、.png、.svg 會去 img 資料夾，這樣就可以解決我的問題了，而且 Vladimir Mikulic 大大超用心，回報 issue 每天都會即時更新處理狀態，實在是太感謝他，讓我終於能在正式專案中使用 Parcel 了！

目前我是用 Parcel 1 的版本，因為這支外掛只支援一代，Parcel 與分類資料夾的外掛都已經定義在 ODS 的 package.json 裡面了，所以只要下 npm install 就可以一次安裝完成。

需要注意的地方是 scripts 的部分：

"scripts": {
  "build": "parcel build --no-content-hash src/js/script.js src/sass/style.scss src/img/\*\* --no-source-maps -d assets",
  "start": "parcel serve src/\*.pug --no-source-maps -d dev"
},

當執行 npm run start 的時候，會啟用 Parcel 的本地環境，然後會即時更新所有檔案的修改狀態，預設 port 是 localhost:1234，然後會把檔案編譯到 dev 這跟資料夾

當執行 npm run build 的時候，會執行 Parcel 的 Build 指令，也就是編譯出正式環境需要用的檔案，給 Theme 的只需要靜態資源檔，然後放在 assets 這個資料夾，wp\_enqueue\_script 的檔案路徑也是指到這邊。

更多 Parcel CLI 可以參考官方文件 ---> [https://zh-tw.parceljs.org/cli.html](https://zh-tw.parceljs.org/cli.html)

### **UIKit**

[![](https://oberonlai.blog/wp-content/uploads/2020/04/CleanShot-2020-04-18-at-12.28.17-1024x746.jpg)](https://getuikit.com)

自從三年前發現了它之後，它現在是我每個專案的必備 CSS 框架，與最熱門的 Bootstrap 或其他框架不同，它不用再裝其他套件就可以使用 Modal、Slideshow、Sticky Menu 之類的效果，

當然也提供了很多 CSS class 來做版面的設計，像是 Container、Grid、Column，它提供的元件超級豐富也超實用，我沒事的時候就會打開他們的網站把每個元件都瀏覽一遍用法，現在都能把常用的背起來了。

另一方面，它的 JS API 也非常完整，完整到我可以借用它來開發[一套編輯器](https://oberonlai.blog/flash-stories-technical/)，下面會說明在前端開發時如何運用以及客製化 UIKit，ODS 列出的所有元件，都是根據它衍伸而來的，熟悉它了之後做介面就會超快而且又有組織。

跟 Parcel 一樣，UIkit 已經整合到 ODS Starter Theme 裡面，安裝一次 npm install 後即可使用。

### **Pug**

[![](https://oberonlai.blog/wp-content/uploads/2020/04/1CO3fAi5OwGbOEU6k1eueJA-1024x576.jpeg)](https://pugjs.org/api/getting-started.html)

Pug 可以省去寫 HTML 關閉的標籤，還可以少打 class、style 這些屬性的英文字，然後透過縮排來定義層級，再搭配變數、迴圈、判斷式、引入等等的功能來模組化 HTML。

其中最方便的地方是在於資料結構的定義，找到 themes/ods/src/ 裡面有一支 pug.config.js 的設定檔，可以事先定義好需要用到的變數並且用物件的方式來呈現。

module.exports = {
  locals: {
    header: \[
      {
        navbar: \[
          {
            src: "img/logo.svg",
            nav: \[
              {
                text: "Menu1",
                href: "#"
              },
              {
                text: "Menu2",
                href: "#"
              }
            \]
          }
        \]
      }
    \],
    frontPage: \[
      {
        slidershow: \[
          {
            src: "img/default.jpg",
            title: "文章標題",
            desc:"文章摘要",
            text: "閱讀更多",
            href: "#"
          },
          {
            src: "img/default.jpg",
            title: "文章標題",
            desc:"文章摘要",
            text: "閱讀更多",
            href: "#"
          }
        \],   
        loop: \[
          {
            title: "最新消息",
            titleEn: "LATESTED NEWS",
            list: \[
              {
                src: "img/default.jpg",
                title: "文章標題",
                date: "2019.05.21",
                href: "#",
                text: "文章分類"
              }
            \]
          }
        \],
      }
    \],
  }
};

通常拿到這設計稿的時候，我會先把這份資料整理出來，先從 Header 與 Footer 的資料開始，像是選單有哪些、連結有什麼，再來是分頁面，首頁 Front Page、關於頁 About，把頁面裡面所有的文字都打好 JSON。

這樣做的好處是在整理 JSON 的時候我就可以先定義之後做 ACF 要開的欄位名稱，以及順過所有欄位的邏輯與定義，一開始會花上不少時間做這份 JSON，但之後就會很開心，因為在寫 HTML 的時候就不用一直複製貼上了，範例如下：

each i in header
  header.uk-padding
    each j in i.navbar
      h1.uk-text-center#logo
        img.uk-width-1-6(src=j.src)
      ul.uk-flex.uk-flex-center.uk-margin-top
        each k in j.nav
          li.uk-margin-left.uk-margin-right
            a.text-dark(href=k.href)=k.text

使用 each 把 header 陣列裡面的東西印出來，然後再使用第二次 each 把 header 裡面的 navbar 陣列印出來，第三次把 navbar 裡面的 nav 選單印出來，需要分多少階層看個人習慣，我是喜歡把屬於 Header 的東西都塞在同一個陣列裡面。

最開心的部分是 each k in j.nav 這邊，自動就會撈出 nav 裡面的東西，所以我就不用 li 重複貼上好幾次，要修改 class 的話只要改一個就好。

更進階的用法是可以加入判斷式，範例如下：

ul.uk-flex.uk-flex-center.uk-margin-top
  each k,index in j.nav
    if index===0
      li.uk-margin-left.uk-margin-right.uk-active
        a.text-dark(href=k.href)=k.text
    else
      li.uk-margin-left.uk-margin-right
        a.text-dark(href=k.href)=k.text

多一個參數叫做 index，當 index = 0 也就是第一個選單的時候，li 多加一個 uk-active 的 class 來表示目前所在頁面的選單套用不同的樣式，或是也可以把 class 寫在 JSON 裡面，這樣就可以很靈活的來控制迴圈裡面的東西。

其他還有 Extends、Block 也是常會用的東西，所有頁面的基本框架是放在 src/\_layouts/temp.pug 裡面，而首頁 index.pug 則是延伸了 temp.pug：

doctype html
html
  head
    title ODS Starter
    meta(charset='utf-8')
    meta(name='viewport' content='width=device-width, initial-scale=1.0, maximum-scale=1')
    link(rel='stylesheet' href='/sass/style.scss') 
    link(rel='icon' href='/img/fav.png' type='image/png')
    block head\_end

  body
    
    include ../\_partials/header.pug

    //- 內容區塊
    .content
      block content
    
    include ../\_partials/footer.pug

    script(src='/js/script.js')
  
    block body\_end

include 是載入其他模組化的 pug，像是 header 與 footer，而 block head\_end、content、body\_end 是預先把洞挖好來讓其他 pug 可以寫入這個地方。

extends \_layouts/temp

block content
  .uk-container
    h1.uk-text-center.heading-medium 這是首頁

extends 是衍伸 temp.pug 這個模板，而 block content 就是剛剛在 temp.pug 裡面定義好的一個區塊，寫在這邊的 pug 就會替換 temp.pug 裡面 block content 的部分。

隨著專案的進行我會把 pug 細分成很多獨立的檔案，拆分出來的 .pug 就可以很方便的整理進 ODS 裡面，或是需要之前寫過的元件的時候，也能從 ODS 拿出來立即使用。

### **Sass**

![](https://oberonlai.blog/wp-content/uploads/2020/04/2000px-Sass_Logo_Color.svg_-1024x768.png)

Sass 是一套 CSS 的擴充語法，它可以讓 CSS 擁有程式語言常見的功能，像是定義變數、迴圈、判斷式等等，根據寫法的不同又分為 Sass 與 Scss，Sass 跟 Pug 比較類似，可以少寫很多括號跟分號，而 Scss 乍看之下比較像傳統的 CSS，然後多了跟 Sass 一樣的功能。

因為閱讀上的習慣，我自己是寫 Scss 比較多，而 UIkit 的原始碼也是用 Scss 寫的，所以比較方便的 Sass 反而比較少用到。

前面提到 UIKit 的眾多元件，除了省下很多的開發時間外，讓我覺得最大的收穫是如何組織 CSS 檔案。最早在寫 CSS 的時候，每個頁面的每個元件有各自獨立的 class 名稱，雖然看似很有組織，但實際在運作時會發現很多頁面都會有重複的元件。

後來採取了 [BEM](http://getbem.com/introduction/) 這個 CSS 組織方法，也就是依照 Block、Element、Modifier 這樣的邏輯來命名，一開始用的時候覺得好有系統，而且要回頭再改的時候也很方便找。

![](https://oberonlai.blog/wp-content/uploads/2020/04/github_captions.jpg)

但幾個專案下來之後我發現，在寫 HTML 命名 class 的時候，因爲要想這個元件到底是 Block 還是 Element，或是 Modifier 要怎麼下，寫到後來覺得很煩，因為很不直覺，必須要思考 HTML 的規則。

在這個時期，我事實上是很討厭使用像是 Bootstrap 或是 UIKit 這一類的 Framework，因為我覺得他們很肥，而且要寫一個帶有圓角、陰影的按鈕要塞一堆 class 在 HTML 裡面，整個看起來超髒超亂。

但人總是會變的，現在的我完全回不去以前自己命名 class 的寫法了，轉而擁抱這種當初自己嫌得要命的方式，最大的關鍵在於它讓我可以很直覺得像是在寫 CSS 一樣寫 HTML。

後來才知道這樣的方法叫做 Functional CSS，也就是把每個 CSS 屬性都變成一個獨立的 class，一個 class 只管一件事，當我要設定 font-size 為 12px 就是 .text-xsmall，背景要白色就是 .background-default。

![](https://oberonlai.blog/wp-content/uploads/2020/04/CleanShot-2020-04-25-at-09.49.42.jpg)

一個 h1 可能就帶有四個 class，如果考慮到 RWD 的狀態可能還會更多，但因為這些 class 我知道它們的意義是什麼，所以在寫的時候可以立馬翻譯成 CSS，完全不用思考 class 到底該如何命名。

UIKit 本身就提供許多現成的 class 可以使用，所以以前還不熟的時候沒事就是去逛他們的文件，把每個元件該怎麼用、class 有哪些一遍又一遍的重複看，再搭配實作幾個專案下來，差不多已經記得大部分常用的寫法。

在 ODS 的 src/sass 目錄中，主要分為 components、helpers、partials、vendors 這四個資料夾，以下說明這四個資料夾的用途：

**src/scss/componets** - 放覆寫 UIKit 元件的 Scss 檔，可能會有 heading.scss、button.scss、text.scss 等等，所以檔案名稱就是對應 UIKit 的元件名稱，這樣找起來很方便

**src/scss/helpers** - 放一些讓自己寫 Scss 好用的工具，以及管理所有的變數

**src/scss/partials** - 放頁面區塊為主的 Scss 檔案為放在這邊，像是 header、footer 以及 UIKit 沒有需要自行定義的元件

**src/scss/vendors** - 放第三方的元件的 Scss 檔，像是 WordPress 本身的 Style 或是控制 Facebook Social Plugin 的樣式

新專案我的設計步驟如下：

**一、整理變數**

首先開啟 src/scss/helpers/variable.scss，裡面是 UIKit 大部分會常用到的變數，所以如果你很幸運遇到有組織邏輯的設計師，這些變數可以很快的就整理出來。

$global-small-margin: 10px !default;
$global-margin: 20px !default;
$global-medium-margin: 40px !default;
$global-large-margin: 70px !default;

$global-color: #000 !default;
$global-muted-background: #EDEDED !default;
$global-primary-background: #fc9202 !default;
$global-muted-color: #999 !default;
$global-background: #fff !default;
$global-secondary-background: #ffe400 !default;
$global-success-background: #32d296 !default;
$global-warning-background: #faa05a !default;
$global-danger-background: #f0506e !default;

$global-link-color: #1e87f0 !default;
$global-link-hover-color: #0f6ecd !default;

$global-small-gutter: 15px !default;
$global-gutter: 30px !default;
$global-medium-gutter: 40px !default;

$global-font-family: 'Noto Sans TC','PingFang TC','微軟正黑體',Arial, sans-serif;

$breakpoint-small: 640px !default;
$breakpoint-medium: 960px !default;
$breakpoint-large: 1200px !default;
$breakpoint-xlarge: 1600px !default;

**二、選擇要引入的 Scss**

打開 src/scss/style.scss，這支檔案是控制所有要引入的 Scss 檔，UIKit 有固定的載入順序，所以儘可能的別去變動它，裡面有所有元件的 Scss 檔案，先確認該專案會用到哪些元件，把要用到的取消註解，算是減少一些不必要的 style。

// scss
// @import url('https://fonts.googleapis.com/css?family=Noto+Serif+TC:300,400,500,700,900&display=swap&subset=chinese-traditional');
@import url("https://fonts.googleapis.com/earlyaccess/notosanstc.css");

@import "helpers/mixin";
@import "helpers/variable";
@import "helpers/reset";

@import "../../node\_modules/uikit/src/scss/variables-theme.scss";
@import "../../node\_modules/uikit/src/scss/mixins.scss";

// Elements
@import "../../node\_modules/uikit/src/scss/components/link.scss";
@import "../../node\_modules/uikit/src/scss/components/heading.scss";
// @import "../../node\_modules/uikit/src/scss/components/divider.scss";
@import "../../node\_modules/uikit/src/scss/components/list.scss";
@import "../../node\_modules/uikit/src/scss/components/description-list.scss";
// @import "../../node\_modules/uikit/src/scss/components/table.scss";
@import "../../node\_modules/uikit/src/scss/components/icon.scss";
// @import "../../node\_modules/uikit/src/scss/components/form-range.scss";
@import "../../node\_modules/uikit/src/scss/components/form.scss"; // After: Icon, Form Range
@import "../../node\_modules/uikit/src/scss/components/button.scss";

// Layout
@import "../../node\_modules/uikit/src/scss/components/section.scss";
@import "../../node\_modules/uikit/src/scss/components/container.scss";
@import "../../node\_modules/uikit/src/scss/components/grid.scss";
// @import "../../node\_modules/uikit/src/scss/components/tile.scss";
@import "../../node\_modules/uikit/src/scss/components/card.scss";

...

**三、根據實際寫的狀態加入 Scss**

假設當今天在用 UIKit 的 Accordion 的時候，發現樣式需要調整，這時後就新增一個 accordion.scss 並放在 /src/scss/componets 的目錄下，然後在 style.scss 最下面寫 @import "components/accordion" 來引入覆寫的檔案，這樣之後要修改 Accordion 的樣式時找起來就非常方便。

雖然 UIKit 用起來真的十分方便，但即使有經過引入檔案的控管，編譯完之後 CSS 檔案至少都還是會落在 100 kb 出頭左右，相較於最近很紅的 [Tailwindcss](https://tailwindcss.com/docs/controlling-file-size/#app) 以及其他 CSS framework 比起來肥了好幾倍。

![](https://oberonlai.blog/wp-content/uploads/2020/04/CleanShot-2020-04-25-at-11.25.09.jpg)

主因還是在 UIKit 包含了很多現成的元件方便開發者使用，這在開發上可以省去很多 JS 的撰寫時間，但 Tailwindcss 近期也準備推出 UI components 了，也許之後會跳槽到 Tailwindcss 也說不定XD

### **ES6**

自從經過上一個專案的洗禮之後，對於 ES6 的模組化開發稍微比較有概念了，所以在之後的專案上，有寫到 JS 的部分就會開始採用新版的語法，因為不管是檔案引入或是相容性 Parcel 都幫忙把環境搞定了。

JS 的資料夾目錄跟 Scss 一樣，也是分為 components、helpers、partials，少了 vendors 是因為用 import 就可以直接把第三方工具引入。

ODS Starter Theme 裡面預設的就只有 src/heleprs/Variable.js，主要是用來定義所有的 JS 變數，然後透過 script.js 的引入，可以讓實際運作的程式比較不會有落落長的 Selector 而妨礙閱讀。

import UIkit from 'uikit';
import Icons from 'uikit/dist/js/uikit-icons';
UIkit.use(Icons);

import { Variable } from "./helpers/Variable"; 
const vars = new Variable();

document.addEventListener('DOMContentLoaded',()=>{
  vars.logo.classList.add('logo-class');
})

可以看到 UIKit 跟它的 icon 在最前面被引入，這樣如果需要使用 UIkit 的 API 就可以直接拿來用。接下來是引入變數檔案，並且宣告物件 vars 來拿變數，像是 vars.logo 這樣。

其他像是 ES6 新增的 const、let、箭頭函式、類別都可以直接使用，打包的時候 Parcel 都會幫忙編譯為 ES5 的版本以支援較舊的瀏覽器。

### **打包前端資源**

前端開發完成後，執行 npm run build 就可以把 CSS、JS、圖片檔打包到 assets 資料夾裡面，可以打開 themes/ods/functions.php 這支檔案，看到以下的內容：

<?php
if ( ! defined( 'ABSPATH' ) ) { exit;}

function ods\_scripts() {
if ( ! is\_admin() ) {
wp\_enqueue\_script( 'ods\_js', get\_template\_directory\_uri() . '/assets/js/script.js','','',true ); // name,path,jq,version,footer
wp\_enqueue\_style( 'ods\_style', get\_template\_directory\_uri() . '/assets/css/style.css', array(), '', 'all' );
}
}
add\_action( 'wp\_enqueue\_scripts', 'ods\_scripts' );// 載入全站 js/css

remove\_action('wp\_head', 'feed\_links\_extra', 3);
remove\_action('wp\_head', 'feed\_links', 2);
remove\_action('wp\_head', 'rsd\_link');
remove\_action('wp\_head', 'wlwmanifest\_link'); 
remove\_action('wp\_head', 'index\_rel\_link');
remove\_action('wp\_head', 'parent\_post\_rel\_link', 10, 0);
remove\_action('wp\_head', 'start\_post\_rel\_link', 10, 0);
remove\_action('wp\_head', 'adjacent\_posts\_rel\_link', 10, 0);
remove\_action('wp\_head', 'wp\_generator');
remove\_action('wp\_head', 'adjacent\_posts\_rel\_link\_wp\_head', 10, 0);
remove\_action('wp\_head', 'rel\_canonical');
remove\_action('wp\_head', 'wp\_shortlink\_wp\_head', 10, 0);
remove\_action('wp\_head', 'print\_emoji\_detection\_script', 7 );
remove\_action('wp\_print\_styles', 'print\_emoji\_styles' );
remove\_action('welcome\_panel', 'wp\_welcome\_panel');
remove\_filter('the\_excerpt', 'wpautop');
add\_filter('show\_admin\_bar','\_\_return\_false');
// add\_filter('use\_block\_editor\_for\_post', '\_\_return\_false', 10); // 啟用 or 關閉 Gutenberg

在 wp\_enqueure\_srcipts 的路徑就是 assets 的檔案，這樣就可以開始進行 WordPress PHP 的撰寫了，最後一行的 Hook Filter 在於控制是否要停用 Gutenberg 編輯器，這就看專案性質決定了。

## 小結

透過 ODS Starter Theme 可以把佈景主題的檔案縮減到只留下必要的內容，然後再從專案的實際過程中去加入需要的範本，並從 ODS 去複製相對應的元件或 PHP 來開發 Theme。

另一方面整合各種前端工具來加速頁面切版的工作，透過 Parcel 來提升編譯的速度 ( 或是換 MBP 16 inch )，這就是目前我每個專案的工作流程，也是多年來踩過無數雷、試過一堆工具的血淚史。

也許未來還會有新的工具出來取代現在的作法，同時間我也還在實驗用 Plugin 寫 PHP 類別來整理各種 WP\_Query 以及做資料邏輯處理的組織方式，這樣 Theme 就真的單純是管理 View 而不會去干涉到資料，等有心得再分享出來～

總之這十年來在工作上越深入越覺得自己懂得東西好少，但又有一種信心，只要給我時間我就有辦法找到解決方案並真正的解決它，我想這就是為什麼寫程式會成為我每天的樂趣 ( 痛苦 ) 吧 XD