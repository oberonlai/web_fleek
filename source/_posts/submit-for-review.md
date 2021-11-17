---
title: Wordpress文章發表按鈕變成送交審查
tags: []
id: '1153'
categories:
  - - WordPress
date: 2013-11-14 21:55:39
---

最近中了頭獎，當然不是大樂透，而是 wordpress 的離奇懸案。 話說前天有 FU 想寫些文章，大概打了兩大段準備先儲存為草稿時，發現存的時間異常的久，於是想說算了，先直接發表文章看看會不會比較快，結果看到「發表」按鈕變成「送交審查」讓我下巴掉了下來。 [![04](https://oberonlai.blog/wp-content/uploads/2013/11/04.jpg)](https://oberonlai.blog/wp-content/uploads/2013/11/04.jpg) 這網站的管理員只有我一個人，我也從未修改過發表文章的權限，正在狐疑怎麼會變成這種情況的時候，另外一個更驚人的現象發生了， [![01](https://oberonlai.blog/wp-content/uploads/2013/11/01.jpg)](https://oberonlai.blog/wp-content/uploads/2013/11/01.jpg) [![02](https://oberonlai.blog/wp-content/uploads/2013/11/02.jpg)](https://oberonlai.blog/wp-content/uploads/2013/11/02.jpg)   所有的文章、頁面、分類、標籤全都變成零，進去列表的時候也發現所有文章都不見了，急急忙忙跑去前臺看，發現網頁的載入速度異常遲緩，好不容易載入完後發現唯一值得慶幸的事情，就是所有頁面、文章都還在。   Google 了之後只發現有對岸同胞在百度上提了這個問題，但並未有任何解決辦法，搜尋了 wordpress support，發現到幾篇類似相同症狀的文章， 1.[A cause of the "submit for review" problem](http://wordpress.org/support/topic/a-cause-of-the-submit-for-review-problem?replies=3) 2.[Submit For Review ---- No Publish button](http://wordpress.org/support/topic/submit-for-review-no-publish-button) 3.[Solve a “Submit For Review” issue on WordPress](http://goo.gl/IDj5Qq)   我嘗試過的方法如下： 1.進入 phpmyadmin 修復資料庫 2.進入 phpmyadmin 修改 wp\_usermeta 裡 wp\_capabilities 的值 3.新增資料庫使用者帳密，修改 wp\_config.php 的值 4.執行 ALTER TABLE some\_table AUTO\_INCREMENT=1000 5.匯出所有頁面文章，再重新匯入（經實驗在這種情況下匯出的檔案只有 2KB，內容是 Hello World...） 6.上 wordpress taiwan 正體中文粉絲團求救 7.趕快洗澡睡覺祈求隔天一切完好如初...   如果您的 wordpress 也碰到這個問題，想必您一定也很著急解決辦法是什麼，我的解決辦法很簡單，那就是...
<!-- more -->
 

## 一切重來吧！

  沒錯，我重裝了一套 wordpress，從備份恢復頁面、文章，經過一番功夫總算是回歸正軌，所以小弟事實上完全沒解決這個問題，也無從釐清這問題的狀況是如何發生的，這實在是令人非常氣餒的一件事，但還是在此留個紀錄，讓將來有遇到這問題的朋友有個參考方向，進而解決它。