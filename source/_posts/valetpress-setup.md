---
title: 【 工具 】Mac 限定 WordPress 本機開發環境 Valet + ValetPress
tags: []
id: '675'
categories:
  - - WordPress
date: 2020-04-02 16:30:00
etoc: true
---

## 前言

自從在 2018 年 WordCamp Taipei 分享使用 [Docker + Kusanagi](https://2018.taipei.wordcamp.org/files/2018/10/how-to-use-the-docker-for-the-wordpress-localhost-environment-compressed.pdf) 來搭建 WordPress 本機開發環境之後，我的小不點 MBP 硬碟就被 Docker 的 images 跟 volume 給塞爆了， 這才知道時間一久 Docker 超吃容量@@

後來就全部改用 [Local](https://localwp.com) ( Localhost by Flywheel )，用了幾個月不管是安裝還是效能上都很滿意，但某天發現硬碟又要爆了，就用 CleanMyMac 把一些大檔砍一砍，不砍還好，一砍發現我用 Local 開的站全都 GG...

追查下才發現我誤砍了 Local 要用的 VM，仔細看每個站都有獨立的 PHP、Nginx、MySql，所以 VM 超肥，只能乖乖的把已經做的差不多的案子先移掉，只留下正在進行中的 VM。
<!--more-->
## 什麼是 Valet？

![](https://oberonlai.blog/wp-content/uploads/2020/02/xka1Yj-1024x576.jpg)

Valet 的中文是男僕，讓我想到黑執事這部漫畫，然後又是真人版崩壞...

在 2019 年底聽到這一集 [Podcast](https://yourwebsiteengineer.com/475-2020-version-apps-i-use-daily/) ( 很不錯的 WordPress Podcast，大推！)，Dustin Hartzler 分享了他的 WordPress 開發環境是一套叫做 [Valet](https://github.com/weprovide/valet-plus) [Plus](https://github.com/weprovide/valet-plus) 的工具，Valet 是 PHP 框架 Laravel 推薦的 Mac 開發環境，很好的整合了 Nginx / PHP / MySql，然後重點是下面這句：

> In other words, a blazing fast Laravel development environment that uses roughly 7 MB of RAM. 

對記憶體&硬碟小資族的我來說眼睛立馬為之一亮，它利用已經裝在 Mac 裡面的東西來生出本機網站來，所以再也不用像 Docker 或是 Local 一樣，只要一套 Nginx / PHP / MySql 就可以搞定，這超級省空間啊！

雖然這樣就無法針對不同站提供不同的環境，但如果真的要特別的環境就用 Local 開也是可以的，另外一個好處是他是用 [DnsMasq](https://en.wikipedia.org/wiki/Dnsmasq) 來管理做網址對應，就不用 Host 檔在那邊改半天，

綜合上述理由， Valet 立馬成為我躍躍欲試的開發環境！

## Valet、Valet Plus、ValetPress、ValetXXX

最後一個是我亂掰的XD，研究下去發現有很多不同版本的 Valet，後來找到這一篇：[Install WordPress locally in under 5 seconds with ValetPress](http://aaronrutley.com/install-wordpress-in-under-5-seconds-with-valetpress/)，號稱五秒就可以完成安裝一個新的 WordPress 網站，

> vp empty to create a empty WP site with the 2016 theme (4.7 seconds)  
> vp create to create a WP site from your starter git repo (~28 seconds)

ValetPress 方便之處在於作者 Aaron Rutley 把一些常用的流程都寫好指令了，像是下載 WP、建 DB、設定帳號密碼等等，要改寫成自己的也非常容易，比照辦理就行，最常用的指令是 vp create，也就是新增一個站，輸入後就會用問答式的介面來引導你，

他的指令內容也非常淺顯易懂，節錄如下：

if \[ $1 = "create" \]; then
cd ~/Sites
echo "${VP\_BOLD}ValetPress, create new site ${VP\_NONE}"
echo "${VP\_YELLOW}Project Name:${VP\_NONE}"
read project\_name
mkdir "$project\_name"
cd "$project\_name"
mysql -uroot -e "create database \\\`$project\_name\\\`"
wp core download
wp core config --dbname="$project\_name" --dbuser=root --dbhost=127.0.0.1
wp core install --url="$project\_name".dev --title="$project\_name".dev --admin\_user="$project\_name"  --admin\_password=password --admin\_email=wordpress@wordpress.org
rm -rf wp-content
git clone https://aaronrutley@bitbucket.org/aaronrutley/turbo-light.git wp-content
cd wp-content
rm -rf .git
git init
eval mv ~/Sites/"$project\_name"/wp-content/themes/turbo-light ~/Sites/"$project\_name"/wp-content/themes/"$project\_name"
wp theme activate "$project\_name"
wp plugin activate advanced-custom-fields-pro minimal-admin bulk-page-creator simple-page-ordering
wp plugin update --all
echo "${VP\_GREEN}Success:${VP\_NONE} Project Created: ${VP\_UNDERLINE}http://$project\_name.dev/${VP\_NONE}"
echo "L: ${VP\_UNDERLINE}http://$project\_name.dev/wp-admin/${VP\_NONE}"
echo "U: $project\_name"
echo "P: password"
echo "${VP\_GREEN}Success:${VP\_NONE} Running NPM install"
cd ~/Sites/"$project\_name"/wp-content/themes/"$project\_name"
npm install
echo "${VP\_GREEN}Success:${VP\_NONE} Running Gulp (default)"
gulp
fi

關鍵是 git clone 那邊，你可以把你自己常用的 theme、plugin 整理成一包 wp-content 丟到 GitHub 或是其他版控服務上，然後下面他幫你用 WP CLI 啟用該啟用的東西、以及順便處理更新。

如果有用 NPM 還會自動 npm install，簡單說就是可以把平常自己的工作流程全都用這一支來進行自動化，這樣要建新的開發專案時只要下 vp create 之後就可以去泡茶了～

## ValetPress 安裝步驟

相較於 Local，用指令安裝東西好卡關，主要是 MySql 的部分一直起不來，還有最後 valet 的部分要 link 才可以正確連線，詳細安裝步驟如下：

### 一、安裝 Homebrew

[Homebrew](https://brew.sh/index_zh-tw) 是 MacOS 專用的套件管理系統，開啟終端機，輸入以下指令即可安裝

$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

安裝完成後，先確認 brew 可以正確執行，

$ brew update

如果執行 brew update 發生錯誤，要先把別名加入 .bash\_profile，

$ open .bash\_profile

輸入上面的指令後會打開一個文字檔，在檔案的最下面加入以下文字：

> export PATH="/usr/local/bin:$PATH"

確認 brew update 可以正確執行後就可以進入到下一步了！

### 二、安裝 PHP&Composer

先使用 Homebrew 安裝 PHP，再安裝 PHP 套件管理系統 Composer：

$ brew install php

$ brew install composer

### 三、安裝 MySql

Valet 官方文件是安裝 MySql 5.7，個人是習慣裝 Mariadb：

$ brew install mariadb

狀況來了，當我要 mysql -u root 的時候，會出現時 **ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)，**

爬了一堆解，試了一堆方法都還是行不通，於是找到出問題的 /tmp/mysql.sock 這個檔案，發現他只是一個連結並且沒有連到任何內容，所以我就把他砍了(?)，

再次登入 mysql -u root 出現 **'Access denied for user 'root'@'localhost'**，查了之後說是權限不夠，可以用 sudo 來執行，但 ValetPress 的腳本我試過加 sudo 還是過不了，最後找到另一個解法：前往編輯 vi /usr/local/etc/my.cnf，加入下面這行後再重新啟動 Mariadb，才有辦法成功連線。

\[mysqld\]
skip-grant-tables

$ brew services restart MariaDB

### 四、安裝 Valet

使用 composer 下載 valet 套件：

$ composer global require laravel/valet

下載依賴套件中...

![](https://oberonlai.blog/wp-content/uploads/2020/02/CleanShot-2020-02-07-at-11.23.07.jpg)

上面那一堆跑完後開始安裝 Valet：

$ valet install

這時候出現 **zsh: command not found: valet** 的錯誤訊息，要讓 valet 可以全域使用，必須要 export 才行，我的環境是用 zsh，所以要打開 .zshrc 來加入設定：

$ open ~/.zshrc

系統會開啟一個文字檔，然後加入下方文字：

export PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/user/.composer/vendor/bin:$PATH

.zshrc 有任何修改記得要跑一次 source 才會套用更新：

$ source ~/.zshrc

加入後再次 valet install，會出現 **stopping Nginx** 的訊息，代表正在啟用 valet 中，但如果出現 **Homebrew PHP appears not to be linked.** 代表 Homebrew 沒有連結到正確的 PHP，輸入以下指令來產生連結：

$ brew link php --force

再次輸入 valet install 即可啟用 Valet。Valet 預設情況下它會在背景執行，所以如果要連上測試站的話，再也不用像以前啟用 MAMP、Docker、Local 要等上一段時間，只要一開機 Valet 就會自動啟用。

### 五、安裝 WP-CLI

[WP CLI](https://wp-cli.org/#installing) 是超強大又實用的指令工具，可以從系統底層對 WordPress 上下其手，可以參考官方的安裝方式也可以使用 Homebrew 來安裝：

$ brew install wp-cli

裝完後記得把 wp 關鍵字加入到個人設定檔中，這樣就可以用 wp --info 這樣的簡寫來操作它。

$ chmod +x wp-cli.phar
$ sudo mv wp-cli.phar /usr/local/bin/wp

### 六、下載 ValetPress

上面環境都完成後，主角 ValetPress 要登場了 。先在本機 user 目錄新開一個隱藏目錄叫做「 .valetpress」 與一般目錄 「Sites」 資料夾，前者是放 ValetPress 自動化腳本，後者是之後新建 WordPress 站的網站目錄。

[下載 ValetPress](https://github.com/AaronRutley/valetpress/archive/master.zip) [原始](https://github.com/AaronRutley/valetpress/archive/master.zip)[檔](https://github.com/AaronRutley/valetpress/archive/master.zip)，解壓縮後放入這個 /yourmacusername/.valetpress 這個目錄中，然後 .zshrc 要引入這個 valetpress 這個檔案，先打開 zsh 設定檔

$ open ~/.zshrc

加入以下判斷式：

if \[ -f ~/.valetpress/valetpress \]; then
    source ~/.valetpress/valetpress
else
    print "404: ~/.valetpress/valetpress not found."
fi

一樣，設定檔改好後記得要跑 source 才會更新，如果你有自己改寫 valetpress 也記得要 soruce 一下：

$ source ~/.zshrc

### 七、打完收工

輸入指令以及專案名稱，接下來可以就享受秒速建置本機環境的爽快感啦～但前提是要先記得把你的 repo 換上，ValetPress 內建的是 Private Repo。

$ vp create

輸入後會出現問答式的安裝介面，安裝完成後，正當以為一切都已經搞定，結果連上測試網站的網址一直給我 **404 not found**...

查了之後才知道要做 link：先把目錄切換到 Sites/projectname，然後 valet link projectname，然後 valet restart 即可，這一步驟我自己是已經把它寫在 Valetpress 裡面了，另外還有網站加密，加入以下腳本：

echo "Valet Link Project"
cd ~/Sites/"$project\_name"/
valet link $project\_name
valet secure $project\_name

在做 valet link 的時候會需要入 Mac 使用者密碼，輸入後就會自動完成連結，這樣就大功告成啦～

## 心得

目前使用 ValetPress 在兩個專案中，還沒有發生什麼狀況，網站整體速度也不錯，最感動的是硬碟沒有日益增肥，希望它可以陪我長長久久，不想再為了本機環境燒腦了 Orz...