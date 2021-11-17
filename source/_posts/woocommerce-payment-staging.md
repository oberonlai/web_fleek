---
title: WooCommerce 金流串接實戰（十三）- 建立測試機
date: 2021-09-28 09:30:00
categories:
- WooCommerce
tags:
- WooCommerce 金流
- WordPress 外掛
- WooCommerce 外掛
- 自動化測試
- e2e
etoc: false
---

開發 WooCommerce 金流外掛能在本機測試的部分除了確保設定項有正確寫入資料庫外，剩下的就是確保按下結帳按鈕時不會有任何的錯誤產生，至於能否將正確的資料送到金流商以及成功接收回傳資料，都還是要在遠端主機才能進行測試，雖然我們可以用一些工具像是 Postman 來模擬金流商的 API，但最終這支外掛還是要能夠在正式環境中運作才行。

通常承接外掛開發這一類的案件，客戶本身都已經有運作中的網站，為了要讓我們的外掛可以在不影響客戶網站的情況下測試，最好的方法就是客戶可以提供給我們跟正式環境一模一樣的測試站來執行金流外掛的結帳流程。

但常遇到的狀況是客戶無法提供，這樣的話就需要我們自己架設測試環境，但自架的測試環境跟正式站還是會有所落差，所以主要測試的方向就是確保金流商的 API 可以運作正常，至於裝在正式站是否會跟其他外掛產生衝突或其他狀況而造成問題，這就只能等到上正式站才能知道了。

<!--more-->

通常我的開發流程如下：在本機完成外掛開發 -> 自動化測試 -> 開 VPS 測試機 -> 進入測試機繼續開發與除錯 -> 自動化測試 -> 人工測試 -> 部署到正式站

當本機開發完成第一輪後，接下進入測試機確保 API 的傳送與接收都正常，確認沒問題後再透過自動化工具部署到正式站，在自動部署的過程中會再跑一次測試，萬一上正式站後發生問題，就能手動還原部署前的狀態，接下來檢查造成問題的因素有哪些，再將這些因素搬移到測試機來進行重現並除錯。

最好的情況下是客戶允許我們把正式站的資料倒進測試機，但盡可能只搬移檔案的部分不要動到資料庫，要動到資料庫也排除跟使用者相關的資料表，以減少個資外洩的風險。

如果在測試機無法重現，我們勢必還是要進入正式機來除錯，為了避免顧客不小心選到我們正在測試中的金流商，可以加入一個只限定管理員才能看到該付款方式的功能，另外一定要準備好的就是日誌紀錄，WooCommerce 有提供日誌類別 ```WC_Logger```，使用之後就可以在後台的 WooCommerce > 狀態 > 日誌紀錄來印出訊息，千萬不要讓除錯的資訊直接顯示在前台。

設定好部署流程之後，接下來我們就來建立測試機，我習慣用 VPS 來建置，因為可以透過指令碼把我們需要的工具一次安裝完成，還記得之前我們用的本機開發環境 Valet 嗎？它有提供 Linux 的版本，這樣就能讓我們的本機與測試環境一樣。

但 Valet 的主要用途就是拿來本機開發用，雖然他有外連網址的功能，但因為沒辦法綁網域，因此也就無法安裝 SSL，有些金流商會要求回傳網址需要加密，這讓 Valet 不適合用在測試環境之中。

因此我們會採用一套叫做 WordOps 的 WordPress 環境，只要下一段指令就能一次安裝好 PHP、Nginx、MySql 以及 WordPress，同時也內建了 SSL 功能，再加上它將伺服器設定進行過最佳化，讓網站跑起來可以非常順暢，它可以直接作為正式環境來使用，因此拿來當測試環境再適合也不過。

除此之外，我們還需要以下工具：Git、Compose、Node.js，這些都能透過指令安裝，我們希望在安裝 VPS 時，只要輸入測試站的網址以及後台登入帳密，剩下的這些作業全部可以自動化搞定，為了達成這一點，首先我們必須先找可以使用指令工具來建立環境的 VPS，這邊示範使用 Linode 的服務。

## 安裝 linode-cli

