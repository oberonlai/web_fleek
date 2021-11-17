---
title: 給台灣人專用的 WooCommerce 擴充
date: 2021-10-29 09:00:00
categories:
- WooCommerce 開發
etoc: false
---

這幾年接觸到的案子有八成以上都是與 WooCommerce 相關，而且需求都非常類似，像是結帳頁加入台灣縣市下拉選單、調整結帳流程、選擇超商取貨隱藏運送地址欄位等等，於是我就想何不把這些共同的需求整理成一支 WooCommerce 外掛，這樣之後遇到類似的案子就請客戶直接安裝就好。

有了這個想法之後我就開始投入實作，我大概是三年前開始動工的，到了今天只有做好後台的設定頁面其他都還沒完成XD，幸好在去年年底 Luke 與我接洽，不聊還好，一聊下去不得了，我們有很類似的想法，但他長年深耕網路行銷、電子商務等相關領域，跟我只能自己腦補使用者可能會需要的功能相比，他有更多的實務經驗讓這個外掛發展成更符合台灣中小企業所需要的樣子，於是我們很快的就展開合作。

<!--more-->

從第一個 Task 修改 WooCommerce 的結帳流程開始，到最近剛完成的整合 LINE Pay 金流，我們一起合作了 10 個多月超過 140 個小時開發這支「免費」外掛，對！你沒看錯，他花了數十萬請我開發一支外掛然後免費釋出，我問他為何不收錢，他很堅定的說：「因為受到 WordPress 社群的幫助太多，現在有能力了，決定要來回饋這個社群」，我聽到的當下非常感動，所以我也抱持著回饋社群的想法，盡己之力開發這支外掛。

由於他很低調，並沒有大力宣傳這支外掛，只提供給認識的客戶使用，於是在取得他的同意下，我決定來跟社群好好的介紹這一支功能強大的 WooCommerce 好用版擴充，我就以下三個面向來說明：

## 一、改善結帳流程
首先，WooCommerce 預設的結帳流程是加入購物車 > 購物車頁 > 結帳頁，好用版把購物車頁以及結帳頁整併在一起，所以當點選查看購物車時會直接進入到帶有購物車表格的結帳頁：

