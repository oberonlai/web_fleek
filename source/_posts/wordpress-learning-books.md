---
title: 12 本 WordPress 開發者必讀書籍
date: 2021-10-22 11:00:00
categories:
- WordPress 開發
tags:
- 電子書
- 學習資源
etoc: false
---


前幾週聊到我的 WordPress 開發學習歷程，基本上都是遇到自己過不去的關卡時，靠著搜尋找到語法然後複製貼上拼湊成自己需要的答案，這樣的過程持續了很多年，直到每次要重新回去修改舊的程式碼發現完全改不動或是完全不記得該如何修改，以及改東牆壞西牆的狀況ㄧ再發生後，我才決定回歸基礎，改變先前只想快速找到答案的心態。

如果你也是跟我一樣的歷程，我是覺得少了基礎直接去實戰也沒有什麼不好，我是那種只看理論就會睡著的學生，當我不知道學這個東西可以用在哪裡時，我就完全沒有學習的動力，所以當我經過多年實戰再回頭看這些基礎時，反而會覺得好像重新認識了工作，不管是對於工具的理解還是每一行程式碼代表的意義，進而體會到自己的不足，才能保持著每天都在學習的心態來面對今天的工作。

<!--more-->

在學習 WordPress 開發基礎的路上，發現原來官方有提供這麼多的學習資源，也發現有很多大大寫了很棒的書籍來教我該如何把程式寫得更好，尤其是關於物件導向我一開始完全不知道它在幹嘛，只是依樣畫葫蘆用 class 把所有 function 包起來，但隨著改變作法後就會產生疑惑，這邊為何要這樣寫？那邊那樣寫的用意是什麼？這些疑惑以前的自己完全不在意，因為只要功能可以正常運作就好。

當地雷埋夠多哪天不是被客戶就是被自己踩到，踩到後實際除錯時就開始咒罵上一個經手的工程師，寫這什麼程式碼，語意不明、重複的東西一堆、不相關的函式都塞在一起、叫我阿罵來寫都寫得比這個好...，然後才想起來上一個經手的工程師就是自己XDD

所以我第一個開始意識到的事情就是自動化測試，因為我發現自己花太多時間在除錯了，接下來開始去理解物件導向背後的意義，然後試著使用一些設計模式，去處理更多更複雜的問題，因為現在接的案子跟以往幾個月就能結束的不同，目前手上的客戶都已經合作超過一年以上，這期間我不斷重構一年前寫的程式碼，以便於能夠更彈性的加入新功能而不會造成既有功能的損壞，這對我來說是一個大挑戰。

總之從意識到自己寫的程式會爆炸以及接案模式的改變，讓我有了轉變的契機，以下我就五個面向來推薦值得一看的書籍，這些書籍不一定要照順序看，每個時期會在意的問題不同，從自己最在意的問題先下手即可，時間久了自然而然會衍伸到其他的知識。

另外書目中有不少是官方文件，為了方便閱讀，我花了好幾個月的時間整理成電子書以及 PDF 的格式，如果你跟我一樣不想在下班後用太多螢幕，可以用電子閱讀器來念，官方文件的電子書版本可以在這邊下載：https://handbookspub.oberonlai.blog

## WordPress 開發基礎知識書籍推薦

