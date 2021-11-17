---
title: WooCommerce 金流串接實戰（十四）- 部署正式機
date: 2021-09-29 09:30:00
categories:
- WooCommerce
tags:
- WooCommerce 金流
- WordPress 外掛
- WooCommerce 外掛
- 自動部署
- Buddy
etoc: false
---

當我們在測試機確認過金流功能皆能正常運作後，接下來就是要把我們開發的外掛上傳客戶主機的時候了，在上傳之前，為了避免發生預期外的錯誤，我會進行以下的自動化流程：

1. 檢查程式碼是否符合 WordPress Coding Standard 規範
2. 執行單元測試確保通過所有的測試項目
3. 打包外掛並排除不需要上傳到正式站的檔案
4. 上傳正式機
5. 當發生問題時可以還原到上傳前的狀態


第一個檢查項目是 WordPress 程式碼規範，PHP 有一套自己的程式碼規範，叫做 PHP Standards Recommendations ( PSR )，雖然 WordPress 是用 PHP 寫的，但在規範上有許多地方不一樣，這個檢查項目確保優先使用 WordPress 提供的函式。

<!--more-->

第二個檢查項目是單元測試，這部分我們在先前的文章有提及，事實上在邊開發時我們就會同時進行測試，這邊要檢查的就是全部測試都再跑一遍，確保之前已經通過的測試在所有功能開發完成後依舊可以過關。

第三個項目是打包需要上傳的檔案，在客戶的正式機上面我們不需要放單元測試以及做端對端測試的套件，另外還有一些快取資料夾，這部分我們可以先透過 .gitignore 來控管，然後等到打包的時候只要安裝必要的套件即可。

最後一個項目也是最重要的還原機制，在正式機上面一定會裝有很多外掛，所以當我們的外掛安裝上去時就非常有可能出現預期外的錯誤，所以一定要有還原的機制，確保在上傳之後萬一出問題可以恢復成正常的狀態，以免造成客戶的損失。

部署到正式機根據客戶情境有兩種方式：

## 透過第三方外掛部署

有時候客戶能夠提供的管理權限就只有後台帳密，可能是礙於安全性考量或是他們也不知道還有什麼其他帳密可以提供給我們，遇到這種情況就只能從後台來進行部署，雖然可以手動打包 zip 檔從後台進行外掛上傳，但步驟就會變得很繁瑣，這時候我們可以在正式機安裝 WP Pusher 這支免費外掛做自動化處理。

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-01.jpg)

它的邏輯是透過 webhook 來偵測是否有新的 commit 被 push，有的話就會自動從 Github clone 最新的一份下來，因此我們只要把 WP Pusher 安裝並設定好我們的存放庫，之後在每一次的 push 它都會幫我們把最新版的檔案自動 pull 回正式站，這樣就不用手動上傳壓縮檔。

具體的設定步驟如下：

1. 前往 https://wppusher.com 下載免費版本，付費版差別在於可以使用私有存放庫
2. 進入正式站後台的 > 外掛 > 安裝外掛，將下載回來的 ```wppusher.zip```上傳後並啟用
3. 找到後台主選單的 WP Pusher 並選擇 Github 頁籤，點選 Obtain a  Github token

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-02.jpg)

4. 取得 token 後貼回 WP Pusher 並點選 Save Github token
5. 進入 WP Pusher > Install Plugin 選單，輸入使用者＋存放庫的名稱，並勾選 Push-to-Deploy，按下 Install Plugin

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-03.jpg)

6. 安裝完成後就能在外掛列表以及 WP Pusher 的 Plugins 選單看到我們的外掛

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-04.jpg)

但這個方法有一個很大的壞處，那就是存放庫上面有什麼檔案它就會拉什麼檔案，它沒有辦法針對我們需求進行各別打包，也就是自動部署流程第三點提到的項目，譬如說在 vendor 資料夾裡面有正式環境不需要的套件像是 PHPUnit、PHPMock 等等，透過 WP Pusher 一併會被拉到正式機。

解決的辦法是從本機或是測試機 push 的時候，就先把整個 vendor 資料夾 ignore 掉，然後再用白名單的方式把我們需要的套件取消忽略，假設我們的 vendor 有以下套件，打 * 號的是要上正式機的：

```
vendor
├── a7 *
├── autoload.php *
├── oberonlai *
├── php-mock
└── phpunit
```

gitignore 要先把整個 vendor 資料夾忽略掉，然後使用驚嘆號把正式機需要用的套件加入：

```
vendor/*
!vendor/a7
!vendor/oberonlai
!vendor/autoload.php
```

但這樣做還是會很麻煩，因為當正式環境需要的套件有相依套件時我們可能不會知道，如果沒有把相依套件也加入白名單的話一樣會噴錯，因此最好的方法還是可以取得客戶網站的 SSH 或是 SFTP 帳密，如果沒有的話至少也要有 FTP 才能使用部署工具來打包所需套件。


## 透過自動化服務進行整合與部署

如果很幸運可以取得客戶的 FTP 或是主機的遠端登入帳密，那麼在做整合與部署會方便非常多，這類的服務不少，WordPress 官方文件推薦的是 Travis CI，而 Github 本身也有 Actions 可以使用，其基本邏輯是使用一個 yml 設定檔來設定該做哪些流程。

