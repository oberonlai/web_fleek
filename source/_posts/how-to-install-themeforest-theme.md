---
title: WordPress主題購買後如何安裝成像展示的一樣？
tags: []
id: '1200'
categories:
  - - WordPress
date: 2013-12-03 19:11:39
---

曾經有購買過 wordpress 主題的朋友們都知道，花錢買了一套主題就是看上它展示的範例，無論是精美炫麗的 slider 效果，還是各種已設計完成的頁面配置，甚至是聯絡我們、地圖顯示等進階功能，都是吸引我們購買的原因。   但如果是第一次使用 wordpress 的朋友，可能會發現到買回來的檔案有許多的資料夾，到底那個是可以安裝、那個是說明書，對於 wordpress 主題比較不熟悉的朋友可能會有些困惑，因此整理這篇文章簡單說明 wordpress 付費主題商品的相關內容，以及該如何安裝成像它展示的 demo 網站一樣漂亮。(本篇以 [Themeforest](http://themeforest.net/?ref=oberonlai "Themeforest") 主題商品為例，如果不了解什麼是 Themeforest 可以參考[這篇文章](https://oberonlai.blog/wordpress/how-to-buy-wordpress-theme/ "WordPress主題選購指南")。)  

## 第一步：下載所有文件及檔案

  各位朋友於 Themeforest 網站上購買付費 wordpress 主題商品後，會進入以下的下載頁面，建議是選擇 All Files & documentation 進行下載，通常這份檔案會包含 wordpress 主題壓縮檔、外掛程式、範例資料，以及主題的使用說明書。   ![how-to-install-themeforest-theme-img1](https://oberonlai.blog/wp-content/uploads/2013/11/how-to-install-themeforest-theme-img1.jpg)
<!-- more -->
下載完成後，將檔案解壓縮，通常可以看到如下的資料夾內容：   ![how-to-install-themeforest-theme-img2](https://oberonlai.blog/wp-content/uploads/2013/12/how-to-install-themeforest-theme-img2.jpg)   **dt-pressocre\_v.2.0.1.zip** : 佈景主題安裝檔，直接可從 wordpress 後臺上傳後進行安裝，或是可將其解壓縮後上傳到 wp-content/theme 的目錄中，再由後臺啟用即可。 **Dummy** : 展示網站中的網頁資料，通常為一份 xml 的檔案格式，可以由 wordpress 後臺進行匯入，稍後將說明如何匯入。 **Licensing** : 此商品的版權與使用限制說明。 **PSDs** : 一般設計師在設計介面時，都會採用 [Adobe Photoshop](http://www.adobe.com/tw/products/photoshop.html) 進行創作，而 psd 檔就是 photoshop 的檔案格式。如果購買回來的主題需要進行較大幅度的畫面修改，可聘請專業的網頁設計師來處理，這時就可以把 psd 檔交由他們設計，這樣就能以現行的網頁架構進行調整，縮短實際製作上的時間。 **User Guide** : 佈景主題的英文使用說明書，通常都會詳述主題該如何安裝、頁面如何修改、功能如何使用等等。  

## 第二步：安裝主題及外掛

  確認過解壓縮的內容有 .zip 的佈景主題安裝檔後，我們就可以進入 wordpress 後臺進行安裝，安裝方式與一般佈景主題一樣， 登入 wordpress 後臺 > 外觀 > 佈景主題 > 安裝佈景主題 > 上傳   ![how-to-install-themeforest-theme-img3](https://oberonlai.blog/wp-content/uploads/2013/12/how-to-install-themeforest-theme-img3.jpg)   上傳完成後並啟用該佈景主題，基本上就已經完成一半了，接下來要安裝的是這主題有使用到的外掛，我購買的這套有自動安裝的功能，只要勾選確定安裝就能完成。   ![how-to-install-themeforest-theme-img4](https://oberonlai.blog/wp-content/uploads/2013/12/how-to-install-themeforest-theme-img4.jpg)     如果購買的主題沒有這樣方便的功能，通常在第一步解壓縮檔案的時候還會多一個 plugins 的資料夾，讓我們可以手動把這些外掛用 ftp 的方式上傳到 wp-content/plugins 的目錄下。  

## 第三步：匯入資料

  接下來我們要把範例網站的資料匯入，讓我們的網站也能像他們展示的網站有一樣的效果，這樣修改起來就會方便許多。   ![how-to-install-themeforest-theme-img5](https://oberonlai.blog/wp-content/uploads/2013/12/how-to-install-themeforest-theme-img5.jpg) 進入 wordpress 後臺，點選左方選單 工具 > 匯入，進入頁面後點選 wordpress 安裝匯入程式。   ![how-to-install-themeforest-theme-img6](https://oberonlai.blog/wp-content/uploads/2013/12/how-to-install-themeforest-theme-img6.jpg) 點選立刻安裝。   ![how-to-install-themeforest-theme-img7](https://oberonlai.blog/wp-content/uploads/2013/12/how-to-install-themeforest-theme-img7.jpg) 安裝完成後點選「啟用外掛與執行匯入程式」。   ![how-to-install-themeforest-theme-img8](https://oberonlai.blog/wp-content/uploads/2013/12/how-to-install-themeforest-theme-img8.jpg) 接下來把我們解壓縮後 dummy 資料夾裡的 xml 檔案上傳，選擇好檔案位置後點選「上傳檔案並匯入」。   ![how-to-install-themeforest-theme-img9](https://oberonlai.blog/wp-content/uploads/2013/12/how-to-install-themeforest-theme-img9.jpg) 這步驟是要選擇所有匯入文章與頁面的作者是誰， 可以選擇為系統管理員 admin 即可，下方的 Import Attachments 是匯入所有附件檔案，建議可以不用勾選以省下匯入時間，確定後點選「Submit」。   ![how-to-install-themeforest-theme-img10](https://oberonlai.blog/wp-content/uploads/2013/12/how-to-install-themeforest-theme-img10.jpg) 成功匯入後會出現一堆 Failed to  import 媒體的訊息，這是因為我們剛剛沒有勾選匯入附件的原因。   ![how-to-install-themeforest-theme-img11](https://oberonlai.blog/wp-content/uploads/2013/12/how-to-install-themeforest-theme-img11.jpg) 接下來再到頁面或文章列表，就可以看到所有的範例頁面，這時我們就可以比對範例去修改成我們要的內容文字。   ![how-to-install-themeforest-theme-img12](https://oberonlai.blog/wp-content/uploads/2013/12/how-to-install-themeforest-theme-img12.jpg) 最後再到 設定 > 閱讀，選擇首頁要顯示的頁面即可。  

## 小結

  到 Themeforest 購買 wordpress 主題商品後記得選擇下載所有文件跟檔案，並將檔案解壓縮後把 .zip 安裝檔於 wordpress 後臺進行上傳，成功完成安裝並啟用後，依照指示再安裝相關的外掛功能，如無提示則自行由 FTP 上傳安裝，最後再將壓縮檔內 xml 檔進行匯入，並設定首頁顯示頁面，即可將範例網站的內容成功建立在自己的網站中。  

## 還找不到適合自己企業風格的主題嗎？

  Themeforest 商品眾多，且全數都是英文，往往很容易忽略中文的排版是否能正確顯示。另一方面該 wordpress 主題是否適合我們的企業風格，只看展示頁面與實際狀況仍存有很大差異，又不可能每一套都買來試裝看看，因此我們開發了一套解決方案。   EveryTheme 是一套快速尋找 WordPress 主題的產品，除了可以依產業別、色系來搜尋 Themeforest 的商品外，更可以透過影像合成技術，將企業標誌與主題展示畫面進行合成，讓我們在挑選 WordPress 主題時更能想像最終完成的效果。想了解更多關於 EveryTheme 可以參考這一篇文章--->[三十秒, 看見你的理想網站](https://oberonlai.blog/everytheme-intro/ "編輯「三十秒, 看見你的理想網站」")，或 [試試 EveryTheme](http://everytheme.com.tw)