#### Coding Standard Handbook
![](https://handbookspub.oberonlai.blog/img/1.06f8af9e.png)

https://developer.wordpress.org/coding-standards/

為了讓程式碼更好閱讀，遵循著一致的規範非常重要，可以讓程式碼經過多年後依舊有可閱讀性，當然要記得所有的規範是不太可能的，在實務上可以透過一些小工具來讓我們的程式碼符合 WordPress 的規範，但小工具並非萬能的，還是需要一些基礎知識才能完全符合規範。


#### Common APIs Handbook
![](https://handbookspub.oberonlai.blog/img/3.5d1f2f90.png)

https://developer.wordpress.org/apis/

開發 WordPress 常用的 API 都寫在這本裡面了，以往在做相關功能的時候都是一知半解，所以在看這本時我一直發出驚呼聲，想不到原來還有這個跟那個，驚呼到連老婆都來關切發生了什麼事XD，雖然看完之後還是會忘記，但當搜尋時自然就會用對關鍵字，甚至想起好像在哪邊看過這支 API 的說明，這時再回去翻就會感謝自己有看過這本。

#### Discover object-oriented programming using WordPress

https://carlalexander.ca/object-oriented-programming-wordpress/

由 Carl Alexander 所撰寫的付費電子書，裡面使用生動的例子來說明何謂物件導向以及相關的特性，並且將 WordPress API 封裝成類別使用，很多範例程式碼都可以直接拿來實戰，我覺得是一本 CP 值很高而且寫得非常清楚的書籍，英文用字都不會太難，這本幫助我開始使用物件導向來做 WordPress 開發。


## WordPress 佈景主題開發書籍推薦

#### WordPress 客製化實戰講座：自製佈景‧外掛‧社群行銷‧SEO優化

![](https://cf-assets2.tenlong.com.tw/products/images/000/110/851/medium/9789863124610_bc.jpg?1525540185)

https://www.tenlong.com.tw/products/9789863124610?list_name=srh

我很喜歡日本作者寫的電腦書籍，常可以看到很多生動的圖片來解釋抽象的概念，這本也不例外，他把開發佈景主題所需的知識講解的非常清楚，幫我釐清了許多每天在用卻一知半解的概念，如果你要設計自己的佈景主題大推這本！

#### Theme Handbook

![](https://handbookspub.oberonlai.blog/img/4.defeea9e.png)

https://developer.wordpress.org/themes/

涵蓋所有開發 WordPress 佈景主題的所有面向，包含了主題結構、範本層級、安全性、發佈主題等等，我會建議先把上面那本中文的看完後再來看這本會比較有概念。

## WordPress 外掛開發書籍推薦

#### Building Web Apps with WordPress

![](https://cf-assets2.tenlong.com.tw/products/images/000/127/746/medium/51v-okZykeL._SX379_BO1_204_203_200_.jpg?1574929377)

https://www.tenlong.com.tw/products/9781491990087

由 Brian Messenlehner 與 Jason Coleman 合著的好書，內容十分豐富，同時也提供了程式碼範例，雖然不少章節跟 WordPress Handbook 有所重疊，但解釋的方式比較容易理解，再搭配實際的案例更能幫助記憶，英文用字也很淺顯易懂，我有整理了四篇書摘：https://oberonlai.blog/tw/building-web-apps-with-wordpress/

#### Plugin Handbook
![](https://handbookspub.oberonlai.blog/img/5.7803c2e3.png)

https://developer.wordpress.org/plugins/

WordPress 外掛開發手冊，介紹開發外掛所需要的各種知識，但缺少實際的應用案例，我會建議先把上一本看完後再用這本來補足相關知識。

## 物件導向開發相關書籍推薦

#### PHP 大師之路 - 開源的技術淬練

https://ithelp.ithome.com.tw/users/20111119/ironman/3269?page=1

這是由 Terry Lin 在 2020 年的鐵人賽參賽作品，從一開始的 PHP 設計模式切入，到 PHP 套件的開發，最後整合成 WordPress 的外掛，這 30 篇文章非常精彩，有非常完整的說明，但因為作者過於忙碌，要出版成冊只能靠緣份了XD

#### PHP OOP Way

![](https://oberonlai.blog/wp-content/uploads/2020/09/hero.jpeg)

由 Segrey Zhuk 撰寫的付費電子書，專注在解釋 PHP 的物件導向，比 Discover object-oriented programming using WordPress 更深入的介紹，而且可能是因為讀了 Carl 的那一本，PHP OOP Way 讓我有更上一層的感覺，我有整理部分書摘：https://oberonlai.blog/tw/php-oop-way/

#### 大話重構

![](https://cf-assets1.tenlong.com.tw/images/94935/medium/PG21454-1.jpg)

https://www.tenlong.com.tw/products/9789864340460?list_name=srh

這本不講 PHP，裡面範例都是 Java，唸起來有點辛苦，但是對於重構的概念深深吸引著我，因為每天在做的事就是不停修改自己上禮拜寫的程式碼，書中提到的一個核心就是加新功能前先停下來，檢視目前程式碼的結構，調整成可以容納新功能的狀態，完成後再把新功能加進去，這件事對我幫助超大，現在要改之前寫的東西比以前輕鬆許多。



## 其他

在每天的工作中最常碰到的不外乎是 WooCommerce，但相關書籍都還是偏向後台介面的操作而非程式開發，所以主要都還是以 WooCommerce 的官方文件為主

#### WooCommerce Theme Developer Handbook

https://docs.woocommerce.com/document/woocommerce-theme-developer-handbook/

開發 WooCommerce Theme 的說明文件。

#### WooCommerce Plugin Developer Handbook

https://docs.woocommerce.com/document/create-a-plugin

開發支援 WooCommerce Plugin 的說明文件。

剩下的就是直接去爬有上架在 wordpress.org 的 WooCommerce 外掛，直接從程式碼中看他們是如何寫相對應的功能，一開始就直接複製貼上，然後再改成自己要的，從程式碼學習是開源軟體的最大福利。

---

以上是我推薦的 WordPress 開發閱讀書單，如果你也有唸過不錯的書籍想跟我分享的務必跟我說，還是大家現在都是看線上文件不看書的XD，記得好久以前就被一位朋友說過學技術哪有在看紙本書的，看網路才是最新的知識，年紀越大覺得看紙本或電子書才不會被一堆連結分心，你覺得呢？

---

## 徵求合作夥伴

如果你學了一堆 WordPress 開發知識卻苦無機會實戰，或是接案接的不順利常常加班加到往生又請不到尾款，要不要試著一起合作接案看看？我一直想找合作夥伴很久了，但一直沒有遇到適合的人選，如果你有開發過 WordPress 佈景主題或是外掛，對於[時薪制的敏捷式接案模式](https://oberonlai.blog/tw/wordpress-freelance-practice-1/)很感興趣的話，歡迎跟我聯絡，直接回信附上你的部落格連結即可，希望可以找到一起打拼的你！