而本文要介紹的是一套視覺化的部署工具 [Buddy](https://buddy.works)，它能省去編寫設定檔的時間，直接使用所見即所得的介面來決定部署前的流程，它還整合許多常見的第三方服務，以下就為具體的設定步驟

### 1.建立 Project 與 pipeline

Buddy 是以 Project 為單位，一個 Project 會綁定一個存放庫，它的免費方案可以使用五個 Project，對於個人接案者來說算是非常夠用。先前往 Buddy 進行申請，可以直接使用 Github 註冊：https://buddy.works/sign-up

註冊完成後可以看到 Project 列表，點擊右上方的 Create a new project 即可建立新專案。

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-05.jpg)

可以看到已經 Buddy 已經與我們的 Github 做綁定，直接從中間的 Repository 搜尋找到特定存放庫，並點選存放庫即可完成 Project 的新增。

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-06.jpg)

做好 Project 與存放庫的綁定之後，接下來就是要建立 pipeline，pipeline 指的就是部署前的流程，以本文的情境來說，就是程式碼規範檢查 > 單元測試 > 打包 > 上傳正式機這四個步驟，現在先點選中間的 Add a new pipeline 按鈕：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-07.jpg)

就能看到 pipeline 的設定畫面，先幫這個 pipeline 取名，然後設定要觸發它的時間，有三種模式：手動、push 跟排程，另外 pipeline 可以新增多個，也就是說在不同的觸發條件下我們可以設定不同的自動化工作。像是在開發的時候我們會需要在 push 時跑程式碼規範檢查，或是可以在每天半夜十二點自動做測試。

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-08.jpg)

### 2.檢查程式碼規範

接下我們會看到一大堆的 action，也就是我們要執行的環境或工作，我們先新增第一個 PHP CodeSniffer 來做程式碼規範檢查：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-09.jpg)

點進去後會看一個指令視窗，這個 action 它是一個 Docker 環境，裡面已經幫我們把 PHP 跟 CodeSniffer 都安裝好了，所以我們只要下載並設定 WordPress 的 Coding Standard 即可，輸入以下指令：

```sh
rm -rf wpcs
git clone -b master https://github.com/WordPress/WordPress-Coding-Standards.git wpcs
phpcs --config-set installed_paths /buddy/iron-pay/wpcs
phpcs --standard=WordPress /buddy/iron-pay/src
```

這邊我們先把 /buddy 這個目錄的 wpcs 資料夾刪除，然後從 Github 下載 WordPress 規範，然後設定路徑並指定 phpcs 用 WordPress 來檢查存放庫裡面的 src 資料夾也就是我們外掛的核心檔案，這邊的存放庫路徑記得都要用 ```/buddy/iron-pay/```，因為這是在 Docker 裡面，Buddy 會把存放庫都放在這個目錄之中。

寫好指令後點選 Save this action：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-10.jpg)

第一個工作設定好之後我們就可以點選右上角的 Run pipeline 來檢查第一關是否有通過：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-11.jpg)

執行前要先設定這次測試任務的對應的 commit，這樣之後有問題才能進行還原：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-12.jpg)

執行中的畫面：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-13.jpg)

成功的話會顯示綠色，失敗的話是，點選 Logs 可以看到執行的細節：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-14.jpg)

### 3.執行單元測試

回到 pipeline，繼續來新增單元測試，點選 Actions 並點擊下方的加號來新增：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-15.jpg)

選擇 PHP：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-16.jpg)

將 vendor/bin/phpunit 的註解取消，並點選 Add this aciton：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-17.jpg)

再來執行一次 pipeline，會多出單元測試的 Action，並透過測試：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-18.jpg)

### 4.打包部署的檔案

步驟跟上一步差不多，一樣是使用 PHP 的 Action，然後指令換成 ```composer install --no-dev```，這樣就不會把 PHPUnit 這種測試環境用的套件一併打包進去，也不用再寫 gitignore 白名單來處理。

### 5. 部署正式機

要用什麼方式部署就看我們能夠取得正式機的哪種帳密，Buddy 有提供四種：FTP、FTPS、SFTP 跟 RSync，這邊先選擇使用 SFTP 的 Action 來部署。進入 SFTP 的設定頁面時可以看到需要輸入相關的帳密資訊以及 Source 來源，這邊記得要選擇 Pipeline Filesystem 才會是抓到有經過 ```composer install --no-dev``` 安裝的存放庫：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-19.jpg)

設定好之後，我們的 pipeline 長得如下圖：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-20.jpg)

最後按下 Run pipeline 就能享受一鍵自動部署的樂趣了XD，但萬一發生意外，我們可以回到 pipeline 的 Executions 頁籤，選擇要還原的動作，這樣就能確保有問題時可以快速還原：

![](https://oberonlai.blog/wp-content/uploads/wordpress-deploy/wordpress-deploy-21.jpg)

Buddy 可以做到非常多的功能，像是部署完成後的 Slack 通知、打包 Zip、Cloudflare、GCP、AWS 整合等等一堆的動作，萬一都沒有也可以自己寫指令來跑，另外它還可以把 pipeline 存成範本，之後有新 Project 時直接套用即可。

介紹完自動部署之後，下一篇會說明如何自管 WordPress 外掛的更新，以及當要販售外掛時該如何導入序號機制並整合 WooCommerce 來進行控管。