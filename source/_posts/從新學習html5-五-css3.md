---
title: 從新學習HTML5 (五) CSS3
tags: []
id: '270'
categories:
  - - HTML5
date: 2019-04-04 22:16:21
---

方塊模型 Flexible Box，父層元素宣告為 display: flex 後，可以使用下列屬性來控制子元素的排列方式：

justify-content - 控制所有子元素的排列方式

屬性有 flex-start flex-end center space-between space-around，space-around 在第一與最後一個元素會留空間可以 around。

flex-direction: column 設定子元素為垂直排列，預設為水平排列

align-items - 設定子元素的垂直對齊方式，屬性有 center flex-start flex-end baseline stretch

flex-wrap:wrap 設定子元素要換行

align-content - 設定在子元素換行的情況下，子元素區域的垂直對其方式，屬性有 center flex-start flex-end space-between space-around stretch

order - 設定子元素的順序

border-image - 讓邊框使用圖片，border-image: url('path/border.jpg') 10 20 30 40 repeat，中間數字為上右下左四個角落圖片要被裁切的範圍，也可以使用百分比，最後一個參數為四個角落以外的圖片延展模式。

columns - 讓在同個標籤內的文字可以分欄，設定方式為 columns: column-width column-count

resize: both - 允許使用者改變元素大小，透過右下角的三角形進行拖曳縮放

CSS 動畫相關：

animation-duration 動畫開始到結束的秒數

animation-iteration-count 動畫重複播放的次數，設定為 infinite 則無限循環

animation-direction 正向播放或是反向播放

animation-delay 延遲播放秒數

window.matchMedia - 用 JS 來設定斷點

```
if( window.matchMedia('(max-width: 1000px)').matches ){
  alert("螢幕寬度大於 1000px");
} else if( window.matchMedia('(min-width: 1000px)').matches ){
  alert("螢幕寬度小於 1000px");
}
```

```
// 用 js 做 RWD
var winW = window.matchMedia("(max-width: 768px)");
winW.addListener(resizeWidth);
resizeWidth(winW);

function resizeWidth(pMatchMedia){
  if (pMatchMedia.matches) {
    alert('< 768')
  }else {
    alert('> 768')
  }
}
```