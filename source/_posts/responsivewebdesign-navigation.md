---
title: 響應式網頁設計 - 響應式選單設計模式
tags: []
id: '3236'
categories:
  - - 使用者介面
date: 2013-08-18 22:19:38
---

[![responsiveNav-5](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-5.jpg)](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-5.jpg)   接續上週[響應式網頁設計主題](https://oberonlai.blog/responsivewebdesignforplanning/)，本週將繼續探討在響應式網頁中導覽系統設計的模式，並分析其優缺點與適用情況，以快速應用於實際工作流程之中。  

### 響應式選單設計的限制

有別於桌上型螢幕的大尺寸，手機的顯示空間往往不到其二分之一，雖然可以透過互動的方式來操作選單，但越高的互動性意謂著更高的使用門檻，在行動裝置使用普及率高的環境下，很難保證每一位使用者都有能力來進行操作，因此保持越簡單、越直觀的選單，才能確保更高的可用性。   其次是高互動性往往需要更複雜的程式來執行，對於網路速度較慢的且硬體等級較低的行動裝置會有難以操作的風險，因此響應式選單設計不宜過度參考手機原生應用程式(APP)的導覽模式，仍應就網頁的架構下進行思考。
<!-- more -->
 

### 響應式選單的常見類型

本文根據 Brad Frost 的文章 [Responsive Navigation Patterns](http://bradfrostweb.com/blog/web/responsive-nav-patterns) 為基礎，並加入我們的實務經驗來說明各類型選單的優缺點與適用情況。以下類型為目前網站較常採用之設計模式: 1.頂部導覽 2.底部導覽 3.下拉選單 4.收合選單 5.浮動選單 6.移除選單  

#### (一)頂部導覽

最單純的導覽形式，把選單直接置於頁面頂部，依照畫面寬度自動縮小選單文字，如選單數量超過螢幕寬度則自行換行。

 [![responsiveNav-1](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-1.jpg)](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-1.jpg) [http://trentwalton.com/](http://trentwalton.com/)

    優點 : 不需改變任何版面配置的形式，也不用寫 javascript 來設計互動效果，只需要設定均分每個選單的寬度百分比，就能依照螢幕寬度進行縮放，在使用上也較為直觀，不用進行任何點擊就能找到選單。   缺點 : 如果選單過多則內容容易被往下擠，導致無法在第一眼可視畫面中找到網頁內容；而在不同的裝置、瀏覽器中所採用的預設字體不同，在開發時針對特定裝置進行調整，但無法確保在所有裝置中都能維持希望的版面配置，譬如字體過大而造成選單字斷行；另外因為螢幕變小可點擊範圍也會因此縮小，對於手指較癡肥的使用者可能會造成誤擊選單。   適用情況 : 選單數量少、架構單純的網站，或是將功能導覽採用該模式設計，方便使用者一眼就能找到。  

#### (二)底部導覽

將選單移至頁面底部，而頂部可用錨點連結的方式將頁面帶往底部進行導覽。

 [![responsiveNav-2](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-2.jpg)](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-2.jpg) [http://contentsmagazine.com/](http://contentsmagazine.com/)

    優點 : 不需要使用 javasctipt ，並單純使用 css 即可進行調整，節省網站使用資源。   缺點 : 因為直接跳到頁面底部，會讓使用者對於頁面空間產生混淆，而且視覺效果很突兀，沒有美感。   適用情況 : 相對於頂部導覽的位置，在頁尾增加多層級的複雜選單較無空間上的限制，適合已有設計底部胖選單的網站，再加上選單在頁面底部，能讓使用者看完頁面內容後協助他們往下一個地方去，建議是不需要在頂部設計錨點連結，以避免使用者產生混淆，直接於頁尾中置入選單即可。  

#### (三)下拉選單

將原有選單轉換為表格輸入中的下拉選單，利用瀏覽器本身的表單組件進行導覽。

 [![responsiveNav-3](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-3.jpg)](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-3.jpg) [http://viljamis.com/](http://viljamis.com/)

    優點 : 非常節省顯示空間，是使用者易於辨認且熟悉的互動方式，行動裝置瀏覽器的內建下拉選單組件，讓使用者可以輕易完成任務。   缺點 : 需使用 javascript 來轉換，另外下拉選單是輸入表單資料所採用的元件，可能會造成使用上的混淆。   適用情況 : 可以運用在含有多階層的選單之中，但在可用 javascript 的環境之下，有其它更好及更易於辨識的作法來設計導覽系統。  

#### (四)收合選單

運用 javascript 來呈現選單，各種不同的選單展開方式提升互動性與美觀度。

[![responsiveNav-4](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-4.jpg)](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-4.jpg) [https://oberonlai.blog/](https://oberonlai.blog/)

  優點 : 節省空間，且並不會讓畫面跳動而造成頁面瀏覽混淆，以及可以設計符合整體風格樣式，達成使用體驗一致性，實際製作上也很容易，可輕易達成豐富的互動性選單。   缺點 : 越豐富的呈現效果，依賴 javascript 的程度也越深，對於硬體效能不佳的行動裝置而言，會導致使用體驗降低。   適用情況 : 架構從簡單至稍微有規模的網站幾乎都能勝任，或是想提供更豐富使用體驗的網站都能採用此類型做法。  

#### (五)浮動選單

從APP延伸而來的選單設計方式，將選單覆蓋至螢幕三分之二的大比例。

[![responsiveNav-5](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-5.jpg)](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-5.jpg) [http://mmenu.frebsite.nl/examples/responsive/index.html](http://mmenu.frebsite.nl/examples/responsive/index.html)

優點 : 非常優雅的呈現方式，並且可以把選單層級一次展開，全部檢視所有項目，又能節省許多空間。   缺點 : 使用者可能會被內容與選單區塊所混淆，並且在不同解析度的裝置下無法確保能完整的呈現選單。   適用情況 : 適合有許多層級選單的中大型規模網站。  

#### (六)移除選單

考量到行動裝置上不需要桌面版的導覽系統，僅留下所需之功能選單。

 [![responsiveNav-6](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-6.jpg) http://www.authenticjobs.com/](https://oberonlai.blog/wp-content/uploads/2013/08/responsiveNav-6.jpg)

    優點 : 沒有任何選單會佔使用者的螢幕空間。   缺點 : 容易讓使用者迷路。   適用情況 : 對於行動裝置使用者的目的性非常明確，不提供他們不需要的選單以降低干擾。  

### 小結

對於行動裝置使用者而言，短暫、快速、片段的使用特性，往往影響他們瀏覽網頁的行為模式，因此如何讓使用者快速找到內容，以及如何透過導覽系統協助他們，此乃響應式選單設計的目標，然而除了全域導覽外，亦可利用區域導覽吸引使用者瀏覽更多內容，或是併用各類型的模式，來設計更多樣化的使用體驗。