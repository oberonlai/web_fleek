---
title: FS 程式開發日誌
tags: []
id: '283'
categories:
  - - 老人碎念
date: 2019-12-03 09:38:19
---

### 2019/12/03

重刻編輯器的 UI ，JS 的部分比想像中要來得棘手，改了 DOM 之後噴一堆錯，看來還是習慣用 getElementById 比較安全，querySelector 太依賴 DOM 結構。然後意外發現 CSS 的 calc 竟然可以先乘除後加減，對於要計算 100vh 的比例很方便。

```
height: calc( ((100vh - 102px) / 5) / 3.5 );
```

### 2019/11/29

「開始設計你的影像故事吧！頁面管理可以讓你新增、刪除、編輯頁面，還可以使用拖曳的方式來改變順序，在開始製作前不妨先把影像故事的頁面結構先建立完成，說一個好故事就從大綱開始！」這是在設計頁面管理的介面時，寫下的使用教學說明文字，在重新整理介面時，發現到一件事，這工具的背後，是不是該從教使用者如何寫出一個好故事開始？想要打造品牌，沒有一個好故事要從這麼競爭激烈的環境中脫穎而出實在困難，除非商品有獨特性或是獨佔性，但一般的業者可能很難有這樣的資源，業者的素材不好、品牌沒有深度、產品沒有靈魂，相反的，每次看旅遊雜誌總是介紹許多台灣努力在地深耕的品牌，其品質、包裝、行銷能力完全不輸國外品牌。希望這工具，可以幫助有心要經營品牌的業者，幫助他們把他們的理念可以傳達給更多人。

### 2019/11/28

搞定頁面排序功能，比想像中碰到的阻礙小，要注意的地方就是重新排序完畢之後，要更新 data-slide 的數值，因為這會關係到 slide 的切換順序

```
setPageOrder(){
  this.vars.pageNav.addEventListener('stop',e=>{
    let orderArray = [];
    Array.from(this.vars.pageNav.querySelectorAll('li a')).forEach(element => {
      orderArray.push(`${element.dataset.page}`)
    })
    for (let i = 0; i < orderArray.length; i++) {
      this.vars.pageNav.querySelectorAll('li a')[i].dataset.slide = i;
      this.vars.slideshowPage.querySelector('.uk-slider-items').append(document.querySelector(`li[data-page=${orderArray[i]}]`))
    }
    Array.from(this.vars.pageNav.querySelectorAll('li a')).forEach(element => {
      setTimeout(() => {
        element.classList.remove('active');
      }, 600);
    });
  })
}
```

開始著手進行編輯器的 UI 調整，先用 Sketch 設計，讓左側主選單有順序性，引導使用者照步驟操作，第二層選單改用 tab 來設計，緊靠著主選單來暗示關聯性，比較麻煩的地方就是小工具的地方需要改寫不少 code，變成不能用 offcanvas 來做而是要用 toggle，這樣的調整在 class 方面可能會造成某些無法預期的問題，封面設定的部分捨棄讓人困惑的 Google 搜尋結果頁，改以單純的頁面來顯示，輔以文字說明，在視覺線索上可以多強調這張圖是封面的意涵。

### 2019/11/27

在改變小工具的尺寸後，右邊的設定項會噴錯這件事過關了，不是原本 moveable 的寫法有問題，而是在 script.js 裡面有做一個取得 config 的點擊事件，因為取到的 e.target 包含範圍太廣，造成在拖曳 moveable 也會觸發到偵聽事件，而 moveable node 並沒有 data-config 的屬性，所以導致 config 噴錯，處理方式是把這一個事件拿掉，但又會造成背景的 config 取不到，所以再寫了一個點擊事件來取得背景圖的 config。因為這件事學到了怎麼從 class 裡面把需要的東西傳出來，只要在建構式的時候先取好要拿的東西的變數名，然後指派值給他，這樣他就會變成這個類別的屬性，接下來只要 new 出實體後就可以用 . 取得該屬性的值。

```
Array.from(document.querySelectorAll('li a[data-config^=page]')).forEach(element => {  // 點擊內頁底圖取得 config
    element.addEventListener('click',(e)=>{
      config = e.currentTarget.dataset.config;
    })
  });
```

開發上遇到小低潮，根本原因是找不到現階段自己開發的目標是什麼？原本希望 11 月底就可以完成編輯器的部分，但遇到 UI 需要大幅度的調整，需要重新思考這工具的操作邏輯應該會是在何種使用情境底下，或是去類比既有的設計模式來進行調整，如果是以部落客為主要使用對像，操作方式可能會是類似文章發布系統，或是社群網站貼文的模式。如果是以大眾都熟悉的軟體，可能比較像是 Power Point，但真要這樣改下去整個 UI 都要重弄了，懊惱自己開發前為何不多做點研究以及使用 Prototype 做 user test，只能從現有的邏輯中先去調整看看了。

