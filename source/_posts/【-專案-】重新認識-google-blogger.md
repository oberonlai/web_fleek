---
title: 【 專案 】重新認識 Google Blogger
tags: []
id: '422'
categories:
  - - 專案
date: 2019-12-23 12:32:44
---

![](https://oberonlai.blog/wp-content/uploads/2019/12/CleanShot-2019-12-23-at-12.08.02-1024x703.jpg)

在還沒有開始學習 WordPress 以前，Google Blogger 是我最早接觸的部落格平台，有別於當時台灣的無名小站、Yahoo 奇摩家族、天空部落等等，Blogger (當年還叫做 Blogspot) 可以自訂的部分更多，還可以寫程式碼，重點是不會逼人硬要塞廣告，後台的操作介面友善，因此 Blogger 成為我紀錄個人資料的首選平台。

<!--more-->

後來的日子裡，因為工作需要開始研究 CMS，轉而發現 WordPress 這個開源的部落格平台，如果以賣東西來比喻，Blogger 是推著路邊攤在賣，而 WordPress 就是全球連鎖超級市場，後者的彈性以及衍伸的周邊資源，是 Blogger 望塵莫及的，也因此 Blogger 就慢慢的從我的腦海中遠去...(遠目)

## 改版任務

這次的專案接到一個很特別的任務，沒錯，就是要幫 Blogger 改版(驚)，在了解業主需求時，早已熟悉 WordPress 的我無不大力推從更換部落格系統，佈景主題多、外掛多、相關資源多...

想不到列了一堆好處還是無法說服業主更換 Blogger，該部落格以經營了六年多，裡面有 500 多篇文章，文章的上稿方式是先從 iPad2 的備忘錄完成文章，然後再用 iOS5 版的 Safari 登入 Blogger 後台把文章貼入，重點就是在於 WordPress 後台在舊版的 Safari 無法正常運作，也無法使用 WordPress 的 iOS App，因此在不改變上稿流程的情況下，還是只能繼續沿用 Blogger 。

![](https://oberonlai.blog/wp-content/uploads/2019/12/CleanShot-2019-12-23-at-12.10.38-1024x755.jpg)

舊版網站的首頁&內頁

而這此需要改版的主要原因，在於舊版版面的設計已經無法符合 500 多篇文章的需求，再加上分類項目數量眾多，對於瀏覽者很難透過視覺動線去尋找文章，其次是文章以書評為主，大量的文字缺少圖片，少了圖片就比較難吸引瀏覽者點閱，因此針對舊版的網站的問題提出了相對應的改善方案：

### (一) 文章太多被埋沒

*   改變版面配置
*   增加圖片
*   增加精選文章
*   增加熱門文章

### (二) 分類太多難搜尋

*   主選單顯示文章分類
*   規劃書籍主題策展

### (三) 跳出率過高

*   增加文章延伸閱讀
*   顯示同類別的其他文章

### (四) 與其他書評部落格缺少差異化

*   增加品牌標語與主視覺元素

## 遇到的挑戰

太習慣用 WordPress 的邏輯來思考部落格，首頁是 index、內頁是 single、列表頁是 archive，不同用途的頁面應該是分屬在不同檔案，但 Blogger 從頭到尾就只有一份 xml 檔 (雖然介面文字是寫 html，但標頭明明就是 xml)，要如何在同一份檔案去處理不同的頁面呈現是第一個面臨到的問題，

其次是 Blogger 把資料、介面、控制邏輯也都放在同份 xml 檔，這對於習慣 MVC 邏輯的我實在是坐立難安，因此常發生明明已經改過的東西卻被還原，這是因為後台的控制項變更後，Blogger 會自動把 xml 檔進行覆寫，所以逼自己養成習慣，改 xml 之前都要先重整頁面。

最後是 Blogger 的 xml 編輯器實在不怎麼友善，因為要把所有資料、頁面全都塞進去，每次載入時會需要花上一些時間，對於習慣 hot reload 的我就像等上十年之久，然後因為 controller 也在裡面，所以會看到一堆 Blogger 專用的 <b> tag，在閱讀上有些吃力，也只能逼自己慢慢習慣了。

但這個編輯器讓我最崩潰的地方是當 xml 寫錯時，點下儲存等了約五秒後，會跳出「更新失敗」的提示，就這樣，只顯示「更新失敗」這四個字，除此之外沒有任何的除錯訊息告訴我是哪邊發生錯誤、錯在哪邊 (怒)，只好用刪除法把剛剛寫過的東西逐一刪除看是誰出問題，但每儲存一次就要五秒以上，因此常常改個小東西就要半天的時間了，實在是苦不堪言。

也試著把整份 xml 貼入 viscode 改完後再貼回去，但即使是原封不動的複製貼上，只要一進過編輯器的 code 就是無法再正常儲存，然後繼續丟「更新失敗」這四個字給你，總覺得這陣子在睡覺時做夢都夢到這四個字...

![](https://oberonlai.blog/wp-content/uploads/2019/12/CleanShot-2019-12-23-at-12.13.30-1024x795.jpg)

更新失敗地獄

最後沒辦法，還是找現成的主題來修改，選擇了 Emporio Flamingo 這個主題，有首頁、內頁兩種版型，側邊欄也可以根據不同版型來進行設定，另外他也設計了他自己的小工具，在套用上還滿方便的，然後也從他的程式碼中去學習 Blogger 的專屬語法，回想起自己最早學習 WordPress Theme Custom 也是從別人寫好的佈景主題去摸索的。

![](https://oberonlai.blog/wp-content/uploads/2019/12/CleanShot-2019-12-23-at-12.14.23-1024x769.jpg)

謝謝 Emporio 的拯救

## Blogger Tags

Blogger 標籤是以 <b:> 這樣的形式出現，也是會有開頭跟結尾，然後根據不同的標籤類型會帶有相對應的屬性，常用的標籤有以下幾種：

### (ㄧ) 判斷式 <b:if>

要在同一個檔案顯示不同頁面的內容主要就是要靠判斷式，其屬性為 cond，判斷不同頁面的寫法如下：

```
<b:if cond='data:blog.pageType == "archive"'>
<!--庫存頁面會出現的內容-->
</b:if>

<b:if cond='data:blog.pageType == "index"'>
<!-- 首頁會出現的內容 -->
</b:if>

<b:if cond='data:blog.pageType == "item"'>
<!-- 文章內頁會出現的內容 -->
</b:if>
```

判斷式也可以用巢狀結構來達成 AND，或是使用 else 來進行條件不成立時的描述。

```
<b:if cond='data:blog.pageType == "index"'>
  <b:if cond='data:blog.searchQuery'>
    <!--首頁與搜尋頁會出現的內容-->
  </b:if>
</b:if>

<b:if cond='data:blog.pageType == "item"'>
    <!--文章內頁會出現的內容-->
<b:else/>
    <!--除了文章內頁以外會出現的內容-->
</b:if>
```

更多條件式的用法可以參考 -> [https://ultimatebloggerguide.blogspot.com/2016/07/blogger-conditional-tags-for-page-types.html](https://ultimatebloggerguide.blogspot.com/2016/07/blogger-conditional-tags-for-page-types.html)

### (二) 迴圈 <b:loop>

迴圈會用到的地方是在要顯示文章列表時使用，範例如下：

```
<b:loop index='i' values='data:posts' var='post'>
  <div>
    <h3>
      <b:eval expr='data:i + 1' />. <data:post.title />
    </h3>
    <div>
      <data:post.body />
    </div>
  </div>
</b:loop>
```

可參考 -> [https://bloggerbook.blakbin.com/2018/11/blogger-bloop-tag.html](https://bloggerbook.blakbin.com/2018/11/blogger-bloop-tag.html)

### (三) 常用參數

如果要取得網站標題、文章名稱、發表日期等等的相關資訊，就可以使用 <data:> 標籤來取得，沒有結尾標籤，直接使用斜線結尾，相關範例如下：

```
<data:blog.title />      // 取得網誌標題
<data:blog.pageType />   // 取得頁面類型
<data:posts.title />     // 取得文章標題
<data:posts.body />.     // 取得文章內文
```

完整屬性可以參考 Google Blogger 文件 ->[https://support.google.com/blogger/answer/47270?hl=zh-Hant](https://support.google.com/blogger/answer/47270?hl=zh-Hant)

但在實際運用上有滿多在 Google 文件上都找不到的屬性，然後有些屬性寫在 <Head> 會沒作用，當初在寫 og tag 的時候嘗試了滿多，以下為可正確顯示的範例：

```
<head>
    <meta content='width=device-width, initial-scale=1' name='viewport'/>
    <title><data:view.title.escaped/> &#8226;文學女孩的開卷日常&#8226;</title>
    <meta content='IE=edge,chrome=1' http-equiv='X-UA-Compatible'/>
    <meta content='width=device-width, initial-scale=1.0' name='viewport'/>
    <!--Open Graph Begin-->
<meta expr:content='data:blog.title' property='og:site_name'/>
<b:switch var='data:blog.pageType'>
<b:case value='item'/>
   <meta content='article' property='og:type'/>
   <meta content='Miffy Huang' property='article:author'/>
   <meta expr:content='data:blog.pageName' property='og:title'/>
     <meta content='data:widgets.Blog.first.posts.first.featuredImage' property='og:image'/>
   <b:if cond='data:blog.metaDescription'>
      <meta expr:content='data:blog.metaDescription' property='og:description'/>
   <b:else/>
      <meta expr:content='data:blog.metaDescription' property='og:description'/>
   </b:if>
<b:case value='index'/>
   <meta content='blog' property='og:type'/>
   <meta expr:content='data:blog.title' property='og:title'/>
   <meta expr:content='data:blog.metaDescription' property='og:description'/>
   <meta content='https://1.bp.blogspot.com/-vbSrd8AWktA/XfyOaPYu82I/AAAAAAAAAo8/fF0MJDsIFEEg8VKE-xqM2NtBtKGoQGGiACLcBGAsYHQ/s1600/ogimage.jpg' property='og:image'/>
<b:case value='archive'/>
   <meta content='website' property='og:type'/>
   <meta expr:content='&quot;&quot; + data:blog.pageName + &quot; on &quot; + data:blog.title' property='og:title'/>
   <meta expr:content='&quot;This is an archive page &quot; + data:blog.pageName + &quot; on &quot; + data:blog.title + &quot;&quot;' property='og:description'/>
<b:case value='static_page'/>
   <meta content='website' property='og:type'/>
   <meta expr:content='data:blog.pageName' property='og:title'/>
     <meta content='https://1.bp.blogspot.com/-vbSrd8AWktA/XfyOaPYu82I/AAAAAAAAAo8/fF0MJDsIFEEg8VKE-xqM2NtBtKGoQGGiACLcBGAsYHQ/s1600/ogimage.jpg' property='og:image'/>
   <b:if cond='data:blog.metaDescription'>
      <meta expr:content='data:blog.metaDescription' property='og:description'/>
   <b:else/>
      <meta expr:content='&quot;This is an static page &quot; + data:blog.pageName + &quot; on &quot; + data:blog.title + &quot;&quot;' property='og:description'/>
   </b:if>
</b:switch>
<!--Open Graph End-->
</head>
```

## 加入第三方功能

為了可以讓瀏覽者瀏覽更多文章，在文章內頁下方的加入同分類的其他文章連結，本想自己寫迴圈來撈，但在需要顯示圖片以及摘要的寫法上碰到了困難，於是找了一下有沒有相關的外掛可以使用，想不到還真的有(大心)。

解法是這使用這篇的工具 -> [https://sneeit.com/premium-flexible-related-post-widget-for-blogger-blogspot/](https://sneeit.com/premium-flexible-related-post-widget-for-blogger-blogspot/)

加入的方式很簡單，先把下方程式碼貼入到 xml 文章結尾的地方

```
<!-- START::_140414_related_posts::START -->
<b:if cond='data:blog.pageType == &quot;item&quot;'>
 <b:if cond='data:post.labels'>
  <b:loop values='data:post.labels' var='label'>
   <span class='_140414_related_label' expr:post_id='data:post.id' expr:url='data:label.url' style='display:none'><data:label.name/></span>
  </b:loop>
 </b:if>
 <div class='_140414_related_posts' expr:post_id='data:post.id'>
 </div>
</b:if>
<!-- END::_140414_related_posts::END -->
```

然後再用他們提供的工具來設定要顯示的篇數、是否顯示圖片、日期等等的資訊，最後點擊 Add Widget 就會在 Blogger 後台的版面配置出現相關文章的小工具，然後再把他移動到頁面主體區塊的最下方即可。

![](https://oberonlai.blog/wp-content/uploads/2019/12/CleanShot-2019-12-23-at-12.15.13.jpg)

視覺化設定項

要注意的是相關文章會出現在留言區塊之上，以瀏覽者來說的邏輯怪怪的，應該是先看完留言結束這篇文章的瀏覽後才會顯示其他文章，所以要把上方的程式碼移到留言下方才是正確的位置：

```
<b:if cond='data:showCmtPopup'>
  <div id='comment-popup'>
    <iframe allowtransparency='allowtransparency' frameborder='0' id='comment-actions' name='comment-actions' scrolling='no'>
    </iframe>
  </div>
</b:if>
</section>
<!-- 要在 showCmtPopup 的 Section 下面 --!>
 <!-- START::_140414_related_posts::START -->
<b:if cond='data:blog.pageType == &quot;item&quot;'>
  <h3 class='title_related'>相關閱讀</h3>
...
```

## 後記

這次 Blogger 的改版前後大概花了一個禮拜的時間，比較多的修改都是 CSS 的調整，但最耗時的還是存檔、除錯以及找要改哪裡的時間，目前還在觀察改版之後的瀏覽數據是否有所提升，最重要的還是希望可以吸引到出版社或是書商的邀稿合作，喔，對了，忘了說業主是我太太，還希望有好的機會能懇請介紹XD

[![](https://oberonlai.blog/wp-content/uploads/2019/12/CleanShot-2019-12-23-at-12.16.44-1024x795.jpg)](https://www.justgirl.me)

改版首頁

[![](https://oberonlai.blog/wp-content/uploads/2019/12/CleanShot-2019-12-23-at-12.17.35-1024x792.jpg)](https://www.justgirl.me/2019/06/blog-post.html)

改版內頁

網站連結 -> [https://www.justgirl.me](https://www.justgirl.me)