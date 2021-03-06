---
title: WooCommerce 金流串接實戰
date: 2021-09-15 09:26:00
categories:
- WooCommerce
etoc: false
---

WordPress 的接案者主要區分為兩種類型：懂得運用各種外掛、佈景主題來滿足客戶需求的接案者，另一種為專門客製化的程式開發者。許多像我一樣沒有程式基礎的朋友，剛接觸到 WordPress 時都會被它生態圈的深度與廣度驚艷到，只要是能想得到的功能幾乎都有人開發出來，更不用說豐富的佈景主題容易讓人有選擇障礙，再加上成熟的付費外掛市集更是買也買不完。

早期許多接案者結合了這些工具進行接案獲得了豐碩的果實，他們熱心的把這些接案用的佈景主題、外掛整理成文章、拍教學影片分享出來，或是做成一系列的線上課程，內容的完整度可以讓學員上完後就能真的做出一個滿足客戶基本需求的官網或是包含金物流串接的購物網站。

<!--more-->

所以架站的門檻越來越低了，市場上的競爭者也越來越多，這時候這種類型的接案者勢必要開始把自己的核心價值放在除了架站以外的項目，像是數位行銷、廣告投放、社群經營或是搜尋引擎最佳化的服務，不然就很容易被市場淘汰，像我這部分就很弱，所以開始學習寫程式改做開發就變成我後來的選擇路線。

但該做的外掛都已經被開發出來了，走程式開發路線還有什麼機會呢？有的，而且超級多，因為 WordPress 的便利性讓市場上用它架站的客戶數量龐大，首先這就讓潛在需求變多，其次，客戶的網站不是做完就沒事了，隨著業務的發展，每天都會有許多變化，如果線上業務沒有跟著實體業務的情境來做調整，這會讓客戶失去很多業績，所以針對客戶內部需求的客製化就是很大的一塊市場，而且只要公司沒倒，永遠都會有做不完的需求。

最後，也是我覺得最關鍵的一點，不管 WordPress 生態圈再豐富、功能再強大，有一塊客戶需求是無法被滿足的，那就是與本地化的服務整合，不會有國外的開發者想整合台灣的 CRM 廠商，或是住在歐洲的開發者不可能來串接台灣的金流服務，因為在他們的市場根本沒人用，這時候就是我們在地開發者的切入機會，因此這幾年有越來越多台灣的資訊公司投入 WordPress 生態系。


## WordPress 工程師 vs. PHP 工程師

WordPress 是一個既有的框架，就跟其他的程式框架一樣，有自己的規則與設計哲學在裡面，它是在 2003 年誕生的，十幾年的歷史雖然有不少包袱，但早已是一套非常成熟的框架，有數以千計的工程師貢獻過核心程式碼，這些都讓 WordPress 不斷的進化與成長 ( 也常常進化到爆炸就是了 🤣  )，那麼已經熟悉 PHP 的工程師朋友，該如何切入 WordPress 開發呢？我從以下三點來說明：

### 一、對於勾點（ Hook ）機制的理解

勾點機制是 WordPress 可以擁有成千上萬各式各樣外掛的主要原因，透過「掛」載的動作就能在不需修改核心程式碼的情況下，加入新的功能，這樣當核心程式碼在更新的時候，新功能一樣可以被保留下來。看過太多被硬改 WordPress 核心程式碼的狀況，然後客戶被廠商要求 WordPress 不得更新，這會導致其他後果。

WordPress 的更新除了會加入新功能以外，還會修補許多安全性漏洞，畢竟它是全世界市佔率最高的架站工具，也會引來想要大展身手的攻擊者，所以如果不能更新 WordPress，那可能只能把主機的網路線拔掉才會 100% 安全。

為了要能更新 WordPress 程式，勾點機制是絕對必要的，而且透過它，還能提供開放介面讓其他工程師來修改自己寫好的外掛，像是購物車 WooCommerce 就有超多針對它開發的外掛，而我們即將要介紹的金流外掛也是屬於這一類。


### 二、對於 Loop 與操作資料庫方式的理解

