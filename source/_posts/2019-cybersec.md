---
title: 2019 台灣資安大會 Day1
tags: []
id: '229'
categories:
  - - Host
  - - TCP/IP
date: 2019-03-20 22:54:26
---

![](https://i2.wp.com/oberonlai.blog/wp-content/uploads/2019/03/IMG_8004.jpg?fit=1024%2C768&ssl=1)

後兩天沒空，所以應該只會有 Day1 的心得XD

生平第一次參加由 iThome 所主辦的台灣資安大會，規模之大讓我大開眼界，整個研討會分為三天，除了上午大會堂的主講演講外，下午更有十軌的議程在同步進行，橫跨台北會議中心與世貿一館二樓的多間教室，廠商展覽攤位也是非常熱鬧，令我眼花繚亂@@

但一方面覺得可能是自己跑錯棚，或是根本沒待過相關產業，在這台灣 IT 產業這麼大的盛事裡，我竟然沒有看到半位認識的人，驚覺自己實在太渺小，也讚嘆台灣 IT 產業鍊之大，讓我這井底之蛙能以全新的視野來看待自己每天在處理的業務，也算是開了眼界。

一整天聽不懂的東西比聽進去的東西還多，雖然很多都是來賣藥的，但對於身兼半個網管的我似乎沒太大吸引力，唯一一場覺得比較有社群分享交流的講座是「IT 人專職資安人的起手式」，講師幽默風趣又分享許多免費資源，是一整天聽下來覺得賣藥意味比較不那麼強烈的內容，但賣藥歸賣藥，加減還是有學到一些東西，整理紀錄如下：

### 超連結時代下的資安協議與回應

這是接著小英總統致詞後的第一場開幕演講，講師 Bruce Schneier，大有來頭，世界知名的密碼學專家，出版過許多知名著作，[部落格](https://www.schneier.com)也有非常多隱私議題相關的文章，會後的新書簽名會也是大排長龍，算是今天認識到最重量級的人物。

在什麼都能連線的年代，資訊安全變得爆炸困難，每天隨身攜帶的手機、家裡的冰箱、上班開的汽車全都有可能會成為駭客攻擊的目標，講師提到六個課題是現在 IT 從業人員所必須面對的：

1.  軟體的開發流程要納入安全性測試，讓參與人員能擁有安全意識。
2.  現在很大一部分的漏洞修補都是在彌補早期網路技術所衍伸出的問題。
3.  如何確保每一個使用者的裝置都能保持在最新狀態，但對於消費者而言，更新之後有很多功能根本就不是他們想要的。
4.  系統的複雜性造成安全防護的困難。
5.  互相相連的裝置變成了一個巨大的網際網路，其中一個被植入非常容易傳染給其他裝置。
6.  第六點沒聽懂，好像是關於密碼這事...

潛在的個人資料被竄改也是很危險的事，像是醫院裡面的病歷被竄改、汽車電子鎖被入侵，手機更是非常危險的裝置，裡面有所有與你相關的個人機密。

資訊安全已經是國際間的問題，現在證明這問題基本上是無解，講師的建議是在軟體開發時就要把安全防護機制一併開發進去，在開發流程中就要有安全意識，而各國政府也要開始思考安全策略，從整體面來著手資訊安全問題。

### 恐懼的總和

講師為資安業界非常資深的張裕敏協理，現職為趨勢科技全球核心技術部，他帶來很精彩的分享內容，首先就駭客攻擊國家基礎設施的案例開頭。

3/7 委內瑞拉大停電，主因是水利設施被攻擊，停止運轉所以造成無法發電，其他國家也發生過污水處理廠被攻擊，而紐約則是發生過水壩控制中新被入侵後將水導入其他城市。駭客攻擊的目的不外乎是希望造成損失、霸佔資源或是當做正式攻擊前的練習。

駭客的攻擊手法有植入後門、社交工程、透過內部員工取得機密資訊等等，去年講師預測過未來有可能發生的攻擊事件，結果準確預測後引來大批國外媒體採訪，因為被誤認為是攻擊者，所以今年就不做預測了...

國家關鍵基礎設施有明確定義，影響民生妨礙國家安全都算基礎建設，一共有八大類，鍵基礎設施被駭實例有污水處理廠、水壩、核電廠、偷取上市公司未公開的財報資訊等等。

駭客的火力支援，要漏洞有漏洞，因為網路上都查得到，[https://ics-cert.us-cert.gov/advisories](https://ics-cert.us-cert.gov/advisories)

駭客要工具有工具，github 上面就一堆 ICS 開源工具，駭客要方法有方法，參考 [https://attack.mitre.org/matrices/enterprise/](https://attack.mitre.org/matrices/enterprise/)，目的是要控制與損害關鍵設備。

駭客要目標有目標，使用 Shodan 搜尋引擎來找攻擊目標  
[https://www.shodan.io](https://www.shodan.io)  
[https://ics-radar.shodan.io](https://ics-radar.shodan.io)  

[](https://www.shodan.io)駭客眼中的新玩具：關鍵基礎設施，主要為網路基礎設施、金流基礎設施以及資訊媒體。使用 DNS 劫持讓訪客連到駭客指定的網站，因此設定 DNSSEC 就非常重要。路由器劫持，會造成偽造流量、封包路由異常。Public wifi 藉由外部入侵、實體入侵或是偽冒 AP，公眾 wifi 的路由器可能會被植入木馬。最後是 4G/5G 假基地台，會造成蓋台、通訊協定漏洞。

金融基礎設施的攻擊包含後端資訊系統攻擊、外部入侵、帳戶清除，另外還有放置假 ATM 機偷吃提款卡與密碼，如果提款機左右兩邊是看的到機器本體一定是假的，真的機器有規定三面都一定是要密封的，避免有任何人為的植入行為。

最新釋出的 ATM 惡意程式會讓提款機顯示畫面出現俄羅斯方塊、拉 BAR 等小遊戲，中獎時會吐鈔票。

資訊媒體基礎設施的攻擊包含電視、網路、社群媒體等等，攻擊的主要目的是為了資訊操控、散播假資訊、資訊再加工，背後有特定目標及人士進行組職。最嚴重的資訊媒體入侵事件發生在 2013 年首爾的 darkseoul 事件。  
[http://blog.xecure-lab.com/2013/03/darkseoul-apt.html](http://blog.xecure-lab.com/2013/03/darkseoul-apt.html)  

入侵資訊媒體基礎設施可以讓駭客有穩定的收入來源，有心人士也能更好操作輿論。最後試想萬一哪一天下班，捷運暫停行駛、要開車去加油結果油價漲了 2000 倍、路上的交通號誌全部是綠燈、手機 GPS 沒訊號、智慧門禁沒電進不了家門、廣播電視完全沒有訊號、無法刷卡、ATM 無法提款、假消息、陰謀論，這會是世界末日的場景？

終極防禦之術？政府要多參考其他國家是如何防禦基礎設施，資訊安全是大家的事，如果哪天在等公車看到有人在對電子公車看板上下其手、毛手毛腳，立即通知相關單位，那人有很有可能就是在尋找實體入侵的 USB 接口。

### 當代資安的迷思與數位韌性  

講師為韋萊韜悅台灣分公司企業風險與保險經紀的吳明璋協理，著有鋼索上的管理課 韌性管理一書，分享關於企業如何做好韌性管理，第一次聽到原來還有資安保險這種東西，以及做筆記的時候一直把韌性打成任性XD

資安的難題與挑戰，通常來自你我未知的領域，講師透過 jothari window 周哈里窗來描述資安的現況：我知你也知是開放、我知你不知是隱藏、你知我不知是盲點、你不知我也不知就是未知，而資安風險就是要盡可能消弭未知的存在，透過交流學習來避免未知。

ISO 27001 是一套資訊安全管理系統驗證，許多企業努力取得該驗證，但還是有不少企業遭受到攻擊，所以引發了業界討論關於 ISO 27001 的有效性。關於資安常見的迷思：公司門禁森嚴還需要資安嗎？請人「測試」系統來跟老闆爭取經費？資安保險有必要嗎？

資安風險的典範移轉正在進行中，發生攻擊事件是無法百分之百避免的，所以要聚焦在風險可接受的範圍、風險如何移轉、以及風險發生時如何減少損失，而韌性管理已成為國際資安領域中的顯學，因為資安而造成的損失金額並不會低於天災。

數位韌性 ( Cyber-Resilience ) 的定義也就是當遭受攻擊時企業能夠應變的程度，並且能把損失控制在可接受範圍，避免骨牌效應的產生，數位韌性的執行三步驟：Assessment、 Protection、Quantified。

資安保險的承保範圍包括數位資產的遺失、資料外洩、網路斷線以及網路勒索均能理賠。

### It 人專職資安人的起手式

![](https://i0.wp.com/oberonlai.blog/wp-content/uploads/2019/03/574669285.330213-2.jpg?fit=576%2C1024&ssl=1)

講師非常有趣，因為怕被查水表以及所屬公司未允許在公開場合簡報，所以帶著駭客組織 Anonymous 匿名者專用的 V 怪客面具，讓我非常期待會有什麼大爆料的精彩簡報，聽完後簡報收穫很多，但完全沒爆料啊XDD

講師吳明蔚現為奧義智慧共同創辦人，是業界非常知名的白帽駭客，他講話速度非常快，聽完後只覺得 30 分鐘根本不夠而且意猶未盡，難怪這一堂課教室大爆棚，滿到根本沒地方坐，分享內容以免費的資安工具介紹為主軸，還小噹了一下教室外面在賣的資安產品，這講師的風格完全對我的胃口XD

使用 Open Source 有相對應的風險，萬一東西壞了沒有客服讓你問，只能自己下去修，log 也要自己爬、疏理不同格式的日誌，最麻煩的就是更新與相容性問題，爆炸了一樣只能自己下去修，所以會花很多時間去處理 Open Source 工具所衍伸出來的問題。

要做好資訊安全第一間最重要的事，就是先盤點現有的所有機器，包含公司的電腦、路由器、伺服器等設備，並嚴查 Internet 的出入口，也就是 Port 號要嚴格控管，另外有人登入主機要能夠發通知給管理者，以防主機被登入都不知道。

使用 Nmap 來檢查主機狀態與對外開的 port 號  
[https://blog.gtwang.org/linux/nmap-command-examples-tutorials/](https://blog.gtwang.org/linux/nmap-command-examples-tutorials/)  

盤點資產用軟體 OCS inventory  
[https://www.ocsinventory-ng.org/en/](https://www.ocsinventory-ng.org/en/)  
  
使用 The Dude 來圖像式整理網路設備  
[https://mikrotik.com/thedude](https://mikrotik.com/thedude)  

做好盤點之後，接下來就是來決定這些設備的重要性，存有客戶資料的機器很重要，老闆電腦的 D 槽也是非常重要XD，可以使用新竹市稅務局的資訊資產盤點表做為範本，裡面有有非常詳盡的格式來記錄所有資訊資產。  
[https://www.informationsecurity.com.tw/article/article\_print.aspx?aid=5796](https://www.informationsecurity.com.tw/article/article_print.aspx?aid=5796)  

盤點與紀錄都完成後，剩下的就是每個月的例行事務，像是使用 Nmap 每個月掃一次看看有沒有被開奇怪的 port，如果有被開但問人卻抓不到是誰，很簡單，把 port 關了在用的人就會來叫了。

至於平日主機的監控，可以使用 Zabbix，他可以做到主機登入偵測，在手機上都可以看到主機被登入的警報，另外還可以做多台主機的叢集式管理，登入警報有個 Windows 作業系統的雷，因為某些執行緒會觸發登入行為，所以會被誤認為是主機登入，如果警報太多就沒有意義了，有跟沒有一樣。  
[https://www.zabbix.com](https://www.zabbix.com)  

另外可以使用 fail2ban 來做惡意 IP 的排除，在 Windows 上用的叫做 wail2ban  
[http://www.fail2ban.org/wiki/index.php/Main\_Page](http://www.fail2ban.org/wiki/index.php/Main_Page)  
[https://github.com/glasnt/wail2ban](https://github.com/glasnt/wail2ban)  

傳統防火牆跟應用程式防火牆 (WAF) 最大的差異是傳統防火牆只會分辨識是文旦還是榴槤，判斷是文旦後就把文旦放在籃子裡，但無法分辨出放進籃子的文旦有沒有發霉或是爛掉，而 WAF 就好比 X 光機可以掃描文旦，看他的內部有沒有出問題。

也就是以 OSI 模式來說，傳統防火牆只能偵測第三層的封包，也就是網路層 TCP/IP 協定的封包，而應用程式防火牆可以偵測第七層也就是應用層的封包，現在很多攻擊都是從應用層下手，而 WAF 主要就是防範這類的攻擊。

WAF 可以看成一個反向代理伺服器，直接在源頭就把可疑封包給過濾掉，連讓他進主機的機會都沒有，可以使用 Nginx 做反向代理。

萬一真的被入侵了，不要急著砍掉重練，先把被入侵的資料做備份後再做處理，要抓兇手也要先留下證據，第一時間就重灌的話什麼證據都沒有了。

### 狙擊駭客，拯救網站安全

講師為虛擬主機捕夢網雲端架構師陳建宏，講題內容主要在介紹他們所代理的 WAF 產品 dotDefender，涵蓋許多網站層面的防護。

之前流行的勒索病毒是透過公司內部的 45 port 進行散播的，這代表企業內部的資安意識不足，而被攻進了內網。為了加強網站的安全性，在開發流程就要把資安檢測項目一併納入測試流程之中，最好的情況可以用分散式架構來分散網站、資料都在同一台主機的風險，另外也可以購買資安產品來加強防護。

除了開發者的資安意識外，也要將使用者納入安全防護圈中，近年來電商網站講究購物便利，所以都會幫客人記住帳密然後自動登入，結果這些資料用明碼的方式存在 Cookie 中，有心人士就能直接登入竊取信用卡資訊。

網站被入侵要先備份，千萬不可直接還原，才能診斷原因，進而找出漏洞的根源，漏洞補完後，要去檢討修補成效，也就是要進行複測，確保漏洞已被修補。

平時就要定好被攻擊後的應變方法，通報該通報的單位，說明事情的起因以及處理方式，在台灣，企業都很害怕被別人知道自己網站被入侵，應該要仿效歐美的方式，把事發經過整理出來讓同業交流。

WAF 有分為硬體式、軟體式以及雲端式，dotDefender 為軟體式，可以部署在自己的主機中。雲端式 WAF 的缺點是 DNS 要指過去，軟體式的好處是可以依照系統架構來安裝，比較彈性也好管理。

中國菜刀木馬程式  
[https://fly8wo.github.io/2018/08/17/中国菜刀之写一句话木马变形/](https://fly8wo.github.io/2018/08/17/中国菜刀之写一句话木马变形/)  

電子商務網站提供給客戶的訂單查詢功能要小心，訂單號要記得加隨機亂碼，不然被猜出訂單邏輯其他客戶的訂單就會被看光光。

## 小結

看到很多廠商都在賣 WAF，這是今年資安界的趨勢嗎？