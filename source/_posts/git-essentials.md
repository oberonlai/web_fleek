---
title: 人生也有 Git 就好了...
tags: []
id: '59'
categories:
  - - Git
date: 2019-01-24 23:24:35
---

![](https://oberonlai.blog/wp-content/uploads/2019/01/img_7899.jpg)

回想當年第一次接觸 Git ，是在開發線上旅遊照片整理服務「Mr.Travelo 我的旅遊書」的時候，當時夥伴找到了一位來自美國的工程師來協助我們進行後端程式的開發，我的 Git 就是在這位工程師的指導下，第一次用 Git 實戰在專案之中。

但英文聽力弱弱的我，再加上自己那時候非常習慣用 Dropbox 來做檔案的同步與備份，要學一個新的版本控制方法讓我頭非常痛，光是要把 Git 的環境安裝起來就卡了非常多關，更不用說後來開發時遇到衝突、檔案還原等狀況，卻只能用終端機指令模式來操作的方式，讓我非常不適應 Git。

將近十年過去了，除了比較沒那麼害怕終端機外，也慢慢習慣在每個專案採用 Git 作為版本控制，但總覺得對 Git 的認識還是停留在當年懵懂的階段，於是在圖書館逛到這本，就還是乖乖借回來 K 了～

本書分為七個章節，從基礎到進階的知識，再到實用的設定方法以及工具介紹，算是滿全面的，而且還有說明怎麼從 SVN 移轉到 Git 的方法，實務上還滿常碰到 SVN 的環境，但可能是翻譯書，有些部分不太像中文，或是專有名詞的翻法跟一般見到的不太一樣，市面上有許多其他 Git 的出版，第一次接觸的朋友可能還是買台灣工程師寫的書讀起來比較好理解些。

以下就我自己覺得實用的部分做個紀錄：

## 零、實用的基本指令

git remote add upstream https://xxx.xx 可以跟被 fork 的程式碼保持同步  
git reset HEAD~1 回到前一次的提交  
git reset Head~1^1 回到前一次的分支提交  
git reset --hard HEAD~N 回到前N次的提交  
git log -2 查看最近兩筆的 commit 紀錄  
git reflog 查看所有 git 的操作記錄  
git fetch 只是抓回遠端檔案還沒有進行合併，需要再執行 git merge  
git fetch + git merge = git pull  
git stash 把東西先打包起來，在還沒做完卻又必須切換分支的情況下適用  
git merge 跟 git rebase 後者不會留下 commit 紀錄  
git diff 比對檔案差異

## 一、Github Flow 的介紹

因為沒有待過大型的開發團隊，在我自己開發上很少用到分支這個功能。最完整且最全面的分支管理模式，應該是分為五條的 GitFlow，從分出 delevop、release，再到 feature 以及 hotfix，沒實戰過但感覺起來是滿複雜的，

書中介紹的由 Github 提出的 Flow 就相對單純的多，主要規則如下

1.  在 master 分支的任何東西是可部署的：也就是自己在合併前都能確保程式無誤，不會害 master 爆炸
2.  建立有清楚描述的分支：以開發的功能會處理的具體事項作為分支命名，團隊中要能訂定分支的命名規則，方便其他人辨識。譬如 new-user-creation
3.  經常推送分支：頻繁地將分支推送到遠端伺服器，讓團隊成員透過 git fetch 可以看到活躍分支的狀態
4.  隨時建立 Pull Request：利用 Github 特有的 PR 功能來讓其他人檢視你的程式碼並提供改進建議
5.  只有在經過 Pull Request 通過後才進行合併，確保程式都是有被審核過的
6.  審查通過後立即部署
7.  此流程對於需要頻繁更新的網站專案非常適合，即時且高效

## 二、Git Config 的設定

Git 可以依照自己的使用習慣設定許多便利的功能，像是別名、自動修正錯別字等等，config 設定的影響範圍分為三種層級

1.  系統層級 --system: 這代表設定的影響範圍會包含此電腦中的所有使用者以及使用 Git 做版控的專案資料夾
2.  使用者層級 --global: 只給當前的系統使用者使用，最常用的選項是這個
3.  儲存庫層級 --local: 只作用在當前的工作目錄

git config --global help.autocorrect 10 當拼字有誤時，自動修正 git 指令的功能。設定別名的方式：輸入 git config --global alias.別名名稱 "要取代的指令"，像是設定 git config --global alias.ad "add ."，之後只要打 git ad 就可以完成加入檔案追蹤的動作。其他實用別名設定如下：

git config --global alias.last "log -1 HEAD" ---> 查看最新的 commit 訊息  
git config --global alias.undo "reset HEAD~1"--->回到上一動  
git config --global alias.cm "!git add . && git commit -m"--->使用驚嘆號來執行外部指令，只要輸入 git cm 直接完成 commit

可以用這方法去組合更多讓自己工作更加快速的別名。

## 三、良好的 Git Message 撰寫習慣

這部分是我認為這本書最棒的地方，以往都想說 message 只有自己看，所以大概都會亂打一些英文，連中文都懶得打，然後 commit 很隨便，常常推上去後才發現有東西沒改到，所以就會出現 "修改介面-1"、"修改介面-2" 這樣重複堆疊上去的訊息，簡單說我根本是把 Git 當成 FTP 在用，書中提到的重點完全讓我重新審視自己寫的 message，並且運用在專案之後，一整個覺得考試都考一百分了喔喔喔～

1.  在紙上先將工作事項拆解成小任務：這跟看板方法提到的準備作業很像，先拆分準備要做的事，書中更具體的提到一個蕃茄鐘 25 分鐘可以完成的工作為一個小任務，有這個先決條件就很好拆分任務內容了。
2.  先寫下小任務的提交訊息再開始寫程式碼：這點非常驚人，先寫下要提交的訊息除了可以確保訊息是有意義外，更重要的是他可以讓你專注在這個任務，只開啟跟這個任務相關的檔案，如果改了其他無關的檔案，立刻就會發現這不是我現在要做的事情，就會回頭乖乖做現在該做的事。
3.  每完成一個小任務進行提交：小任務完成後就提交很有爽快感，每次小任務的提交都能離完成大任務更近一步。
4.  別做出斷斷續續的提交：要保持開發的完整性，萬一真的寫到一半被抓去救火，先用 git stash 的功能把目前的進度封存起來。
5.  說明功能變化而非實作細節修 bug 的話先描述問題或是原始需求：訊息以動詞開頭，像是新增、修改、除錯什麼什麼功能或介面，以非工程師也能看得懂的淺顯文字進行撰寫。

現在的網頁編輯器越來越先進，直接整合 Git 版控，只要在工作目錄有 git init，設好 git remote，它就可以抓到有修改的檔案，並且自動 git add 到索引，然後在介面中輸入 message，直接 commit 完成，也能推送到遠端，甚至是做視覺化的 git diff。

![](https://oberonlai.blog/wp-content/uploads/2019/01/螢幕快照-2019-01-24-下午11.14.49.jpg)

![](https://oberonlai.blog/wp-content/uploads/2019/01/螢幕快照-2019-01-24-下午11.11.19.jpg)

我現在都是用 Visual Studio Code 作為主力開發編輯器，強大的功能、豐富的外掛生態以及還能維持很不錯的效能，真的是微軟這幾年最偉大的發明之一也不為過～

## 個人感想

依照這態勢 Git 應該還是會繼續紅下去，更不用說微軟把 Github 原本需要付費的 private repo 功能給完全開放，讓我從 bitbucket 跳槽回 Github，不知道還要亂打什麼感想，十一點半了來洗洗睡了...