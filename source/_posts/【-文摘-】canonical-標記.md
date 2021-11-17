---
title: 【 文摘 】Canonical 標記
tags: []
id: '477'
categories:
  - - SEO
date: 2019-12-25 11:17:17
---

原來 **Canonical 標記**是為了重複內容來集中網頁權重使用的，特別是在不能做 301 轉址的情況，像是同一種商品有五款顏色，每個顏色都有獨立網址但內容都重複，就能使用 Canonical 來指定主要網頁，常看到的作法還有電腦版與手機版網站，也都會用這個標記來指定需要集中權重的網頁，但文中提及 Canonical 不能互相指，想想也合理，如果彼此互相指定就分不清楚到底誰是主要網頁，只能統一指定一個主要網頁。

> 註：Google官方不保證他採用你所寫的canonical元素會被採用，但我們沒得選擇，這確實Google官方提出的解決方案，能告訴Google你有重複內容的問題，並且Google會盡可能處理。
> 
> 同時，使用上你要注意，避免有兩個網址互相用canonical指向，舉例來說，如果你在綠色毛衣的網頁上用canonical指向紅色毛衣，在紅色毛衣上canonical指向綠色毛衣，這樣Google不會知道你的標準網址到底是哪一個。正確做法應該如上方的圖片所示，在黃、紅、藍三個毛衣的頁面底下加入<canonical>標記，而綠色毛衣的網頁不使用canonical 標記（因為綠色毛衣自己就是標準網址，而黃、紅、藍三個毛衣的頁面才是重複內容）。

感謝 Harris 大大的精闢說明 -> [](http://www.yesharris.com/content-duplicate-issue/) [http://www.yesharris.com/content-duplicate-issue/](http://www.yesharris.com/content-duplicate-issue/)