linode-cli 是使用 Python 開發的，因此要確保本機環境有安裝，如果沒有的話可以使用 Homebrew 進行安裝：

```sh
$ brew install python3
```

接下來安裝 linode-cli：

```sh
$ pip install linode-cli
```

安裝完成後我們先把登入授權設定好：

```sh
$ linode-cli configure --token
```

它會請我們前往管理後台取得登入的 Token，照它的說明取得後輸入即可：

```sh
Welcome to the Linode CLI.  This will walk you through some initial setup.

First, we need a Personal Access Token.  To get one, please visit
https://cloud.linode.com/profile/tokens and click
"Create a Personal Access Token".  The CLI needs access to everything
on your account to work correctly.
Personal Access Token:
```

再來是設定測試機的機房位置與規格：

```sh
$ linode-cli configurate
```

我們使用東京機房，所以選擇 ap-norteast：

```sh
Default Region for operations.  Choices are:
 1 - ap-west
 2 - ca-central
 3 - ap-southeast
 4 - us-central
 5 - us-west
 6 - us-southeast
 7 - us-east
 8 - eu-west
 9 - ap-south
 10 - eu-central
 11 - ap-northeast

Default Region (Optional): 11
```


選擇最小規格的主機每月 5 美金：

```sh
Default Type of Linode to deploy.  Choices are:
 1 - g6-nanode-1
 2 - g6-standard-1
 3 - g6-standard-2
 4 - g6-standard-4
 5 - g6-standard-6
 6 - g6-standard-8
 7 - g6-standard-16
 8 - g6-standard-20
 9 - g6-standard-24
 10 - g6-standard-32
 11 - g7-highmem-1
 12 - g7-highmem-2
 13 - g7-highmem-4
 14 - g7-highmem-8
 15 - g7-highmem-16
 16 - g6-dedicated-2
 17 - g6-dedicated-4
 18 - g6-dedicated-8
 19 - g6-dedicated-16
 20 - g6-dedicated-32
 21 - g6-dedicated-48
 22 - g6-dedicated-50
 23 - g6-dedicated-56
 24 - g6-dedicated-64
 25 - g1-gpu-rtx6000-1
 26 - g1-gpu-rtx6000-2
 27 - g1-gpu-rtx6000-3
 28 - g1-gpu-rtx6000-4

Default Type of Linode (Optional): 1
```

預設的 OS 使用 linode/ubuntu20.04：

```sh
Default Image to deploy to new Linodes.  Choices are:
 1 - linode/almalinux8
 2 - linode/alpine3.10
 3 - linode/alpine3.11
 4 - linode/alpine3.12
 5 - linode/alpine3.13
 6 - linode/alpine3.14
 7 - linode/arch
 8 - linode/centos7
 9 - linode/centos8
 10 - linode/centos-stream8
 11 - linode/debian10
 12 - linode/debian11
 13 - linode/debian9
 14 - linode/fedora32
 15 - linode/fedora33
 16 - linode/fedora34
 17 - linode/gentoo
 18 - linode/debian9-kube-v1.19.11
 19 - linode/debian9-kube-v1.20.7
 20 - linode/debian9-kube-v1.21.1
 21 - linode/opensuse15.2
 22 - linode/opensuse15.3
 23 - linode/rocky8
 24 - linode/slackware14.2
 25 - linode/ubuntu16.04lts
 26 - linode/ubuntu18.04
 27 - linode/ubuntu20.04
 28 - linode/ubuntu20.10
 29 - linode/ubuntu21.04
 30 - linode/alpine3.9
 31 - linode/centos6.8
 32 - linode/debian8
 33 - linode/fedora31
 34 - linode/opensuse15.1
 35 - linode/slackware14.1

Default Image (Optional): 27
```

完成基本設定後，就能使用指令建立機器，後面的 123456 就是 root 權限登入密碼：

```sh
$ linode-cli linodes create --root_pass 123456
```

接下來會看到以下的主機資訊：

| id       | label          | region       | type        | image              | status       | ipv4            |
|----------|----------------|--------------|-------------|--------------------|--------------|-----------------|
| 12345678 | linode12345678 | ap-northeast | g6-nanode-1 | linode/ubuntu20.04 | provisioning | 123.123.123.123 |