![](https://oberonlai.blog/wp-content/uploads/woocommerce-taiwan-extension/woocommerce-taiwan-extension-01.jpg)

其次，好用版調整結帳頁的整體版面為單欄式，並改變區塊的順序，預設的情況下是先填寫帳單資訊，萬一是使用超商取貨的運送方式，有些佈景主題會發生當選完超商跳轉回結帳頁，帳單資訊要全部重填的問題，這會讓人抓狂，因此好用版把運送方式拉到結帳頁的最上面，接下來是選擇付款方式，最後才是填寫帳單資訊：

![](https://oberonlai.blog/wp-content/uploads/woocommerce-taiwan-extension/woocommerce-taiwan-extension-02.jpg)

在帳單資訊的部分，好用版會判斷運送方式是否為超商取貨，如果是的話會自動隱藏縣市地址相關欄位，如果是需要填寫地址的，則為自動帶入台灣縣市下拉選單，減少顧客手動輸入的資料：

![](https://oberonlai.blog/wp-content/uploads/woocommerce-taiwan-extension/woocommerce-taiwan-extension-03.jpg)

其他還有像是支援虛擬商品隱藏地址欄位、設定國家欄位是否置頂、設定結帳按鈕文字等等的許多功能，目前支援 20 套常見的佈景主題，像是 Astra、Avada、OceanWP 以及 Flatsome，因為每個主題都會針對 WooCommerce 做一些結帳頁的客製化調整，因此需要花費許多時間來整合好用版的版面配置。

## 二、整合台灣金物流服務

拜 WordPress 開源社群所賜，好用版整合了五家金流、四家物流、兩家電子發票服務，感謝 Richer 的 [RY WooCommerce Tools](https://richer.tw/ry-woocommerce-tools)、羊羊數位的 [PayNow WooCommerce Plugin](https://paynow.yangsheep.art)，以及 WP 萃煉實驗室的 [LINE Pay Plugin](https://docs.wpbrewer.com)，讓好用版不用自行串接 API 就能包含各家金物流服務。

在啟用好用版之後，可以在 WooCommerce 設定頁面找到金流設定、物流設定、電子發票設定三個頁籤，只要開啟相對應的服務商就能立即使用相關的服務，對於使用者來說就不用再去找各家金流的外掛進行安裝：

![](https://oberonlai.blog/wp-content/uploads/woocommerce-taiwan-extension/woocommerce-taiwan-extension-04.jpg)

物流的部分也可以透過好用版進行設定：

![](https://oberonlai.blog/wp-content/uploads/woocommerce-taiwan-extension/woocommerce-taiwan-extension-05.jpg)

未來也會持續整合其他的金物流服務，讓好用版變成建立台灣電商網站的萬能工具箱。

## 三、後台操作介面調整

WooCommerce 後台的操作邏輯跟台灣的電商服務不太一樣，為了讓台灣使用者可以更快速的上手 WooCommerce 的操作，好用版做了許多調整，首先是訂單介面的修改，在帳單地址編輯的部分內建是拆成六個欄位，對於管理者來說要從其他地方貼上顧客地址時，需要分成六次來貼，而好用版增加了一個完整地址欄位，只要把地址貼入這個欄位就會自動寫入 WooCommerce 內建的欄位之中，省去多次複製貼上的時間：

![](https://oberonlai.blog/wp-content/uploads/woocommerce-taiwan-extension/woocommerce-taiwan-extension-06.jpg)

其次是加入商品時的變化商品設定，我用了 WooCommerce 這麼多年，老實說它的變化商品設定每次都讓我燒腦好久，總是會忘記要先設定屬性再設定價格，好用版為了解決這個問題參考了許多台灣電商服務後台，希望能用比較直覺的介面來讓使用者操作。

好用版首先會判斷如果當商品類型是可變商品時，自動將屬性與變化類型兩個頁籤置頂，提醒使用者這是要優先設定的項目：

![](https://oberonlai.blog/wp-content/uploads/woocommerce-taiwan-extension/woocommerce-taiwan-extension-07.jpg)

接下來進入商品屬性點選新增按鈕，可以看到數值輸入的介面不一樣了，以往要用「 |」 符號來分隔每一個商品屬性的規格，好用版讓它跟輸入文章標籤的介面一樣，每輸入一個點選 Enter 就能產生，閱讀上比較清晰也好辨識：

![](https://oberonlai.blog/wp-content/uploads/woocommerce-taiwan-extension/woocommerce-taiwan-extension-08.jpg)

可以看到下面還有一個變化類型的選項，你可以設定在前台商品頁面是要使用下拉選單還是單選方塊來顯示這些規格，同時還可以決定每一行要顯示幾個：

![](https://oberonlai.blog/wp-content/uploads/woocommerce-taiwan-extension/woocommerce-taiwan-extension-09.jpg)

前台的顯示效果如下：

![](https://oberonlai.blog/wp-content/uploads/woocommerce-taiwan-extension/woocommerce-taiwan-extension-10.jpg)

另一方面，以往在設定每個規格的售價時，都需要展開後才能設定，如果商品屬性的規格一多，操作起來就會要兩個動作：點開規格 > 輸入價格，所以好用版規格的售價直接放在外面，這樣就不用點進去才能設定：

![](https://oberonlai.blog/wp-content/uploads/woocommerce-taiwan-extension/woocommerce-taiwan-extension-11.jpg)

除了變化商品的設定外，在運費設定針對台灣離島也做了一些調整，可以直接在運送方式中指定離島區域，以往要自行填入離島的郵遞區號，現在可以使用勾選方塊來決定，看是要送到所有離島，或是離島不提供運送，甚至是只針對金門縣的金沙鎮來提供運送，透過好用版的介面都可以彈性的設定：

![](https://oberonlai.blog/wp-content/uploads/woocommerce-taiwan-extension/woocommerce-taiwan-extension-12.jpg)

以上是 WooCommerce 好用版擴充的簡單介紹，更多的功能可以實際下載來體驗看看，下載連結：https://morepower.club/morepower-addon/