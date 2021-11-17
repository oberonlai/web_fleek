---
title: WordPress LINE 登入外掛
date: 2021-05-05 16:10:00
categories:
- LINE
- 外掛開發
tags:
- 會員登入
- WordPress 會員外掛
etoc: false
---

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-00.jpg)

## 外掛介紹

LINE 即時通訊軟體在台灣有超過兩千萬的用戶，持續推出相關的生活應用，從遊戲到新聞、叫車到支付，已經大範圍的滲透到台灣人的日常生活，因此幫自己的電商網站增加 LINE 帳號登入功能，能大幅度減少註冊網站會員的門檻，進而增加購物轉換率，而 WordPress LINE 登入外掛能協助您自行串接，無需再花費技術人員的成本。

<!--more-->

## 主要功能

本外掛適用於由 WordPress 內容管理系統建置的網站，如搭配購物車外掛 WooCommerce 能得到更好的支援，主要功能介紹如下：

### 1. 整合 LINE 第三方登入

在 WooCommerce 帳號登入頁面中自動增加 LINE 登入按鈕，點擊後會引導至 LINE 的登入介面來完成會員註冊 or 登入的動作。你可以自行選擇是否要在帳號登入頁顯示 LINE 登入按鈕，以及按鈕位置的設定，範例頁面：https://demo.lineforwp.com/my-account/

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-21.jpg)

可以前往該網址進行設定測試：https://demo.lineforwp.com/wp-admin/admin.php?page=line-for-wp

登入帳密為 demo/demo

### 2.自訂整合按鈕是否顯示與設定尺寸

在 WooCommerce 登入頁與結帳頁中的 LINE 登入按鈕，皆可設定顯示位置於表單上方或下方，並可自訂按鈕尺寸為滿版、大、中、小等四種尺寸，可搭配各種佈景主題的版面設計來決定登入按鈕的顯示樣式。

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-22.jpg)

### 3. 自訂登入後的跳轉方式

可以在頁面或文章中加入按鈕後放上由本外掛產生的 LINE 登入網址來進行跳轉，或是在網址中加入參數來設定自動跳轉，就能讓瀏覽者在進入網站前就先完成 LINE 登入的動作，以利後續的操作。

只要在網址中帶入參數 lgmode，即可輕易的選擇跳轉模式，各模式說明如下：
- lgmode=true - 如果設定為 true，當使用者點選該網址時會直接跳轉到 LINE 的登入頁面，待登入完成後會自動跳轉回該頁面，並且顯示為已登入狀態。

>範例：當設定連結網址為 https://example.com/landing?lgmode=true 時，會引導使用者跳轉至 LINE 登入，登入完成後跳轉回頁面 https://example.com/landing ，適合要做商品導購或是表單填寫時可預先帶入會員資料的使用情境。

- lgmode=https://example.com/lading - 帶入登入後的跳轉網址，可以指定使用者登入後跳轉的頁面。

>範例：當設定連結網址為 https://example.com/?lgmode=https://example.com/landing 時，完成登入後頁面會跳轉至 https://example.com/landing

### 4. 在區塊編輯器中自訂 LINE 登入按鈕

WordPress 的區塊編輯器提供許多豐富的小工具來更方便的設計頁面內容，如果你的銷售頁面是使用區塊編輯器的頁面來製作，透過本外掛可以快速增加 LINE 登入按鈕進而降低註冊會員的門檻。

只要在編輯頁面中搜尋 line 或是輸入斜線 line 即可找到 LINE 登入按鈕：

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-17.jpg)

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-16.jpg)

新增 LINE 登入後可透過右方的設定項來進行設定，可調整的範圍有按鈕文案、尺寸以及登入後的頁面跳轉，以便更彈性的根據銷售頁面的文案內容來調整登入按鈕：

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-18.jpg)

如果是在會員已登入的狀態下，頁面會不顯示該按鈕。可以前往該網址進行設定測試：https://demo.lineforwp.com/wp-admin/post.php?post=23&action=edit

登入帳密為 demo/demo

### 5. 使用短代碼自訂 LINE 登入按鈕

如果網站使用其他的頁面編輯器，像是 Elementor、Divi，也可以用短代碼 ( Shortcode ) 來放便置入 LINE 的登入按鈕，寫法為：

```php
[linelogin text="快速登入" size="m" lgmode="true"]
```

段代碼名稱為 linelogin，設定參數說明如下：

- text - 按鈕的文字名稱
- size - 按鈕的尺寸，可選參數有 f、 l、m、s 四種尺寸，f 為滿版按鈕
- lgmode - 跳轉的模式，設定 true 時會跳轉回原頁面，設定網址時會跳轉至該網頁
- align - 對齊方式，可選參數有 left、center、right


如果是在會員已登入的狀態下，頁面會不顯示該按鈕。可以前往該網址進行設定測試：https://demo.lineforwp.com/wp-admin/post.php?post=23&action=edit

登入帳密為 demo/demo






## 外掛基本設定

LINE 的開發者帳號非常容易申請，只要有 LINE 帳號即可，申請的步驟很單純，只要提供相關的資訊即可建立完成。

