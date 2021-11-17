---
title: 從新學習HTML5 (四) 語意元素
tags: []
id: '267'
categories:
  - - HTML5
date: 2019-04-01 22:25:54
---

section - 表示文件中的特定區塊，裡面可以下 h1~h6

article - 表示文件中與其他內容沒有關聯的獨立區塊，通常會放一段文字

aside - 表示文件中與其他內容有輕微關係的區塊，多用於側邊欄的標籤

header - 頁首或是 section 的頁首

hgroup - 區塊內容的標題區域，裡面會有多個 h1~h6 標籤

footer - 頁尾或是 section 的頁尾

nav - 表示帶有導覽功能的區塊

figure - 帶有說明的圖像元素

figcaption - figure 區塊的標題

與以上無相關的區塊就還是使用 div，有需要大綱編排層級的內容就用 section。

## HTML 5 元素分類

Metadata content - <head> 裡面的東西

Flow content - 文件內大部分的內容皆屬此類

Sectioning content - 帶有層級的大綱

Heading Content - 定義 section 的標題

Phrasing content - 文字內容的標籤元素

Embedded content - 外部匯入的內容至目前網頁

Interactive content - 支援使用者的互動的元素

## 大綱 Outline

瀏覽器會根據使用的 h1~h6 標籤輸出大綱來讓搜尋引擎更能理解整個網頁內容的架構，大綱就如同一本書的目錄，所以是以標題標籤來進行劃分，所以如果每個區塊有獨立的標題層級，要用 section 來進行分隔，並且都是從 h1 開始以方便辨識。

```
<body>
<h1>Title</h1>
<section>
  <h1>Section Title</h1>
</section>
<section>
  <h1>Section Title</h1>
</section>
</body>
```