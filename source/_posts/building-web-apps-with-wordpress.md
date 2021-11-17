---
title: 【 書摘 】Building Web Apps with WordPress
tags:
  - mvc
  - webapp
id: '828'
categories:
  - - WordPress
date: 2020-03-13 10:57:50
etoc: true
---

![](https://oberonlai.blog/wp-content/uploads/2020/03/250w-1.jpeg)

能夠看到這麼多實務技巧以及如此全面的 WordPress 開發書籍，就覺得買這台電子書閱讀器值回票價了！

Building Web Apps with WordPress 是由 Jason Coleman&Brian Messenlehner 合力撰寫的一本關於如何使用 WordPress 開發 Web App 的程式書籍，雖然標題是寫 Web Apps，但對於想要致力於開發 Theme 或是 Plugin 的朋友都非常適合，因為它涵蓋了所有在開發 WordPress 時會涉及的議題。

像是從檔案結構、資料庫結構，到怎麼開發外掛、佈景主題，CPT、Taxonomy、OOP 開發技巧、User Role、WordPress API、安全性、ES6、WordPress REST API、Gutenberg Block editor、Multisite、本地化、效能優化、規模化、WooCommerce、整合 PHP Libraries 等等，

最後一章還預測了未來 WordPress 會走到哪個方向，對於想要掌握開發趨勢、學習基礎、進階技巧，個人超級大推這一本，看完之後一整個對於 WordPress 有了全新的認識。
<!--more-->
全書的範例檔在這邊 --> [https://github.com/bwawwp/bwawwp](https://github.com/bwawwp/bwawwp)，裡面用了一個 SchoolPress Open Source 當教學範例--->[https://github.com/bwawwp/schoolpress](https://github.com/bwawwp/schoolpress)，Jason Coleman 是  [Stranger Studios](https://www.strangerstudios.com/) 的創辦人，這公司開發了 [Paid Membership Pro](https://github.com/bwawwp/schoolpress) 這隻外掛。

本書共有 18 個章節、1546 頁，每一章我會努力的用中文做摘要，再加入我自己的經驗做腦補，希望有辦法能在一個月之內整理完這個大工程，好長的一條路啊，開始走囉～

## 目錄

[一、Building Web Apps with WordPress / 使用 WordPress 打造 Web Apps](#cp1)

[二、WordPress Basic / WordPress 基本介紹](https://oberonlai.blog/building-web-apps-with-wordpress-basic/)

[三、Using WordPress Plugin / 使用 WordPress 外掛](https://oberonlai.blog/【-書摘-】building-web-apps-with-wordpress-using-wordpress-plugin-使用-wordpress-外掛/)

四、Themes / 佈景主題

五、Custom Post Types, Post Metadata, and Taxonomy / 自訂文章、文章欄位、文章分類

六、Users, Roles, and Capabilities / 使用者、角色、權限

[七、Working with WordPress API, Objects, and Helper Functions / 使用 WordPress API、物件、與實用函式](https://oberonlai.blog/building-web-apps-with-wordpress-api-objects-and-helper-functions/)

八、Secure WordPress / 加強安全性

九、JavaScript Frameworks and Workflow / JavaScript 框架與工作流程

十、WordPress REST API

十一、Project Gutenberg, Blocks, and Custom Block Types / 關於古騰堡編輯器

十二、WordPress Multisite Networks / 多站網路管理

十三、Localizing WordPress Apps / 多語系

十四、WordPress Optimization and Scaling / WordPress 效能優化與規模化

十五、Ecommerce / 電子商務

十六、Mobile Apps Power by WordPress / 用 WordPress 運作手機應用程式

十七、PHP Libraries, Web Service Integrations, and Platform Migrations / PHP 函式庫、網頁服務與平台整合

十八、The Future / 未來

## 一、Building Web Apps with WordPress / 使用 WordPress 打造 Web Apps

在開始之前，作者先來定義什麼叫做網頁應用程式 Web App 。

## **Web / 網站**

由許多頁面所構成，使用網頁瀏覽器進行瀏覽與操作

## **App(Application) / 應用程式**

由很多可以互動的功能所組成的軟體

## **Web App / 網頁應用程式**

可以運作在瀏覽器上面的軟體，或是泛指帶有更多互動的網站，Web App 的特徵如下：

（一）有互動元素可以進行互動

（二）任務優先於內容，可以解決某些問題或是產出某些東西的網站

（三）有登入註冊會員系統，可以儲存會員資料

（四）完整的裝置相容性，可以取得裝置的硬體使用權限，像是相機、地理位置定位

（五）沒有網路在離線狀態時也能使用

（六）與其他平台串接整合，使用各種服務的 API

因為觀察到 Web App 的趨勢，Google 推出了 Progressive Web App ( PWA ) 的技術來讓網站方便整合 Web App 的特性，使用 WordPress 架站的話可以利用 [PWA plugin](https://wordpress.org/plugins/pwa/) 讓現行網站具備 Web App 的功能，關於 PWA 的特性如下：

（一）有獨立的 URL：Web App 在現今的開發框架上多半採用 Single Page 單頁式的方式來呈現操作介面，然後透過 Ajax 去呼叫資料來載入並顯示內容，所以整個 Web App 就只有一頁，在某些情況下對使用者體驗不佳，所以 PWA 可以把不同的內容用 Rewrite 的方式拆分成不同的網址，然每個頁面可以獨立被連到。

（二）頁面切換沒有延遲或是必須等待載入的感覺，因為如同第一點所提，整個網站只有一頁，所以切換頁面完全不用讀取，需要等待的只有透過 Ajax 載入資料的時間。

（三）連線為 HTTPS

（四）使用 Responsive Web Design ( RWD ) 技術來頁面相容於各種裝置

（五）載入快速，因為只有一頁

（六）使用本地儲存技術讓網站在沒有連線的狀況下也可以使用儲存在客戶端的資料來呈現內容，待裝置與網路連線後再把本地資料傳送給伺服器進行儲存

（七）加入桌面捷徑 Add to Home Screen 功能：因為 Web App 必須透過瀏覽器執行，為了讓使用者可以方便再次進入，開發者通常都會希望使用者把這個網址加入桌面的圖示變成像手機原生的 App 一樣，不用再開啟瀏覽器即可再次造訪

Google 提供了 [Lighthouse Tool](https://developers.google.com/web/tools/lighthouse) 來測試你的網站是否支援 PWA 功能。

> 如果還是有朋友不了解什麼叫做 Web App，可以打開一下 Google Map、Google 文件、Gmail，這些使用瀏覽器就可以造訪的服務或軟體，就是很標準的 Web App。
> 
> 也因為這樣的技術已經成熟到一個階段了，有非常多的軟體公司在靠著這樣的服務在創造營收，Netflix、Office 365、Dropbox，這樣的商業模式叫做軟體及服務 Software as a Service ( SAAS )，這類服務最典型的收費模式就是訂閱制，而本書常提及的外掛 Paid Membership Pro 就是一套可以設計 SAAS 服務的工具。

## **為什麼要用 WordPress 做 Web Apps**

網頁技術百百種，為什麼要選擇 WordPress？首要原因當然就是因為你已經用它做過很多網站了，而你非常熟悉它，閉著眼睛都可以數得出來後台左側選單依序有哪些，如果今天要開發一套工具，當然是非他莫屬！

除了個人因素外，其他客觀原因還有：

（ㄧ）內容管理超級好用，常有的欄位都已內建，不夠用還可以自行新增，或是創造新的文章類型

（二）會員管理簡單易用又安全，密碼自動 Md5 加密，角色權限分層直接內建完整，不夠用一樣可以自行新增

（三）第三方的外掛數量爆炸多，而且大部分都是免費，付費的功能超多價格又能負擔得起

（四）彈性很高，WordPress 可以做各種類型的網站，通常一間新創公司對於網站的需求大致如下：使用者一頁式網頁宣傳服務 -> 加入表單搜集名單 --> 加入部落格 ---> 做 SEO ---> 社群分享 ---> 增加論壇 --> 加入付費機制 ---> 加入付費會員專屬內容或功能 ---> UI 改版 ---> 優化網站並擴大硬體規格 ---> 多語系 ---> 加入 PWA --> 上架 APP 等等，以上這些步驟 WordPress 全部都有相對應的外掛或服務可以利用

（五）WordPress 核心維護團隊對於安全性漏洞的修補速度快

（六）建置成本低，WordPress 程式碼是免費的，PHP / MySQL 免費、大部分的外掛也都是免費，初期要負擔的成本就是主機租賃的費用

> Open Source 軟體我覺得最重要的就是社群，因為 WordPress 本身是免費的，能否有辦法得到社群的技術資源是關鍵。我自己最早用的開源 CMS 是 Xoops，當年的資源少得可憐，一遇到問題只能去國外論壇爬文，就算提問也很少得到解答。
> 
> Xoops 相關的外掛更是少到不行，台灣只有吳弘凱老師有固定持續在開發新的模組，很謝謝他當年的分享！所以之後嘗試了 Joomla，Joomla 在台灣有固定的小聚，而且還剛好在上班的地方附近，但我一次也沒去過，因為 Joomla 的後台操作邏輯太奇妙了...
> 
> 至於現在 WordPress 的社群就不用講了，在多位大大的努力之下，早已遍地開花，偶爾想上去正體中文社團幫忙回答一些問題，只要問題ㄧ PO 出來一分鐘內立馬就會有人主動回覆，根本搶不到XD
> 
> 更不用說還有國際級的 WordPress 活動 WordCamp 也在台灣成功舉辦兩次了，只能說當年改用 WordPress 一整個是壓對寶！也因為用的人超多，一堆能想得到的各種應用都有人做出來，國外的 WordPress 商業模式也已非常成熟，從主機到各種進階的外掛應有盡有！

作者對於一些關於 WordPress 的批評也作出了回應：

（ㄧ）WordPress 只是拿來寫部落格用的工具：早在 WordPress 3.0 加入 Custom Post Type 的機制後，WordPress 就能用來製作各種類型的內容網站，形象官網、新聞媒體、購物商城，只要網站需要資訊呈現的內容，WordPress 都能輕鬆應付！

（二）WordPress 無法適應大型網站：WordPress.com 就是一個最好的範例，相同的原始碼，可以承載世界頂尖的媒體網站，像是 [BBC America](http://www.bbcamerica.com)、[TechCrunch](https://techcrunch.com)、[Facebook Newsroom](https://about.fb.com/news/)，就跟其他框架一樣，分散式架構、快取、CDN 都可以應用在 WordPress 上面，看人會不會用而已。

（三）WordPress 不安全：WordPress 的核心程式碼有全世界專門的團隊在維護，有嚴重的漏洞發生時很快就會釋出更新，跟請一位工程師自行開發的網站比起來，完全不用擔心哪天工程師離職了就沒人維護，歷年來的 WordPress 更新紀錄可以參考 [Release Note](https://wordpress.org/download/releases/)。

（四）WordPress 的外掛爛透了：有些真的很爛，而且出問題了根本找不到開發者，但這幾年來因為市場的規模擴大，越來越多公司投入外掛開發，商業模式的成熟讓外掛品質有顯著的提升，也讓公司賺進大把的鈔票，又因此吸引更多公司投入，形成良性的競爭循環。

> 個人覺得最功不可沒的就是 Envato，他們把整個 WordPress 的商業生態給建立了起來，早期投入的廠商獲得豐厚的果實，讓後期投入的開發者無不絞盡腦汁設計出各種類型的高品質商品，值接促進更多企業願意選擇 WordPress 作為架站系統，這樣的循環持續下去，如果哪天 WordPress 的全球市占率超過 50% 我也一點都不意外。

## **何時不要用 WordPress 做 Web Apps**

（一）如果你的商業模式是要把網站連 Source Code 都要一起售出的時候：WordPress 採用的的是 GPL 授權，它的主要授權方式是任何人都可以自由的修改、散播程式碼，而用在 WordPress 裡面的所有程式也必須言用 GPL 授權。

如果今天你的 App 是架在自己的主機上，販售的是會員使用權限，像是付費會員有更多進階功能或內容，那麼這跟 GPL 授權沒有關係。

但如果這套系統是要裝在對方的主機上，那麼就不得根據「程式碼」來進行收費，因為所有 WordPress 的 source code 都是開源的，而你客製過的程式也必須遵守開源的規範，必須以相同的授權進行免費散播，同時讓人有自由修改的權利。

簡單來說，當你使用 WordPress 進行創作的時候，如果要給其他人使用，都是必須照著 GPL 的規範，所以如果你是計畫要賣掉網站來賺錢，WordPress 就不是一個合適的選項。

> 因為 GPL 的授權方式，現在可以找到一堆 GPL Download 的服務，這些服務從各種管道取得付費程式的原始碼，然後用低於市價或是訂閱制的方式銷售這些程式，然後聲稱他們收的是「維護費」，並強調他們提供的程式 100% 正版，絕對沒有後門，而且還會定期更新。
> 
> 之前看過一篇訪談，被訪者是幾間知名外掛大廠的負責人，談對於 GPL 授權以及 GPL Download 的想法，多數老闆對於 GPL Download 斥之以鼻，但也有老闆不在意，因為他們賣的不是程式碼，而是「服務」，協助使用者解決問題或是開發更多新功能才是他們的核心價值。
> 
> 身為開發者的我當然很度爛 GPL Download，但與其要去防堵這些堵不完的網站似乎不太可能，因為 GPL 的授權條款就是這樣運作的，WordPress 看來也沒打算改變，只能專注在提供更好的服務。
> 
> 對於使用者而言，如果你不擔心程式碼是否為正版、不在意是否有售後服務、不關心開發者的心血，那就還是去乖乖買正版，因為不買正版你就沒有這麼多價格實惠又功能強大的外掛可以用，只能花大錢、花時間、花精力請一個世界級的團隊幫你開發了。

（二）如果你的技術人員有其他更熟悉的技術，像是 Asp.net、Ruby、JAVA，那就不要用 WordPress，可以快速把商業需求實作出來的技術才是重點，用什麼技術取決於執行人員的掌握度。

（三）如果你的網站很單純，未來不需要持續更新或加入更多的新功能，甚至是連內容的更新都不需要，這種可以用幾個 HTML 頁面就搞定的網站，就不要用 WordPress。

（四）如果你需要高度即時互動的網站，像是可以即時聊天的線上遊戲、或是需要每秒更新內容的網站，這種就不要用 WordPress 來架。但這並非是 WordPress 的問題，而是傳統網站系統架構的問題，因為所有使用者請求都要經過伺服器，再透過後端語言來處理請求結果必顯示在畫面上。

WordPress 這兩年已開始透過 Gutenberg 這個編輯器來設計不需要透過後端伺服器來進行資料的處理，大量使用 JavaScript 、REST API 與非同步技術來改善傳統網站的流程，也許在未來即時互動網站也適合使用 WordPress 來處理。

## **WordPress vs. MVC Framework**

從早年的 Blog 成長為 CMS 內容管理系統，到現在有非常成熟的 API 以及豐富的 Hook 機制， WordPress 已成為網站開發框架 ( Platform )。

相較於強調其他 MVC 框架，WordPress 沒有採取非常正規的MVC 設計模式，但還是有可以互相對應的地方。Plugin 可以看作是 Model，控制資料的部分，Theme 是 View，負責視覺呈現的介面，而 Template Loader 則可以看作 Controller。

當造訪 WordPress 網站時一律會達到 index.php，再透過 Theme 的模版系統來決定資料要呈現在哪一個模板上，也就是 Controller 負責的工作。

> MVC 模式（Model–view–controller）是軟體工程中的一種軟體架構模式，把軟體系統分為三個基本部分：模型（Model）、視圖（View）和控制器（Controller），
> 
> **MVC**模式的目的是實現一種動態的程式設計，使後續對程式的修改和擴充簡化，並且使程式某一部分的重複利用成為可能。
> 
> ＷikiPedia

## **SchoolPress 專案介紹**

[SchoolPress](https://schoolpress.me) 是一個開源的專案，它是運作在多站網路上面的一套系統，主要功能為讓學校的老師可以透過這套系統來管理班級、課程內容、出作業以及管理學生提交的作業，主要的架構簡要如下：

*   商業模式為學校付錢給老師使用，老師使用來管理班級學生
*   每一個學校是多站網路下面的一個子站，網址使用子網域
*   班級管理是用 BuddyPress 的 Groups
*   老師出的作業是 CPT
*   學生交的作業是 CPT
*   學年度是班級 CPT 的 Taxonomy
*   系所示班級 CPT 的 Taxonomy
*   Theme 使用 [MemberLite](https://wordpress.org/themes/memberlite/)

了解這個專案的內容結構對於接下來的範例會比較有概念。