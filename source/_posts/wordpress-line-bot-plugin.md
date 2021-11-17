---
title: WordPress LINE Bot 聊天機器人外掛
date: 2021-08-14 12:20:00
categories:
- LINE
- 外掛開發
tags:
- Bot
- 聊天機器人
etoc: false
---

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-bot-plugin/wordpress-line-bot-plugin-00.jpg)

## 外掛介紹

LINE 聊天機器人的一大功能是可以在特定情境下自動發送多樣化的訊息到店家的官方帳號，只要透過 LINE Messaging API 就能根據店家網站的行為自動發送訊息，像是訂單未完成的提醒、ATM 付款轉帳帳號資訊、超商取貨的到貨通知，這些都可以透過店家的 LINE 官方帳號來提醒顧客。

WordPress LINE Bot 聊天機器人外掛整合了 LINE Messaging API 與 WooCommerce，能直接在 WordPress 後台設定機器人訊息的觸發條件以及通知內文，並可帶入站內的會員以及訂單資訊提供更個人化的訊息推播。

<!--more-->

## 主要功能

本外掛適用於由 WordPress 內容管理系統建置的網站，並搭配購物車外掛 WooCommerce，主要功能介紹如下：

### 1. 根據訂單狀態發送訊息

WooCommerce 內建多種訂單狀態，如果想要在訂單建立付款完成後發送 LINE 提醒，可以在觸發規則中將條件設為「當訂單狀態為已完成時」觸發，或是可以根據付款方式，像是針對 ATM 付款的顧客來發送轉帳帳號與金額、針對超商付款顧客發送繳費代碼，另外也能在訂單已出貨時提醒顧客取貨。

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-bot-plugin/wordpress-line-bot-plugin-05.jpg)

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-bot-plugin/wordpress-line-bot-plugin-06.jpg)

### 2. 支援 LINE 訊息格式

支援文字以及圖片，可自訂訊息內文、上傳圖片，除了透過文字來傳達訊息外，亦可使用圖片來設計更多樣化的提示內容。

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-bot-plugin/wordpress-line-bot-plugin-07.jpg)

### 3. 訊息內文帶入個人資訊

想要讓訊息更貼近顧客嗎？透過文字訊息編輯器，可自動將會員資訊帶入內文中，像是會員姓名、訂單編號、購買商品等，讓你的提示訊息可以更精準的與顧客傳達。

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-bot-plugin/wordpress-line-bot-plugin-08.jpg)


## LINE Messaging API 計價方式

WordPress LINE Bot 聊天機器人外掛主要使用的是 Messaging Push API，而這 API 官方是會根據每月推送的則數來進行收費，計價模式有三種，免費方案每月可以推播 500 則訊息，中用量月費 800 元可推送 4,000 則，高用量月費 4000 元可推送 25,000 則。

更多的方案可以參考 LINE 的官方說明頁面：https://tw.linebiz.com/column/budget-auto-count/


##  LINE Messaging API 申請與建立

LINE 的開發者帳號非常容易申請，只要有 LINE 帳號即可，申請的步驟很單純，只要提供相關的資訊即可建立完成。建


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

#### **STEP3. 新增 Messaging API**

進入剛剛新增的 Provider，點選 Create a Messaging API channel：

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-bot-plugin/wordpress-line-bot-plugin-01.jpg)

輸入 Channel name 與 Channel description，Channel name 會跟 LINE 官方帳號做連動，如果已經有申請 LINE 官方帳號的話，這邊輸入帳號名稱即可：

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-bot-plugin/wordpress-line-bot-plugin-02.jpg)

下方依序輸入相關資訊欄位，輸入完成後點擊 create 即可。


#### **STEP4. 取得 Messaging API 相關資訊**

完成 LINE Messaging API 的建立之後，要先將兩段文字複製起來，一個是 Channel ID，另一個是 Channel secret，可以在 Channel 的主畫面中找到：

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-bot-plugin/wordpress-line-bot-plugin-03.jpg)

![](https://oberonlai.blog/wp-content/uploads/wordpress-line-bot-plugin/wordpress-line-bot-plugin-04.jpg)

複製後回到網站後台，將 Channel ID 與 Channel Secret 資訊貼入，以及輸入 LINE 的官方帳號 ID 儲存即可完成設定。

## FAQ

### Q: 我的顧客如果沒有加入我的 LINE 官方帳號他還能收到推播嗎？

不行，必須要讓顧客加入官方帳號才能收到推播。

### Q: 我的網站會員需要使用 LINE 登入才能收到推播嗎？

不一定，前提是只要能夠取得顧客的 LINE user id 並存入會員資料庫即可，建議是搭配本站開發的 [WordPress LINE 登入外掛](https://oberonlai.blog/tw/wordpress-line-login-plugin/)會有最佳的相容性。



## 取得 WordPress LINE Bot 聊天機器人外掛

需要 WordPress LINE Bot 聊天機器人外掛嗎？[請與我聯絡](mailto:m615926@gmail.com)～

&nbsp;
