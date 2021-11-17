---
title: 從新學習HTML5 (一) 概觀
tags: []
id: '186'
categories:
  - - HTML5
date: 2019-03-11 22:04:24
---

![](https://i2.wp.com/oberonlai.blog/wp-content/uploads/2019/03/IMG_7996.jpg)

![](https://i1.wp.com/oberonlai.blog/wp-content/uploads/2019/03/CleanShot-2019-03-11-at-21.54.26.jpg?ssl=1)

第一次完整學習 HTML 是在剛入行的時候，算一算也有十幾個年頭了，這些日子以來，對於 HTML5 只有著片段的知識，每當卡關去爬 Stackoverflow 時，才會驚訝的發現原來還有這些新東西可以用，所以在圖書館看到這本時，應該是老天爺叫我要好好的來重新認識 HTML 了。

看完幾個章節以後，有一種在看說明書的感覺。就像電器用品一樣，不用看說明書就可以用得順手，然後哪天在整理東西時發現冷氣機的說明書，隨手翻了一下，才驚喜的發現原來家裡的冷氣除了會冷以外，還有除濕、暖氣、烘乾、洗衣、脫水、無線分享器、播音樂等各種功能(?)，而且有些還真的非常實用，在看這本 HTML5 的時候就是這種感覺，每翻一頁處處都是驚喜，許多新功能立馬能在工作上派上用場，滿滿的意外收穫～

但這本書實在太厚了，根本就是整個 W3C 文件的濃縮版，涵蓋的面向非常完整，從 HTML5 的歷史介紹、介面設計、通訊功能、影像資料處理，到本地資料儲存機制應有盡有，而且不是草草帶過，書裡介紹完整屬性並搭配實作範例，真的是 HTML5 參考書不為過。

怕自己沒辦法一次整理完這麼龐大的資料量，只好分篇來紀錄，本文以 HTML5 的入門概觀為主，重點摘要整理如下：

## HTML5 的歷史

記得學生時代用 Dreamweaver 設計網頁時，在檢視原始碼的模式下，有時候都會看到最上面有類似這一段的開頭：

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/2002/08/xhtml/xhtml1-strict.dtd">
```

落落長的內容裡面寫著 XHTML ，心想這應該就是 HTML 的標準形式吧，在還是 DW 的年代，完全不知道怎麼寫 HTML，只會用現成的介面來產出網頁。直到後來才知道，原來還有一種叫 XHTML 的東西準備要來取代 HTML...

HTML 最早是在西元 1990 年由網際網路發明人 Tim Berners-Lee 所制定的，並組成了 W3C (World Wide Web Consortium) 來維護 HTML，到了西元 1999 年進化到第四版，也就是 HTML5 的前身 HTML4.01，但由於 HTML4 本身架構以及語法的問題，遲遲無法進行改良，因此 W3C 決定以 XML 語言基礎，打掉重練 HTML 並開發 XHTML。

直到 2003 年有新技術出現，原有的 HTML4 可以升級了並進行擴充了，於是由 Apple、Mozilla、Opera 組成的 WHATWG 來制定下一版的 HTML，但 W3C 也已經花了不少時間在著手 XHTML 的開發，所以情勢即將變成各玩各的規範，苦的就是瀏覽器廠商以及廣大的網站開發者。

幸好在 2007 年，W3C 也加入了 WHATWG，一起參與下一代 HTML 的開發，雖然 W3C 並未放棄 XHTML 的計畫，但至少 HTML 可以繼續的被使用下去。到了 2010 年 HTML5 訂出草稿，最後在西元 2014 年 10/28 發表，雖然當時還是 Flash 橫著走的年代，直到 [Steven Jobs 力推 HTML5 並抵制 Flash](https://chinese.engadget.com/2010/04/29/steve-jobs-publishes-some-thoughts-on-flash-many-many-thou/)，HTML5 才逐漸發揚光大，成為現今主流的網頁技術。

這幾年各家瀏覽器對於 HTML5 的支援越來越完整，有興趣的朋友可以到 [https://html5test.com](https://html5test.com) 看看自己使用的瀏覽器的支援程度，個人覺得有點意外的是，macOS Safari 的支援度有點低，當年最早在制定 H5 的公司不就是 Apple 嗎？反倒是 Chrome 的支援度較完整。

![](https://oberonlai.blog/wp-content/uploads/2019/03/CleanShot-2019-03-09-at-21.55.24.png)

![](https://oberonlai.blog/wp-content/uploads/2019/03/CleanShot-2019-03-09-at-21.55.55.png)

## HTML5 的主要內容

廣義來說，HTML5 主要包含以下七大項的新增內容，當使用到其中一項內容時，我們便可以說該網頁是使用 HTML5 技術所開發的：

#### (ㄧ) 語意標籤 semantic

使用更具有意義的 HTML 標籤，讓搜尋引擎或是爬蟲機器人可以更方便與快速的了解網頁的內容，也讓開發人員有更ㄧ致的準則來使用適合內容的標籤。像是頁首使用 <header>、主要區塊使用 <section>、側邊欄使用 <aside> 等等。

#### (二) 離線儲存 offline&storage

透過快取技術、本地儲存與 Index DB，讓裝置在沒有網路的環境下，依舊可以瀏覽網站的內容，這對於裝置行動化的趨勢來說，是非常實用的功能。

#### (三) 裝置存取 Device

隨著手機出貨量超越桌上型個人電腦，手持裝置變成是瀏覽網站最主要的來源，HTML5 可以取得使用裝置硬體的權限，讓網站可以像原生 APP 一樣，做到許多原本無法做到的功能，像是透過瀏覽器啟用相機、取得手機地理位址等等，大大加強了網站的應用性。

#### (四) 連結性 Connectivity

HTML5 讓網站之間傳輸資料更即時，非常適合做即時通訊或手機推播等功能。

#### (五) 效能與整合性 Perfermence&Integrate

整合 XMLHttpRequest 2 非同步載入技術，以及 Web Workers 支援網頁多工執行的特性，讓動態網頁從伺服器端請求資料更即時，使用體驗也更好。

#### (六) 2/3D 圖形與特效 2/3D Graphic&Effects

以前 Flash 之所以能稱霸網路，就是因為透過 Action Script 可以開發出各種 2D 或 3D 的視覺動態呈現，為了補足這一塊，HTML5 開發了 Canvas、SVG、WebGL 讓設計人員可以在不需要安裝任何瀏覽器外掛的情況下，製作出具有高水準的影像或動畫。

#### (七) 多媒體 multimedia

Flash 最麻煩且最棘手的地方，就是在於他需要安裝瀏覽器套件才能使用，播放影音也不例外，所以 HTML5 直接內建影音播放功能，不需要任何的套件安裝就可以讓用戶可以看到內容。

## HTML 基礎知識

#### (一) 文件物件模型 Document Object Model Tree

網頁內容是由 DOM Tree 的節點組成，每個 html 的標籤都會轉換成一個物件，以巢狀的方式紀錄每個標籤的父子關係，參考下圖的對應關係，左邊是 HTML 的標籤內容，右邊是轉換後的文件物件模型。  

```
<!DOCTYPE html>
<html>
  <head>
    <title>網頁</title>
  </head>
  <body>
    <h1>文章標題</h1>
  </body>
</html>
```

```
DOCTYPE
html
├── head
│   └── title
├── body
│   └── h1


```

#### (二) HTML 根元素

HTML 跟元素是整份文件最上層的節點，擁有兩個屬性，一個是 manifest，專門設定來讀取網站快取清單的路徑，範例如下：

```
<html manifest="cache.manifest">
```

第二個是 lang，用來設定網頁所使用的語系：

```
<html lang="zh-tw">
```

#### (三) head 元素

head 元素主要放網頁描述資料的區塊，也就是中介資料，常用的有以下幾種：

<title> 網頁的標題，js 使用來取得網頁標題 document.title

<base> 設定網站的基準網址，讓需要取得路徑的地方寫相對路徑即可，範例如下：

```
<base href='https://abc.com'>
<a href='about.html'>Link</a>
// 點擊 Link 網址是 https://abc.com/about.html
```

<style> 放置 CSS 樣式的標籤

<link> 引入媒體檔案的標籤，通常放在外部的 CSS 檔或使用它來引入

```
<link rel="stylesheet" type="text/css" href="style.css">
```

<script> 引入外部 JavaScript 檔案的標籤，HTML 5 新增加了兩種讀取模式：

預設：DOM 跑到就執行，會等 js 載完才會繼續往下跑  
async：DOM 跑到就執行，不等 js 載完繼續往下跑  
defer：DOM 跑到不執行，等 DOM 全部載完 js 最後才執行  

<noscript> 不支援 JavaScript 情況下所顯示的文字

<meta> 設定頁面的描述資料，使用 http-equiv 可設定 http 回應標頭狀態，主要分為三種：

content-type 編碼宣告

```
<meta http-equiv="content-type" content="text/html" charest="UTF-8"> 

// H5 中可直接由 charset 取代
<meta charset="UTF-8">
```

default-style 預設樣式 - 指定預設要讀取哪一個 CSS 檔

```
<meta http-equiv="Default-Style" content="Blue Style" />
<link rel="alternate stylesheet" href="/css/green.css" type="text/css" title="Green" />  
<link rel="alternate stylesheet" href="/css/blue.css" type="text/css" title="Blue" />
```

refresh 重整頁面

```
// 隔 30 秒後跳轉到 redirect.html
<meta http-equiv="fresh" content="30;URL=redirect.html" />
```

## 實用新增標籤

前面提到 HTML5 新增的內容包含語意標籤、影音功能等等，因此也新增了不少新標籤來定義這些內容，這邊以三個面向進行分類，並挑選實用的標籤來進行說明：

#### (一) 影音相關

<video> 播放影片使用的標籤，可放入多個 source 來源，當瀏覽器不支援該影片格式時，可以向下相容

```
<video width="400" controls>
  <source src="mov_bbb.mp4" type="video/mp4">
  <source src="mov_bbb.ogg" type="video/ogg">
  你的瀏覽器不支援 HTML5 播放器
</video>
```

<audio> 播放音檔使用的標籤

#### (二) 文本相關

<mark> 文字重點強調

<progress> 顯示進度條

```
<progress value="22" max="100"></progress>
```

<meter> 顯示水平量表

```
<meter value="3" min="0" max="10">十分之三</meter>
```

<time> 顯示時間

<wbr> 讓過長的文字在指定的地方斷行

#### (三) 介面相關

<details><summary> 內建的 accordion 功能

```
<details>
  <summary>標題</summary>
  展開內容
</details>
```

<datalist> 讓 input text 可以依照使用者搜尋文字跳出相關建議詞

```
// input 的建議字使用 id 為 keyword 的 datalist 裡面的內容
<input list="keyword" />

// 定義相關字詞
<datalist id="keyword">
  <option value="Google">
  <option value="Yahoo">
  <option value="Bing">
</datalist>
```

## 新增全域屬性

除了新的標籤以外，也有許多新的屬性可以使用，有了這些屬性，可以讓標籤更靈活的運用，此外還能擴充更多自訂屬性：

contenteditable：可以讓任何標籤變成可輸入文字的 input text 狀態

```
<p id="msg" contenteditable>輸入內容</p>
document.getElementById('msg').innerHTML); // 取得輸入內容文字
```

data-\*：自訂資料屬性，讓標籤可以擴充自己定義的屬性

```
<div id="products" data-price=450 data-name="books" data-name-en="books-en"></div>
document.getElementById("products").dataset.price // 450
document.getElementById("products").dataset.name // books
document.getElementById("products").dataset.nameEn // books-en 用駝峰寫法
```

draggable/dropzone：標記可拖曳/置放標籤

hidden：隱藏標籤

spellcheck：拼字檢查

## 小結

整理完 HTML5 的由來以及新增的功能之後，接來深入介紹 HTML5 在介面設計上新增的標籤以及 CSS3 的各項功能。