所以有一種走了三個月又要從頭來過的感覺，想放棄的念頭就這樣冒了出來，再加上對於產品方向的不確定性，開始懷疑這一切是不是做白工？一覺睡起來，看著已經完成的 task 有 109 個，程式碼行數超過萬行、開發總小時數超過 300 個小時，學會了 js 的 class 運用、不依賴 jQuery 全寫原生 js、如何使用 localStorage 儲存複雜的資料、如何開發各種頁面小工具、drag＆drop、ajax insert post...，以一個 Side Project 的練習量來看好像已經足夠了XD。

接下來呢...？進入撞牆期了，依照網站開發十多年的經驗，最好的解決辦法就是閉上嘴，專注在每一個 task 上面，其他，都不用想了。

### 2019/11/25

這件事情卡關，在改變小工具的尺寸後，右邊的設定項會噴錯，原因是因為當點擊縮放控制點時，用來判斷要改變的對象 config 變數被清空了，所以需要在點擊縮放控制點時，要設定 config 為當前小工具的 data-config，於是寫了下面這一段，想要放在類別裡面，但 config 變數是在最外層的建立的，因此下面這段必須放在外層，又因為他是非同步事件，無法使用 return 的方式來回傳值，只能做成 function 寫在外層的 dataInput function 裡面

```
Array.from(document.querySelectorAll('.wrap-moveable.active .moveable-control')).forEach(element => {
  element.addEventListener('click', e => {
    config = e.currentTarget.parentNode.previousElementSibling.dataset.config;
  })
})
```

紀錄一下頁面拖曳改變順序的思路：左側選單當完成拖曳後，會依照現在的選單的順序組成一個 Array，去抓 data-page-name，組成 \["page0","page1","page2"\]，然後中間的區塊會用迴圈的方式來 append innerHTML，用 getElementById 取得 node，然後依序 append。

點擊複製網址功能，input 不能是在 disabled 的狀態下才有作用，但又不想讓 user 可以修改 input 裡面的內容，所以從 css 下手，用 pointer-events 讓 user 選不到 input field

```
vars.btnCopyEmbedCode.addEventListener('click', () => {
  vars.embedCode.select();
  vars.embedCode.setSelectionRange(0, 99999)
  document.execCommand("copy");
  vars.btnCopyEmbedCode.textContent = "已複製！"
  setTimeout(() => {
    vars.btnCopyEmbedCode.textContent = "複製程式碼"
  }, 2000);
})
```

FB & Line share url

```
<a href="https://facebook.com/sharer.php?u=<?php the_permalink(); ?>" target="_blank"><img src="<?php echo get_template_directory_uri(); ?>/img/01_newspopup_fb.svg" width="9"></a>
<a href="https://lineit.line.me/share/ui?url=<?php the_permalink(); ?>" target="_blank"><img src="<?php echo get_template_directory_uri(); ?>/img/01_newspopup_line.svg" width="16"></a>
```

### 2019/11/23

新增了 Bookend 的 CTA button，結果又發生逗號問題而讓 Bookend 噴錯，實在受夠了，乖乖把 Bookend 的 JSON 改用 PHP 的 Array 來組，最後再用 json\_encode 來餵給 js，一開始組都還沒問題，但卡在 bookend link 這個地方，因為 as 要求的格式為 {},{}，但因為適是用陣列組出來的，所以 php 會變成 \[{},{}\]，多了外面的中括號 bookend 又會 GG，試著把 php array 轉成 object，但 encode 就還是會出現中括弧，一度想去 slack 裡面抱怨哪有人把多筆資料設計成 {},{} 這樣的格式，應該都是要用陣列才對，然後再想想問題提出了後面還要跟進與討論，覺得好懶好花時間，最後用了很爛的招： str\_replace 四次來處理掉中括號以及多餘的逗號問題，希望未來有工程師看到我現在的解法，能想出更好的解決方案了。

```
<?php echo str_replace(',",',',',str_replace('}",','},',str_replace('},"','},',str_replace('\"','"',json_encode( $bookend,JSON_UNESCAPED_UNICODE ))))); ?>
```

另外修改好了 sidebar 的 style，多加了可顯示或關閉 sidebar 的設定項，讓不需要使用 sidebar 的人就能把它關閉，加了重整頁面時會跳出未儲存檔案的提醒、根據文字小工具選到的字型動態載入 Google font。

### 2019/11/22

把 Bookend 做了一個整理，加了品牌介紹的區塊，然後徹底檢查會造成 Bookend error 的各種狀況，會有 error 是因為 Bookend 必須是 JSON 格式，少或多一個逗號都會造成錯誤，當 user 沒有填 Bookend 的所有項目資料為空時，逗號的判斷沒有寫好所以造成各種錯誤，後來想想這個區塊不應該這樣寫，而是先用 PHP 組成陣列之後，再用 json\_encode 去產出 bookend 需要的東西，這樣就不用擔心逗號的問題，未來有時間再處理了。

另外增加了 uploadcare 的檔案上傳限制，容量限制為 4 MB，檔案型態只允許 jpg png gif mp3 mp4 ，其他全鎖，避免有心人士上傳了惡意腳本來亂。