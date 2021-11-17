---
title: WooCommerce 金流串接實戰（二）- 開發環境
date: 2021-09-17 09:00:00
categories:
- WooCommerce
tags:
- WooCommerce 金流
- WordPress 外掛
- WooCommerce 外掛
etoc: false
---

在自己的電腦中建立一個 WordPress 需要的材料有 PHP、Apache or Nginx 以及 MySql，如果不想手動各別安裝，市面上也有很多現成的免費套裝軟體可以把這些環境自動建置起來，而且還可以根據自己的需要切換不同的版本，然後只要下載 WordPress、伺服器設定、資料庫連線等一系列步驟，就能在本機架設好 WordPress 網站。

由於這幾年全代管 WordPress 網站主機的服務盛行，這些廠商為了增加市占率，提供給開發者免費的軟體來建置本機環境，除了已經內建基本服務外，最方便的是一鍵就能建立網站，所有的伺服器、資料庫連線設定它都已經幫我們處理好，然後當做完後點選部署，直接就能把網站上傳到他們的主機上，省去所有部署的繁瑣流程，關鍵字搜尋「local wordpress development」就能找到很多這類軟體。

<!--more-->

這些套裝軟體的好處很多，除了上述提到的一鍵架好 WordPress 以外，還有提供網站複製、對外分享連結、WP CLI、預設安裝佈景主題、測試發信等等實用的功能，而且 Windows /  MacOS 都有支援，最重要的是完全免費，如果是想快速架設的朋友我建議可以先用這些工具來熟悉 WordPress 的操作。

雖然上述軟體的優點很多，但有一個致命的缺點就是很吃硬碟空間，尤其是對我這台只有 128G 的 Mac 來說，因為這些軟體多半都是用 Docker 來配置每一個站的環境，等於每一個站就會有獨立的 PHP、Nginx、MySql，更不用說如果還要切換版本的話，每個版本都是獨立的映像檔，網站一多佔用的硬碟空間就會暴增。

另外一個缺點是因為是執行在 Docker 上面，這些軟體為了避免長時間執行吃記憶體，所以只會在啟用時才把環境建置起來，這導致了每次重啟的時間會稍微久一點，以及有時候網站的效能會低落，但如果你的工作機硬碟有 1TB、RAM 有 32G，那麼這些工具可以幫你省下很多時間。


## Laravel Valet

如果沒有，而且你的工作環境是 MacOS，我會推薦使用 Valet。Valet 是 PHP 框架 Laravel 提供給 Mac 使用者的本機開發環境，它最大的好處是共用背景執行的 Nginx，只要當電腦開機時就會同步啟用，也不會像套裝軟體每建一個站就會有獨立的 Nginx 要安裝與執行。另外它使用了 DnsMasq 這套 DNS 軟體，無需再自行修改 host 檔來新增測試站網址。

官網上面提到 Valet 執行時大約只會站 7MB 的記憶體，而且速度非常快，我這幾年來已經全面改用 Valet 建過數百個站，我的配備是 2013 MBP 128G SSD、8GB RAM，到現在還每個站跑起來還是跟飛的一樣，完全不會有越用越慢的問題。

Valet 除了可以跑 Laravel，當然也能作為 WordPress 的開發環境，開發者 Aaron Rutley 寫了一份名為 valetpress 的指令稿，透過 WordPress 命令列工具 WP CLI 來完成環境配置，從敲下第一行指令到真正建立好網站，我實測的平均秒數是 6 秒多，完勝所有用套裝軟體的速度。

另一個好處是可以修改腳本的內容，改成符合自己需要的，像是我常常需要空的 WooCommerce 環境來進行測試，我只要在終端機裡面輸入 ```$vp wc```，就能在十秒內建置好裝有 Storefront 佈景主題、倒入測試資料、WooCommerce 以及常用金流外掛的測試站。


## Valet 安裝步驟

先確保作業系統必須是 MacOS，我目前的版本是 10.15.7，CPU 是 Intel Core i5：


### 一、安裝 Homebrew

Homebrew 是 MacOS 專用的套件管理系統，開啟終端機，輸入以下指令即可安裝

