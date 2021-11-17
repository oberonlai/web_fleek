---
title: 從新學習HTML5 (二) 表單
tags: []
id: '214'
categories:
  - - HTML5
date: 2019-03-15 10:59:38
---

上一篇在這邊--->[從新學習HTML5 (一) 概觀](https://oberonlai.blog/html5-intro/)

HTML5 在處理表單的部分有很大的躍進，以前最喜歡用 jQuery Validation Plugin 來處理各種表單欄位的驗證，但現在 HTML5 內建許多 API，初步看來，實務上的表單需求應該都能不透過外掛而用 API 達成，在瀏覽器的支援程度上也算完整，已經算是可以實戰的階段。

關於表單的章節本書先從 input 新增的屬性介紹，還有一些可以更彈性使用 <form> 標籤的屬性，新增的 type 類型直接自動幫你判斷輸入文字是否符合規則，還有不知道什麼時候派得上用場的輸入文字選取 API，以及最實用的 input 驗證 API。希望學到這些以後可以把表單秒刻完啊啊啊～

## input 相關屬性

顧名思義就是給 <input> 標籤專用的屬性，可以直接運用屬性來給輸入欄位下條件，很大幅度省下利用 js 來定義限制條件的時間，當然，比較複雜的條件就還是只能自己寫了。

**autocomplete** - 讓瀏覽器記憶曾經輸入過的文字，下次輸入時會自動提示，沒有特別定義這個屬性的話，預設狀態是開啟的，如果要關閉它再寫 autocomplete=off。

```
<!-- 停用自動提示功能 -->
<input type="text" name="username" autocomplete="off" />
```

**size** \- 控制輸入文字的顯示長度，但不會限制字數，有點不知要用在什麼地方，能輸入幾個字元不就是告訴使用者限制的字數嗎？

```
<!-- 可輸入無限多位元，但只會出現六位數 -->
<input type="text" size=6 />
```

**maxlength** \- 限制輸入字數，這個很常用  

```
<!-- 只能輸入六個位元 -->
<input type="text" maxlength=6 />
```

**required** - 必填欄位，一個單字搞定

```
<input type="text" required />
```

**readonly** - 顯示唯讀狀態，不得輸入任何內容

**min/max** - 設定輸入日期或數字的最大值與最小值，也適用在設定 type=range 滑桿控制的範圍

```
<!-- 輸入介於 2~10 的數字 -->
<input name=day type=number min=2 max=10>
```

**step** - 設定更改數量或滑桿時的級距

```
<!-- 每次修改必須以 2 為單位 -->
<input type=number min=1 max=100 step=2>
```

**pattern** - 使用正規表達式來驗證輸入資料

```
<!-- 只允許輸入大寫英文字母一個 -->
<input type=text pattern="[A-Z]" />
```

**autofoucs** - 指定頁面載入時預設停留的 input，除了 input 外，select 與 button 也支援該屬性

**multiple** - 輸入資料至少要兩組以上，以逗號分隔

```
<!-- 至少要輸入兩組以上的 email -->
<input type=email multiple>
```

## 與 <form> 標籤相關的屬性

自己遇過一個問題，有時候為了排版的考量，表單裡面的欄位沒辦法包在 <form> 標籤裡面，這時候用 form 屬性就可以不受限制，另外還新增了可以複寫 action、method 等屬性的用法，只是又不知道這是什麼時候用得到...

**form** - submit 傳送時讓沒有包在 <form> 裡面的欄位也可以取得其值，但要先給 <form> 一個 id，然後把 form 屬性放在 input 裡面並指定要對應表單的 id

```
<form id="myform">
  <button type=submit>SEND<button>
</form>
<!-- input 不用在表單裡面 -->
<input type=text form="myform" required />
```

**formaction** - 用在 submit button 裡面，複寫 <form> 本身的 action 屬性

```
<form>
  <input type=submit formaction="abc.url">
</form>
```

**formmethod** - 用在 submit button 裡面，複寫 <form> 本身的 method 屬性

```
<form>
  <input type=submit formmethod="post">
</form>
```

**formtarget** - 用在 submit button 裡面，複寫 <form> 本身的 target 屬性

```
<form>
  <input type=submit formtarget="_blank">
</form>
```

**formnovalidate** - 用在 submit button 裡面，取消欄位驗證功能

```
<form>
  <input type=submit formnovalidate>
</form>
```

## 新增的 input type 類型

這些新增的 type 可以更方便的去指定驗證欄位條件，因為都內建的，常見的電子郵件或是 URL 都能判斷，更進階的還有日期選擇器、顏色選擇器，但瀏覽器支援差，還是要靠外掛處理了。

**type=search** - 同 input=text，但 chrome 上會多一個 X 可以清除已輸入文字

**type=tel** - 代表電話，驗證條件為純文字

**type=url** - 代表網址，驗證條件為前方要帶有 http:// 開頭的文字

**type=email** - 代表電子郵件，驗證條件為 @ 符號

**type=password** - 代表密碼，會自動以星號取代輸入文字

**type=datetime 相關** - 日期選擇器，但瀏覽器支援弱，不實用

**type=number** - 代表數值，驗證條件為數字

**type=range** - 使用滑桿輸入數字

**type=color** - 顏色選擇器，每個瀏覽器的選擇介面都不太一樣，不好自訂

## input 文字選取的 api

用來取得 input 裡面文字被選取的狀態，當用滑鼠把文字選取起來時，可以得知是從第幾個字元開始被選取，也可以取得最後一個被選取的字元，還可以指定要自動選取的範圍，完全沒概念這要怎麼應用...

**selectionStart** - 文字選取屬性，表示被選取文字第一個字的索引值

**selectionEnd** - 文字選取屬性，表示被選取文字結束的索引位置

**selectionDirection** - 文字選取屬性，偵測選取文字時是向前還向後選取

**select()** - 文字選取方法

**setSelectionRange(start,end, direction)** \- 程式設定選取範圍，最後一個 direction 選填

```
<input type="text" id="text-box" size="20" value="1234">
<button onclick="selectText()">選取2跟3</button>
<script>
function selectText() {
  var input = document.getElementById('text-box');  
  input.focus();
  <!-- end 索引值是指被選到文字的下一個 -->
  input.setSelectionRange(1, 3); 
}
</script>
```

## input 驗證 API

驗證 API 是本章節的重點，在很多情況下，根本不會有 submit 傳送事件的觸發，尤其是現在常見的 ajax 應用，以及熱門的 js 框架，都採用非同步的方式去處理資料傳遞，所以學會如何使用 API 去控制驗證輸入欄位是非常重要的。

**willValidate** - 判斷欄位是否帶有驗證條件，如果有的話值是 true，這邊驗證條件指的是內建的 type 類型所判斷的條件，自己寫的條件無用，BUT.....

```
<input id="hadValid" type=email>
<input id="hadNoValid" type=text>
<button onclick="check()">CHECK</button>
<script>
function cheeck(){        
  alert(document.querySelector("#hadValid").willValidate)
  alert(document.querySelector("#hadNoValid").willValidate)
}
</script>
```

我以為第一個 alert 是 true，第二個是 false，想不到第二個也是 true，後來爬到說 willValidate 唯一會 false 的情況，只有在 input 被設為 disabled 的時候，而且設了 disabled 不管 type 是什麼，全都是 false，這個屬性應該改名叫做 **isDisabled** 才對吧....Orz

```
<input id="hadValid" type=email disabled>
<input id="hadNoValid" type=text disabled>
<button onclick="check()">CHECK</button>
<script>
function check(){ 
  <!-- 兩個都回傳 false，無言.... -->
  alert(document.querySelector("#hadValid").willValidate)
  alert(document.querySelector("#hadNoValid").willValidate)
}
</script>
```

**checkValidity()** - 判斷欄位是否有通過驗證，有的話回傳 true，如果在未輸入任何值得情況下，這個方法也是會過，所以要用他判斷最好要再加上檢查 value 是否為空，或是給 input required 屬性會比較準確

```
<!--  注意！沒設必填的話空值也會驗證成功 -->
<input id="check" type=email required> 
<button onclick='checkValid()'>CHECK</button>
<script>
function checkValid(){
  if( document.getElementById('check').checkValidity() ){
    alert("我成功啦～")
  } else {
    alert("我失敗啦～")
  }
}    
</script>
```

**validity** - 用來顯示錯誤狀態的一個物件 ValidityState，可以根據取得的結果來做相對應的動作，取得的結果有以下幾種，可以很精準的判斷錯誤狀態以提供使用者正確的回饋訊息：

(一) validity.valueMissing -- 有必填欄位未填的話返回 true

```
<input id="check" type=text required> 
<button onclick='checkValid()'>Send</button>
<script>
function checkValid(){
  if (document.querySelector("#check").validity.valueMissing) {
    alert("必填欄位未填")
  } else {
    alert("驗證成功")
 } 
}    
</script>
```

(二) validity.typeMismatch -- 輸入格式不對的話返回 true，像是 type=email 沒有 @xx.xx，或是 type=url 沒有 http://

```
<input id="check" type=email required> 
<button onclick='checkValid()'>Send</button>
<script>
function checkValid(){
  if (document.querySelector("#check").validity.typeMismatch) {
    alert("格式有誤")
  } else {
    alert("驗證成功")
 } 
}    
</script>
```

(三) validity.patternMismatch -- 正規表示法的驗證沒過返回 true

```
<input id="check" type=text pattern="[A-Z]{3}" required> 
<button onclick='checkValid()'>Send</button>
<script>
function checkValid(){
  if (document.querySelector("#check").validity.patternMismatch) {
    alert("格式有誤")
  } else {
    alert("驗證成功")
 } 
}    
</script>
```

(四) validity.tooLong -- 超過 maxlength 設定的上限值回傳 true，但 Chrome / Safari 似乎不支援，測試了老半天都無法取得 true，還是只好乖乖的用 length 去判斷了

(五) validity.rangeUnderflow -- 小於 min 屬性的下限值回傳 true

```
<input id="check" type=number min=10> 
<button onclick='checkValid()'>Send</button>
<script>
function checkValid(){
  if (document.querySelector("#check").validity.rangeUnderflow) {
    alert("數字不得低於10")
  } else {
    alert("驗證成功")
 } 
}    
</script>
```

(六) validity.rangeOverflow -- 大於 max 屬性的上限值回傳 true

```
<input id="check" type=number max=10> 
<button onclick='checkValid()'>Send</button>
<script>
function checkValid(){
  if (document.querySelector("#check").validity.rangeOverflow) {
    alert("數字不得大於10")
  } else {
    alert("驗證成功")
 } 
}    
</script>
```

(七) validity.stepMismatch -- 不符合 step 變量時回傳 true

```
<input id="check" type=number min=2 max=10 step=2 value=2> 
<button onclick='checkValid()'>Send</button>
<script>
function checkValid(){
  if (document.querySelector("#check").validity.stepMismatch) {
    alert("增加數值要是 2 的倍數")
  } else {
    alert("驗證成功")
 } 
}    
</script>
```

(八) validity.customError -- 判斷是否有設定 setCustomValidity 自訂錯誤訊息文字，有的話為 true

(九) validity.valid -- 僅顯示驗證是否有通過，有的話為 true，驗證沒過為 false

```
<input id="check" type=text required> 
<button onclick='checkValid()'>Send</button>
<script>
function checkValid(){
  if (document.querySelector("#check").validity.valid) {
    alert("驗證成功")
  } else {
    alert("驗證失敗")
 } 
}    
</script>
```

**setCustomValidity(message)** - 自訂驗證未過時的錯誤訊息，並且用 inputElement.validationMessage 取得自訂的錯誤訊息

```
<input id="input" type='text' required/>
<p id='msg'></p>
<button onclick='check()'>SEND</button>
<script>
  var input = document.querySelector('#input');
  var msg = document.querySelector('#msg');
  function check(){
    if( input.validity.valueMissing ) {
       /* 設定錯誤顯示文字   */
      input.setCustomValidity('我錯啦～')
      /* 因為有自訂錯誤訊息，所以是 true */ 
      alert(input.validity.customError ) 
      /* 取得錯誤顯示文字 */
      msg.innerHTML = input.validationMessage 
    }
  }
</script>
```

## 可用在 Input 的新事件

完整的新事件的部分將在下一章中介紹，這邊先介紹兩個跟 input 欄位相關的，一個是 oninput 另一個是 oninvalid。

oninput 可以解決因為 onkeyup 跟 onkeydown 造成輸入判斷不精準的問題，而 oninvalid 顧名思義就是當驗證未過時觸發的事件，我們把上面 setCustomValidity 的寫法改用這兩個事件來處理。

```
<!-- 用事件設定錯誤訊息，在輸入時要清空，在驗證未過時設定 -->
<form action="">
  <input type='text' oninput="setCustomValidity('');" oninvalid="setCustomValidity('我錯啦～');" required/>
  <input type='submit' />
</form>
```

## 小結

這樣整理並且實作下來，發現還是有些屬性跟想像中的用法不一樣，雖然書上的文字描述好像很清楚，但實際用 jsfiddle 去 run 才發現根本不是這麼一回事，另外瀏覽器支援度也是個問題，在用的時候要先查清楚才行。

下一篇繼續整理 HTML5 的新事件。