---
title: Media with FTP 外掛開發
tags: []
id: '281'
categories:
  - - WordPress
date: 2019-07-27 10:20:12
---

第一次練習開發 WordPress 外掛，心得紀錄如下：

# Goal

1.  提交 github 修改
2.  整合 cmb2 到 plugin
3.  開後台設定欄位
4.  上架 [wordpress.org](http://wordpress.org/) svn

## 如何修改別人 Repo 的流程

先 fork 到自己的帳號，clone 到本機，remote 增加原始專案的 upstream，然後開分支處理，本機開發隨時 pull upstream，然後 push 到分支，完成後再開 pull request

```
$ git clone myfork

$ git remote add upstream <others repo>

$ git checkout -b <branch name>

$ git commit -am "message"

$ git pull upstream <branch name>

$ git push origin <branch name>
```

## CMB2 的一些雷

*   放在 theme or plugin 的資料夾直接引入if ( file\_exists( dirname( **FILE**) . '/cmb2/init.php' ) ) { require\_once dirname( **FILE**) . '/cmb2/init.php'; } elseif ( file\_exists( dirname( **FILE**) . '/CMB2/init.php' ) ) { require\_once dirname( **FILE**) . '/CMB2/init.php'; }
*   取得 option page 的值使用 cmb2\_get\_option，至少要帶兩個參數 key 與 idecho cmb2\_get\_option('wfm\_options','wfm\_port')

cmb2 API → [https://cmb2.io/api/index.html](https://cmb2.io/api/index.html)

## 本地化外掛流程

*   主程式註解宣告的地方要有 Text domain 跟 Domain Path/\* Plugin Name: Wp-ftp-media-library Plugin URI: [http://wordpress.stackexchange.com/questions/74180/upload-images-to-remote-server](http://wordpress.stackexchange.com/questions/74180/upload-images-to-remote-server)Description: Let's you upload images to ftp-server and remove the upload on the local machine. Version: 0.1 Author: Pontus Abrahamsson Author URI: [http://pontusab.se](http://pontusab.se/)Text Domain: wp-ftp-media-library Domain Path: /languages \*/
*   用 wp cli 建立 pot 檔，進到外掛目錄，跑下面這段 cli，在寫 \_\_e 的時候第二個參數 text domain 不能用變數，要寫死才行，不然 wp cli 會爬不到需要翻譯的字串$ wp i18n make-pot . languages/my-plugin.pot
*   mo 檔一定要跟 Text Domain 一樣，{text-domain}-{locale}.mo
*   外掛主程式載入翻譯檔function wfm\_load\_text\_domain() { load\_plugin\_textdomain( 'wp-ftp-media-library', false , basename( dirname( **FILE**) ) . '/languages' ); } add\_action('plugins\_loaded', 'wfm\_load\_text\_domain');

## FTP 刪除遠端圖片

*   使用 ftp\_delete，使用流程一樣先 connect 再 login，然後利用 wp\_get\_attachment\_url 取得原始圖路徑，再用 wp\_get\_attachment\_metadata 取得縮圖路徑，hook 用 delete\_attachment
*   開發外掛時檢查變數值的小技巧，用 error\_log 把變數印在 error.log 訊息裡面
*   較大張的圖片會產生一張 wp\_get\_attachment\_metadat 抓不到的縮圖圖片，

```
$file_year = substr(wp_get_attachment_metadata($args)['file'],0,8); $file_original = str_replace($settings['cdn'].'/',"",wp_get_attachment_url($args)); $file_thumb = $file_year.wp_get_attachment_metadata($args)['sizes']['thumbnail']['file']; $file_medium = $file_year.wp_get_attachment_metadata($args)['sizes']['medium']['file']; $file_large = $file_year.wp_get_attachment_metadata($args)['sizes']['large']['file']; $file_post = $file_year.wp_get_attachment_metadata($args)['sizes']['post-thumbnail']['file']; error_log($file_year, 0); ftp_delete($connection,$file_original); ftp_delete($connection,$file_thumb); ftp_delete($connection,$file_medium); ftp_delete($connection,$file_large); ftp_delete($connection,$file_post); ftp_close($connection);
```

## 外掛上架流程

*   外掛命名
*   header comment
*   text domain 命名
*   readme.txt

[如何发布插件到 WordPress 官方插件站](https://blog.wpjam.com/article/listing-your-plugin-at-the-wordpressorg-plugin-directory/)

## Composer 安裝管理

*   使用 composer init 初始化
*   composer install 安裝套件
*   要引入別人寫的套件寫在 compser.json 裡面的 require 定義，然後 require\_once vendor/autoload.php