在等待建立的過程中，我們可以先把自己電腦的 RSA key 傳上去，這樣 ssh 登入就不用輸入密碼了，如果沒有 key 的話先產生一個：

```sh
$ ssh-keygen
```

接下來複製到剛開好的主機中：

```sh
$ ssh-copy-id root@123.123.123.123
```

然後輸入一次 root 密碼即可，完成後會看到以下訊息：

```sh
Number of key(s) added:        1

Now try logging into the machine, with:   "ssh 'root@123.123.123.123'"
and check to make sure that only the key(s) you wanted were added.
```

下一步使用 ssh 登入主機並執行自動化指令安裝：

```sh
root@localhost:~ bash <(curl -s https://raw.githubusercontent.com/oberonlai/sh_staging/master/wordops.sh)
```

首先要回答六個問題，分別為：

- 測試站的網址，不用輸入 http/https
- 管理者登入帳號
- 管理者登入密碼
- 管理者電子郵件
- 外掛名稱
- 外掛的存放庫網址

```sh
1.Staging site url:
staging.example.com
2.Staging admin user:
user
3.Staging admin password:
pass
4.Staging admin email:
mail@mail.com
5.Plugin name:
my-plugin
6.Git clone url:
https://github.com/xxx
```

指令的內容如下：

```sh
# Get information
echo "1.Staging site url:"
read staging_url
echo "2.Staging admin user:"
read staging_user
echo "3.Staging admin password:"
read staging_pass
echo "4.Staging admin email:"
read staging_email
echo "5.Plugin name:"
read plugin_name
echo "6.Git clone url:"
read repo

# Set timezone
sudo timedatectl set-timezone Asia/Taipei

# Install WordOps
wget -qO wo wops.cc && sudo bash wo --force
wo stack upgrade --nginx
wo site create "$staging_url" --wp --user="$staging_user" --pass="$staging_pass" --email="$staging_email" --letsencrypt
cd /var/www/"$staging_url"/htdocs

# Install plugins and themes
wp language core install zh_TW --allow-root
wp language core activate zh_TW --allow-root
wp option update timezone_string "Asia/Taipei" --allow-root
wp site switch-language zh_TW --allow-root
wp plugin install woocommerce --allow-root
wp language plugin install woocommerce zh_TW --allow-root
wp plugin activate woocommerce zh_TW --allow-root
wp theme install storefront --allow-root
wp language theme install storefront zh_TW --allow-root
wp theme activate storefront --allow-root

# Install Tool
sudo apt update
sudo apt -y upgrade
sudo apt -y autoremove
sudo apt -y install git-all
sudo apt -y install nodejs npm
sudo apt -y install composer

cd wp-content/plugins
git clone "$repo"
cd "$plugin_name"
composer install
npm install
usermod --shell /bin/bash www-data
echo "Staging build successfully!"
```

首先設定時區以及 WordOps 的安裝，把我們輸入的資料帶進 WordOps 建立網站的指令中，然後將目錄切換到該站底下，使用 WP CLI 來處理網站的內容，因為我們現在是使用 root 權限在執行，所以要加上 --allow-root 的參數。

安裝完 WordPress 之後再把我們開發上需要的工具 Git、Node.js、Composer 透過 apt 來進行安裝，最後將目錄切換到測試站的 plugins 資料夾底下，把放在 Github 的金流外掛 clone 下來，並根據需要進行 composer 與 npm install，最後將網站使用者 www-data 加入權限，之後就能使用 www-data 來執行指令。

大家可以根據這個範本整理成自己需要的項目，整個安裝過程大概十分鐘，記得在執行之前要先把 DNS 指到這台主機，這樣 WordOps 才能正確加上憑證。

測試機安裝完成後，幫 VSCode 安裝 [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) 套件，這樣就可以在本地連線到測試機繼續開發，之後的版本控管也都可以在測試機上面進行，如果要回到本機測試的話，再 Pull 回來即可。

下一篇介紹自動部署到正式機的方法。