### 前往 LINE 開發者帳號並新增 Channel

一共有四個步驟，關鍵是要取得最後產出的 Channel ID 與 Channel secret，這兩個資訊需要填入到本外掛的設定選項中，以下就各步驟進行說明：

#### **STEP1. 使用個人 LINE 帳號登入**

先前往 https://developers.line.biz 點擊右上角的 Log in 按鈕

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-01.jpg)

選擇使用LINE帳號登入

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-02.jpg)

登入後會被要求輸入開發者相關資訊，需要填入開發者名稱與電子郵件，輸入完成後點選 Create my account 即可。

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-03.jpg)

這樣 LINE 開發者帳號就申請完成了，接下來是新增 Provider。

&nbsp;

#### **STEP2. 新增 Provider**

Provider 指的是內容供應商，點選畫面中 Create a new provider 的按鈕：

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-04.jpg)

輸入 Provider name 之後點選 create

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-05.jpg)

這樣就完成了 Provider 的新增。

&nbsp;

#### **STEP3. 新增 Channel 與設定 Callback URL**

Channel 指的是我們要使用的應用程式，它有很多種類型，由於我們是要使用登入的功能，所以選擇 Create a LINE Login channel：

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-06.jpg)

接下來輸入這個 Channel 的相關資訊，要注意的地方是 Region，記得要填 Taiwan，萬一填錯的話是無法修改的，必須砍掉 Channel 再重新建立一個：

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-07.jpg)

都填寫完畢後點擊最下方的 Create，這樣就完成 Channel 的建立～

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-08.jpg)

接下來回到 Channel 的主畫面，我們要設定 Callback URL，這個 URL 指的是當使用者登入完成後，會跳轉到我們網站的頁面，而這個頁面會去取得該使用者的資料，後續本外掛會將這些資料與 WordPress 資料庫做整合。

先進入 LINE Login 的頁面，然後點選 Callback URL 的 Edit 按鈕，就會出現輸入框輸入網址：

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-09.jpg)

接下來停留在這個畫面，新開一個瀏覽器頁籤，登入 WordPress 網站後台，在左邊選單 LINE > 設定裡面，可以找到這個 Callback URL，將它複製後再貼回 LINE 的後台即可。

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-09-1.jpg)

&nbsp;

#### **STEP4. 請求 OpenID 權限與發佈 Channel**

完成 Channel 設定後，最後要處理的是請求使用者提供電子郵件的權限，因為在 WordPress 這端我們是用電子郵件來判斷會員的身份，因此取得電子郵件是必要的，而電子郵件在預設的情況下 LINE 是沒有提供的，所以需要 OpenID Connect 來取得權限。

先回到 LINE Channel Basic Settings 的頁籤，捲到頁面底端可以看到 OpenID Connect 的選項，點選 Apply：

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-10.jpg)

勾選同意 LINE 的隱私權政策以及上傳電子郵件使用用途的截圖後，點選 submit 即申請完成：

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-11.jpg)

最後找到 Channel 上方的發佈按鈕，將狀態改為 Publish 即完成 LINE Channel 的申請流程。

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-12.jpg)

### 安裝外掛輸入 LINE 資訊

完成 LINE Channel 的申請之後，要先將兩段文字複製起來，一個是 Channel ID，另一個是 Channel secret，可以在 Channel 的主畫面中找到：

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-13.jpg)

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-14.jpg)

複製後回到網站後台，將 Channel ID 與 Channel Secret 資訊貼入後儲存即可完成設定。

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-15.jpg)

可以前往該網址進行設定測試：https://demo.lineforwp.com/wp-admin/admin.php?page=line-for-wp

登入帳密為 demo/demo


## FAQ

### Q: 如果登入者不提供電子郵件會發生什麼事？

本外掛會判斷是否有取得電子郵件，如果沒有的話會引導至 WooCommerce 帳號登入頁面並顯示 LINE 登入失敗，提示使用者需要允許存許電子郵件才能完成登入。

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-login-plugin/wordpress-line-login-plugin-20.jpg)

### Q: 如果網站已經擁有該會員資料會發生什麼事？

取得電子郵件後本外掛會先做會員資料庫比對，如果發現該電子郵件已存在於資料庫當中，則會更新該會員的顯示名稱以及大頭照，反之則會取得該使用者資料後註冊一個新會員。

### Q: 一定要有 LINE 的開發者帳號才能使用本外掛嗎？

LINE 開者帳號是用來申請 LINE Channel 用的，因此建議要申請才能取得該 Channel 的管理權限。

### Q: 一定要有 LINE 的官方帳號才能使用本外掛嗎？

不用，只要有 LINE 的開發者帳號即可。

### Q: 請問有測試網站可以實際操作看看嗎？

可前往該網址進行設定測試：https://demo.lineforwp.com/line-login-test/

## 取得 WordPress LINE 登入外掛

需要 WordPress LINE 登入外掛嗎？[購買連結](https://lineforwp.com/product/wordpress-line-login/)

有任何問題歡迎 [與我聯絡](mailto:m615926@gmail.com)～

&nbsp;
