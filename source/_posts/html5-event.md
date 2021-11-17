---
title: 從新學習HTML5 (三) 事件
tags: []
id: '263'
categories:
  - - HTML5
date: 2019-04-01 22:04:57
---

## Clipboard API

捕捉使用者在頁面剪下、複製、貼上的動作

copy/oncopy 偵測複製行為

```
<p id='text'>這邊是即將要被複製的文字</p>
<script>
  var text = document.querySelector('#text')
  text.oncopy = function(e){
    alert( "你剛剛複製了～" );
    return false; // 多這行禁止複製文字
  }
</script>
```

cut/oncut 偵測剪下行為

```
<p id='text'>這邊是即將要被剪下的文字</p>
<script>
  var text = document.querySelector('#text')
  text.oncut = function(e){
    alert( "你剛剛剪下了～" );
    return false; // 多這行禁止剪下文字
  }
</script>
```

paste/onpaste 偵測貼上行為，使用 clipboardClip.getData('Text') 來取得貼上的內容

```
<p id='text'>這邊是即將要被貼上的文字</p>
<p id='msg'></p>
<script>
  var text = document.querySelector('#text'),
      msg = document.querySelector('#msg')
  text.onpaste = function(e){
    alert( "你剛剛貼上你複製的文字了" );
    msg.innerHTML = e.clipboardData.getData('Text')
  }
</script>
```

## Drag&Drop API

拖曳與置放元素的動作，要完成 drag & drop 需經過以下步驟

1\. 使用 dataTransfer 取得 DataTransfer 物件來設置拖曳資訊

2\. 初始化事件 ondragstart 使用 dataTransfer.clearData() 來清空資訊以及 dataTransfer.setData() 紀錄開始拖曳的元素的資料

3\. 感應被拖曳的元素移動到可置放的元素使用 ondragover 來偵測

4\. 最後的置放使用 ondrop 來觸發，並用 dataTransfer.getData() 來取得拖曳元素，再用 appendChild 來把元素加入到目標元素中

相關事件有：

dragstart - 開始拖曳時觸發  
drag - 拖曳過程中觸發  
dragenter - 拖曳進入目標元素時被觸發  
dragleave - 拖曳離開目標元素時被觸發  
dragover - 拖曳經過目標元素時被觸發  
drop - 置放至目標元素時被觸發  
dragend - 結束拖曳操作時被觸發

相關物件：

dataTransfer.setData() - 寫入拖曳元素資料  
dataTransfer.getData() - 取得拖曳元素資料  
dataTransfer.clearData() - 清空拖曳元素資料  

## History API

透過 JS 來控制頁面的上下頁跳轉以及指定瀏覽歷程

window.history.forward() - 下一頁  
window.history.back() - 上一頁  
window.history.go(pageIndex) - 前往指定的頁面

控制瀏覽歷程以及改變網址列的網址結構

```
// 改變現有網址不換頁
window.history.pushState(null, null, "/new/");

// 作用同上，但不會把瀏覽歷程加到歷史紀錄中
window.history.replaceState(null, null, "/new/");
```

## Selector API

熟悉 jQuery 的話用它當選擇器會很方便

```
<p class='ele' id='ELE-a'>aa</p>
<p class='ele' id='ELE-b'>bb</p>
<script>
document.querySelect('#ELE-a').innerHTML;
var ele = document.querySelectAll('ele').innerHTML;

// 要取出值時要用迴圈去拿
for(var i=0; I< ele.length; i++){
  alert(ele.innerHTML)
}
</script>
```