---
title: Plesk 下找出大容量資料夾或檔案
tags: []
id: '183'
categories:
  - - Host
date: 2019-02-27 14:32:10
---

![](https://i2.wp.com/oberonlai.blog/wp-content/uploads/2019/02/Carbonize-2019-02-27-at-2.15.52-PM.png?fit=1024%2C429&ssl=1)

主機硬碟的空間超過一半了，沒足夠容量做整台主機的全量備份，只好手動找出是哪些大檔再吃硬碟，幾個關鍵容易肥大的資料夾先掃一掃。

第一個是 /tmp 資料夾，如果是用 Plesk 的備份套件來搬家的話，搬完後這邊都會有暫存檔，如果是大站的這邊很容易會爆肥，記得要清。使用指令來撈出檔案最大的前五筆，筆數可自行調整。

```
du -ah /tmp  sort -n -r  head -n 5 
```

第二是 /var/lib/psa/dumps/domain，這是 Plesk 每個 domain 的備份目錄，我習慣都是備份到 Dropbox 上面，所以本機不會留備份檔，但有時後 Dropbox 備份失敗，Plesk 就會把備份檔存在這個目錄，所以檢查這邊有沒有肥大的備份檔是第二個地方。

\--max-depth 是要往下找到第幾層資料夾，以 plesk 的路徑來說是 /var/lib/psa/dumps/domains/www.aaa.bbb，domain 裡面放的就是該網址的備份檔，所以 --max-depth 設為一就好。

```
du -ah /var/lib/psa/dumps/domains --max-depth=1
```

第三是 /var/www/vhosts/，這就是網站的主要路徑，裡面放的是運行中的網站，底下第一層就是 domain，所以一樣撈出第一層就好。

```
du -ah /var/www/vhosts/ --max-depth=1
```

篩選出肥大的網站之後，再進去查看是哪些資料夾肥大，以 WordPress 來說正常肥大的是 wp-content/uploads，這邊放的是圖檔，所以網站經營久了，自然會肥大，只能考慮圖床或是其他方案。

可以砍的像是 snapshots 資料夾，這是在升級或更新 WordPress 前自動做的快照，以防有狀況時可以隨時還原，有固定備份的話這個資料夾的東西就可以砍了。

其他再檢查有沒有不需要的網站，或是為了搬家留下的壓縮檔，就只能一個一個檢查了，清完後空間還是不夠，就確認可以升級硬碟空間了。