```sh
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

安裝完成後，先確認 brew 可以正確執行，

```sh
$ brew update
```

如果執行 brew update 發生錯誤，要先把別名加入 .bash_profile，

```sh
$ open .bash_profile
```

輸入上面的指令後會打開一個文字檔，在檔案的最下面加入以下文字：

```
export PATH=”/usr/local/bin:$PATH”
```

確認 brew update 可以正確執行後就可以進入到下一步驟。


### 二、安裝 PHP&Composer

先使用 Homebrew 安裝 PHP，再安裝 PHP 套件管理系統 Composer：

```sh
$ brew install php
```

```sh
$ brew install composer
```

### 三、安裝 MySql

Valet 官方文件是安裝 MySql 5.7，個人是習慣裝 Mariadb：

```sh
$ brew install mariadb
```

如果遇到要登入資料庫的時候，會出現 **'ERROR 2002 (HY000): Can’t connect to local MySQL server through socket '/tmp/mysql.sock' (2)**，時，先進到該路徑把這檔案刪除，

如果再次登入出現 **'Access denied for user 'root'@'localhost'**，前往編輯 MySql 設定：

```sh
$ vi /usr/local/etc/my.cnf
```

加入下面這行後儲存後退出：

```
[mysqld]  
skip-grant-tables
```

最後再重啟資料庫即可。

```sh
$ brew services restart MariaDB
```

### 四、安裝 Valet

使用 composer 下載 valet 套件：

```sh
$ composer global require laravel/valet
```

等待下載完成後開始執行安裝：

```sh
$ valet install
```

這時候可能會出現找不到 valet 指令的錯誤訊息：**command not found: valet**，因為 valet 是 composer 的套件，而套件會被裝在 vendor 資料夾底下，因此沒辦法直接呼叫，這時候要把 vendor 路徑進行 export。我的環境是用 zsh，所以要打開 .zshrc 來加入設定：

```sh
$ open ~/.zshrc
```

然後加入下方路徑後儲存：

```sh
export PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Users/user/.composer/vendor/bin:$PATH
```

設定檔有任何修改記得要跑一次 source 才會套用更新：

```sh
$ source ~/.zshrc
```

加入後再次 valet install，會出現 **stopping Nginx** 的訊息，代表正在啟用 valet 中，但如果出現 **Homebrew PHP appears not to be linked.** 代表 Homebrew 沒有連結到正確的 PHP，輸入以下指令來產生連結：

```sh
$ brew link php –force
```

再次輸入 valet install 即可啟用 Valet。Valet 預設情況下它會在背景執行，所以如果要連上測試站的話，再也不用像以前啟用 MAMP、Docker、Local 要等上一段時間，只要一開機 Valet 就會自動啟用。

### 五、安裝 WP-CLI

WP CLI 是 WordPress 非常實用的指令工具，可以從系統面來管理網站，不管是安裝外掛或是匯入資料庫都能透過指令操作，安裝方式可以參考官方的說明，另外我們也可以使用 Homebrew 來安裝：

```sh
$ brew install wp-cli
```

裝完後記得把 wp 關鍵字加入到個人設定檔中，這樣就可以用 wp –info 這樣的簡寫來操作它。

```sh
$ chmod +x wp-cli.phar  
$ sudo mv wp-cli.phar /usr/local/bin/wp
```

### 六、下載 ValetPress

上面環境都完成後，剩下就是加入 WordPress 的自動化腳本。先在本機 user 目錄新開一個隱藏目錄叫做「 .valetpress」 與目錄 「Sites」 資料夾，前者是放自動化腳本，後者是之後新建 WordPress 站的網站根目錄。

我依照 Aaron Rutley 所寫的腳本新增了與 WooCommerce 相關的部分，下載連結如下：

[下載 ValetPress - WooCommerce 版](https://github.com/oberonlai/.valetpress) 

解壓縮後放入這個 /username/.valetpress 這個目錄中，然後 .zshrc 要引入這個 valetpress 這個檔案，先打開 zsh 設定檔

```sh
$ open ~/.zshrc
```

加入以下判斷式：

```sh
if [ -f ~/.valetpress/valetpress ]; then  
	source ~/.valetpress/valetpress  
else  
	print “404: ~/.valetpress/valetpress not found.”  
fi
```

一樣，設定檔改好後記得要跑 source 才會更新，如果你有自己改寫 valetpress 也記得要 soruce 一下：

```sh
$ source ~/.zshrc
```

我新增的指令如下：

```sh
if [ $1 = "wc" ]; then
	cd ~/Sites
	echo "${VP_BOLD}ValetPress, create empty new site with WooCommerce${VP_NONE}"
	echo "${VP_YELLOW}Project Name:${VP_NONE}"
	read project_name
	mkdir "$project_name"
	cd "$project_name"
	mysql -uroot -e "create database \`$project_name\`"
	wp core download --locale=zh_TW --skip-content
	wp core config --dbname="$project_name" --dbuser=root --dbhost=127.0.0.1
	wp core install --url="$project_name".test --title="$project_name".test --admin_user="$project_name"  --admin_password=password --admin_email=wordpress@wordpress.org
	wp language core install zh_TW
	wp language core activate zh_TW
	wp option update timezone_string "Asia/Taipei"
	wp site switch-language zh_TW
	echo "${VP_GREEN}Success:${VP_NONE} Project Created: ${VP_UNDERLINE}https://$project_name.test/${VP_NONE}"
	wp plugin install woocommerce
	wp language plugin install woocommerce zh_TW
	wp plugin install ry-woocommerce-tools
	wp language plugin install ry-woocommerce-tools zh_TW
	wp plugin activate woocommerce ry-woocommerce-tools
	wp theme install storefront
	wp language theme install storefront zh_TW
	wp theme activate storefront
	echo "Valet Link Project"
	cd ~/Sites/"$project_name"/
	valet link $project_name
	valet secure $project_name
	echo "L: ${VP_UNDERLINE}https://$project_name.test/wp-admin/${VP_NONE}"
	echo "U: $project_name"
	echo "P: password"
fi
```

裡面大部分都是 WP CLI 的指令，像是安裝中文語系檔、安裝 WooCommerce 外掛、安裝 Storefront 佈景主題並且啟用它們，然後使用 valet link 來做連結，以及 secure 來做 SSL，預設的帳密為專案名稱 / password，如果要換登入密碼可以在 wp core install 那邊修改。

之後新建一個裝有 WooCommerce 的測試站步驟如下：

```sh
$ vp wc
```

輸入專案名稱：

```sh
ValetPress, create empty new site with WooCommerce
Project Name:
wc-test
```

接下就會全自動化進行安裝：

```sh
wc-test
Downloading WordPress 5.8 (zh_TW)...
Using cached file '/Users/oberonlai/.wp-cli/cache/core/wordpress-5.8-zh_TW.zip'...
Success: WordPress downloaded.
Success: Generated 'wp-config.php' file.
Success: WordPress installed successfully.
...
```

完成後就可以用專案名稱的網址來進入測試站，如果要砍掉測試站只要輸入 ```$vp delete``` 即可，刪除的同時資料庫也會一併移除，無須再手動處理。

搞定開發環境後，下一篇文章介紹 WordPress 的資料夾基本結構，以及建立金流外掛的必備知識。




