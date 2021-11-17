---
title: Wordpress 搬家注意事項
tags: []
id: '1252'
categories:
  - - WordPress
date: 2014-03-17 12:52:18
---

1.網站蓋好後注意事項 網頁描述換成 meta descrption 網頁描述換成文章摘要 設定 og tag 設定 favicon [http://www.favicon.cc/](http://www.favicon.cc/) 裝 w3 total cache
<!-- more -->
  2.檔案備份注意事項 sql 檔案匯出前要先最佳化資料表 網頁檔案確保為最新版本 匯出 menu editor 資料 移除 .htaccess   3.環境作業 對方環境請安裝 wamp 請提供資料庫名稱 資料庫 user 帳密 安裝 teamviewer 提供 mail server smtp host、port、帳密   4.移動作業 sql 檔案要小於 2mb，用 zip 壓縮檔處理 如果檔案大於 2mb，修改--->[http://bin.17ball.net/2013/08/phpmyadmin.html](http://bin.17ball.net/2013/08/phpmyadmin.html) sql 檔案路徑直接修改為客戶家網址 搬移網站檔案   5.檔案修改作業 修改 wp-config.php 資料庫資訊   6.apache 設定 設定固定網址需啟用 rewrite service 啟用 mod\_rewrite httpd conf AllowOverride None--->AllowOverride All httpd conf Deny from all --->Allow from all   7.網站設定 wp smtp 設定 upload 的資料夾位置要修改 網頁標題設定 客戶 email 設定   8.權限設定 詢問管理員角色與後臺使用權限，先行設定完成   9.上線後作業 增加 GA 帳號 產生 sitemap [http://www.xml-sitemaps.com/](http://www.xml-sitemaps.com/) 設定 google、bing 網站管理員工具 登入 google 搜尋 [https://www.google.com/webmasters/tools/submit-url?hl=zh\_TW](https://www.google.com/webmasters/tools/submit-url?hl=zh_TW) 登入 yahoo&bing [http://www.bing.com/toolbox/submit-site-url](http://www.bing.com/toolbox/submit-site-url) 加入客戶權限