WordPress 第二個核心是 Loop，它是顯示所有文章內容的機制，傳統 PHP 的作法是先建立資料庫連線，然後寫資料庫語法，把需要的資料取得後，再用迴圈的方式呼叫出來，而 WordPress 使用 Loop 簡化了這樣的流程，還提供許多豐富的 API 來自訂取得的資料內容。

雖然 PHP 工程師還是可以使用傳統的方法來做資料庫的操作，但前面提到，WordPress 已經有全世界的工程師幫你開發出各種 API，無論是針對安全性、效能，都是經過持續的優化再優化，沒有道理不用它，透過這些 API 除了簡化開發流程、增加安全性與效能外，更能學習到世界上其他工程師是如何處理相同問題的邏輯與思維，這對於開發者本身會有很大的幫助。


### 三、對於 WordPress 安全性的理解

上文提到，因為 WordPress 的成功引來了一大群攻擊者，要如何維護 WordPress 的安全性，都已經是許多國外企業的核心服務項目，最有名的就是 Wordfence 他們是一間專門開發 WordPress 防火牆外掛的公司，通時也常發表關於 WordPress 核心程式、佈景主題與外掛的漏洞研究報告。

常看他們的報告會發現，很多外掛的漏洞常發生在缺少驗證程式執行者的身份、執行者的請求來源以及對於惡意程式碼的過濾，這些議題在其他框架也都會遇到，只是因為 WordPress 很大，還有專門的機構來研究漏洞，所以當看到 WordPress 安全性相關的新聞報導時別驚慌，去客觀理解漏洞產生的原因後，就能知道該如何防範。

因此為了提升安全性，使用 WordPress 內建的 API 會是比較好的選擇，另外在執行寫入動作前，先用 ```curent_user_can()``` 檢查使用者權限、```wp_nonce_url()``` 確認傳送資料的來源是否為同一個網站、```sanitize_*()``` 過濾使用者輸入的內容、```esc_*()``` 過濾從資料庫取得的內容，再搭配資料的驗證，像是是否為空值、手機號碼是否為 10 碼等等，這些基本功做到就能防止一般常見的攻擊手法。


以上這些開發 WordPress 的核心概念我會透過一個鐵人付金流外掛來逐一介紹，現在只要記得這三個大方向就好，Hook、Loop、Security。


## WooCommerce 金流串接

WooCommerce 是 WordPress 母公司 Automattic 在 2015 年併購下來的一款專門做購物車的外掛，因為其豐富的功能以及能解決使用者在網路上賣東西的需求，讓原本只是拿來寫部落格用的 WordPress 逐漸成為建置購物網站的選擇。

至於為何選擇介紹 WooCommerce 金流串接來做為程式開發者的切入點？上文提到，不管全世界開發者設計了多少工具，本地的需求還是只能由本地的開發者來實現，再加上因為這類需求是直接關係到客戶的業績，能夠很具體的展現開發者的價值，所以對接案者來說，報價會比較有空間跟彈性。

最重要的一點，只要是使用 WooCommerce 建置的購物車、並且開發時都遵循 WooCommerce 的規則，都可以通用這支外掛，因此可以直接販售給其他有相同需求的客戶，一次工重複賣，這絕對對是開發者的夢想。

因此接下來的文章我會以四個單元來介紹如何開發 WooCommerce 金流外掛，並且帶到開發 WordPress 需要知道的核心知識，並且附上延伸資源來做補充，未來遇到問題就能靠自己的力量找到答案。

**(一) 前置作業**：說明如何拆解金流開發需求、串接文件需要注意的重點，以及本機開發環境與外掛結構介紹。

**(二) 設定介面** 說明如何建立設定選項以及增加自訂欄位來記住金流商回傳的資訊，同時介紹 WordPress 的資料庫，以及如何 CRUD。

**(三) 實作付款類別**：WooCommerce 的金流類別介紹，解析內含的屬性與方法，並且說明如何傳遞與接收參數，以及該如何寫單元測試來進行驗證。

**(四) 部署與發行**：介紹如何自動化部署到遠端主機，以及當想要販售該外掛給其他客戶時該如何做到外掛更新以及序號控管的機制。

下一篇我們就先從前置作業說明如何拆解 WooCommerce 金流串接需求開始。