---
title: 寫給網頁設計師-如何整合html頁面至Wordpress（三）
tags: []
id: '990'
categories:
  - - WordPress
date: 2013-07-22 12:38:11
---

6. **增加企業部落格文章**\---新增文章功能 企業部落格的內容新增方式，是要以文章為主體，新增方法如下 ![新增部落格文章](https://oberonlai.blog/wp-content/uploads/2011/09/addarticle.jpg "addarticle") 在左方工具列找到文章 > 新增文章，再輸入標題與文章內容，與分頁不同的是，它沒有模版可以選擇，另外它還可以對文章進行分類與下標籤，這對於網站的搜尋引擎最佳化作業提供運用的空間，完成後點選右方藍色的發表按鈕，新文章就可以發表完成。
<!-- more -->
7.**設定網站起始首頁與部落格網址**\---運用外掛Home Page Control 現在我們已經完成的頁面有網站首頁、關於我們、服務項目、聯絡我們，以及剛剛新增部落格的幾篇文章，現在試著連到網站首頁，會發現是以部落格為主要頁面，而不是我們所設計的網站首頁，這時就需要工具來幫忙設定一下，而這工具就是我們之前安裝的Home Page Control。 ![自訂首頁和部落格目錄名稱](https://oberonlai.blog/wp-content/uploads/2011/09/homepagecontrol3.jpg "homepagecontrol3") 在選擇首頁的部份，點選下拉選單，會發現有我們剛剛所建立的四頁分頁，網站首頁、關於我們、服務項目以及聯絡我們，只要選擇網站首頁，按下儲存後，再回到首頁查看，就會發現我們已經成功改變首頁為我們自己設定的網站首頁了。 而自定部落格目錄名稱則是把網址設成你自己想要的，這名稱一定要設，不然你就沒有網址可以連到你的部落格，另外順帶一提網址的結構，我個人是喜歡用我自己命名的分頁名稱做為網址結尾，看起來清爽又乾淨， ![自訂網址結構](https://oberonlai.blog/wp-content/uploads/2011/09/url.jpg "url") 從左方工具選單設定 > 固定網址，勾選最後一個自訂結構，並輸入/%postname%/，這樣我們就可以回去網誌分頁設定目錄名稱 ![設定目錄名稱](https://oberonlai.blog/wp-content/uploads/2011/09/url2.jpg "url2") 點選編輯，就可以更改目錄名稱，建議最好是用英文，像聯絡我們就用contact簡單易懂的單字。看到這邊各位應該知道，這目錄名稱並不是我們通常所使用的資料夾名稱，而是可自行命名的，所以這名稱不用真的符合資料夾的名稱。 能夠做到這邊，基本上網站已經完成八成了，剩下最後的關鍵---導覽列的設定。 <四>設定導覽列 在wordpress 3.0的版本開始，在左方的工具列 > 外觀，找的到選單的選項，如果你沒看到的話，趕緊更新一下版本吧，因為這是wordpress正式邁向內容管理系統 (CMS) 的重大一步，就是因為這超級無敵好用的內建選單功能!!! ![自訂選單功能](https://oberonlai.blog/wp-content/uploads/2011/09/nav.jpg "nav") 以我用過xoops和joomla的經驗來說，wordpress的選單設定實在是設計的最傑出的，簡單說，只有wordpress是從"非工程師"的角度來設計所有的設定流程。 然而雖然3.0版有內建的選單功能，但是你安裝的佈景主題也必須要有設定選單的函式，在這篇文章所提供的這份模版主題，我已經有把它加入選單功能，增加的方法需要修改function.php，對，就是之前我說沒事不要進來，進來會出人命的地方，所以我已經設定好兩組選單，一個是在header的主選單組，一個是在footer的頁尾選單組， 其它類型的選單像是次選單我們可以用Sidebar來設定，因此基本上要動到fucntion.php的機會不高，但我相信各位都有像魯夫一樣不去冒險會死掉的精神，或是有時需要增加特定選單的狀況，我稍後會介紹該如何新增一組選單組。 正常的做法，在我們網誌分頁已經都設定好的同時，相對應的選項程式也會自行產出，我們只要把這些選項加到選單組即可。開始建立之前，我把先把顯示選項調整如下圖 ![點選右上角的顯示選項](https://oberonlai.blog/wp-content/uploads/2011/09/option.jpg "option") 在右上角有一個顯示選項，點下去時上方欄位會整個展開 ![顯示欄位選擇](https://oberonlai.blog/wp-content/uploads/2011/09/option2.jpg "option2") 依照圖中選項來勾選，裡面的設定包含是否要顯示選單的id或class名，以及連結目標(target)等等，並把不必要的欄位隱藏，這樣比較好辦事。 ![建立選單組](https://oberonlai.blog/wp-content/uploads/2011/09/nav2.jpg "nav2") 第一步先建立你的選單組，最好是用英文命名，因為這會是css辨識選單組的名稱。 ![控制選單組位置](https://oberonlai.blog/wp-content/uploads/2011/09/nav4.jpg "nav4") 我建立了一個選單組名為main，左側的佈景主題位置就是可以選擇要把選單組顯示在那一個地方，而這些位置，是透過php的一段語法所控制的，主選單位置語法位於header.php與頁尾選單則在footer.php裡面。 假設現在我們要把剛剛新增的選單組main放在主選單的位置，只要在佈景主題位置主選單的下拉清單就可以看到我們所新增的main，選擇它按儲存，這樣就完成選單組的位置安排了。 ![選擇選單組的位置](https://oberonlai.blog/wp-content/uploads/2011/09/nav5.jpg "nav5") 如圖選擇剛剛所建立的選單組main即可 決定好位置之後，那選單組的選項呢?聰明的你一定有看到左下方的網誌分頁 ![增加選單組的選項](https://oberonlai.blog/wp-content/uploads/2011/09/nav6.jpg "nav6") 我們剛剛所建立的分頁，都顯示在這邊，按選擇全部，並新增至選單，最後儲存選單，不到三秒，選單就搞定了，但是順序有點亂，而且還沒有看到最重要的企業部落格選單，接下來，我們做自訂鍊結的部份。 還記得之前我們在Home-Page Control所設定的部落格目錄嗎?先開個新網頁，去連到你的部落格，譬如這是我的部落格網址，https://oberonlai.blog/wblog，把它複製下來 ![增加自訂鍊結](https://oberonlai.blog/wp-content/uploads/2011/09/nav8.jpg "nav8") 把網址貼到鍊結網址的欄位中，並把選項名稱命名為企業部落格，最後按新增至選單，就完成部落格的連結了。 依照這邏輯，看你是想連google還是yahoo都隨便你，只要把網址貼進去就好，當選項都成功加入選單組之後，我們再來把它整理一下。 ![當前選項順序](https://oberonlai.blog/wp-content/uploads/2011/09/nav9.jpg "nav9") 這是目前的選單順序，我看它很不順眼，我想把網站首頁放第一個、然後是關於我們、服務項目...，最後是聯絡我們，調整的方法非常簡單也直覺 ![拖曳選項改變順序](https://oberonlai.blog/wp-content/uploads/2011/09/nav10.jpg "nav10") 是地，就是脫拖一下這麼簡單，第一次看到這種UI出現在wordpress時，感動的都快要掉淚了，特別是最近在用xoops，還要填數字來改變順序的蠢方法比起來，wordpress根本就是佛心來的，另外也可以用拖曳來決定層級，實在是直覺到爆炸。 ![設定class名](https://oberonlai.blog/wp-content/uploads/2011/09/nav11.jpg "nav11") 按下選項右邊的小三角型，會出現一些選項，包含要讓css辨識名稱，或是網頁的開啟目標都可以設在這邊，每個選項都可以獨立設定，如果要用圖片做選單就非常方便了。 ![完成選單順序](https://oberonlai.blog/wp-content/uploads/2011/09/nav12.jpg "nav12") 完成選單順序，記得點儲存選單，就這樣，打完收工。 其它類型的選單，像是次選單或功能選單等等，我們一律都可以用原有的sidebar來解決。sidebar本來是wordpress設計來建立一些瀏覽部落格的輔助工具，在這個版本之中，也可以把選單組加入到sidebar中，這樣可以更方便的來控制次選單。 接著我們再來新增一組關於我們的子選單，新增選單組請參考上面的步驟。 ![](https://oberonlai.blog/wp-content/uploads/2011/09/nav13.jpg "nav13") 我新增了選單組叫做sub-about，裡面包含了兩個選單，公司歷史與公司文化。 接著選擇左方工具列>外觀>模組，就會看到可以放在sidebar的所有功能。 ![啟用模組功能](https://oberonlai.blog/wp-content/uploads/2011/09/nav14.jpg "nav14") 在這邊可以看到許多部落格會常用到的功能，像是文章彙整、近期迴響等功能，這些模組你有空的自己玩看看，當然也可以用裝外掛的方式進行擴充。 ![將自訂選單加入sidebar](https://oberonlai.blog/wp-content/uploads/2011/09/nav15.jpg "nav15") 現在我們只要找到自訂選單，然後把它優雅的拖曳到Sidebar Widget欄位中， ![sidebar選擇選單組](https://oberonlai.blog/wp-content/uploads/2011/09/nav16.jpg "nav16") 然後就會看到如圖中的選項，如果沒有看到請重新整理一下即可，給子選單一個標題，然後用下拉選單選擇我們剛剛建立的選單組sub-about，完成後按下儲存，即成功新增子選單到sidebar中。 假設我們把其它頁面的子選單也都完成了，要來陸續增加到sidebar之中，但你一定會發現，sidebar雖然可以增加很多組選單組，去前臺的看的時候，會發現每一個分頁都是共用所有一樣的sidebar，也就是說，在關於我們可以看到服務項目的子選單，在服務項目也可以看到關於我們的子選單，但這世界不應該是這樣的啊啊啊啊! 當有人說一樣的東西完美無缺的時候，請小心這個人，它不是業務員就是購物頻道主持人，但我都不是，所以我要在這邊來講講wordpress的壞話，那就是他無法從模組這邊設定我那一個分頁要套用那一個模組，這是內容管理系統的基本功能，wordpress如果要朝標準的cms發展，就不能不加入這樣的功能。 之所以抱怨這麼多，是因為之前真的被它玩到了，為了解決不同分頁有不同模組的問題，我甚至一度想直接把次選單做在網誌分頁裡面，把天殺的sidebar踢的遠遠的，雖然很蠢，但至少控制權在自己手上。 後來谷了許多資料，光是這種功能要怎麼下關鍵字查詢就已經夠難了，有找到的多半不是我要的效果，最後在卡了將近三天左右，總算找到了不甚完美的解決辦法，就是安裝一個外掛，叫做Widget logic。 [下載點](http://wordpress.org/extend/plugins/widget-logic/) 以一般安裝外掛的方式安裝，完成後會在模組的Sidebar Widget裡面，再展開其中一個我們所做的選單組，就會發現多了一個欄位叫做widget logic ![widget logic](https://oberonlai.blog/wp-content/uploads/2011/09/nav17.jpg "nav17") 大家一定會困惑，單單多了一個這樣的欄位，要如何去判斷那一頁該出現相對應的sidebar，而這也是為何它不是完美解的原因，因為它要自行輸入判斷的語法.....暈~ [艾德的部落格天空](http://edblog.net/archives/2063)是我找到這個解決辦法的地方，沒有他我現在大概還在原地吃屎，他提供了一些判斷的語法。 ![widget logic的判斷語法](https://oberonlai.blog/wp-content/uploads/2011/09/nav18.jpg "nav18") 看似有點複雜但實際上很好懂，我們想要的功能在第三點---is\_page('分頁名稱') ，我們把網誌分頁網址的結尾，也就是我們自己命名的名稱當做判斷的依據， ![widget logic的判斷依據](https://oberonlai.blog/wp-content/uploads/2011/09/nav19.jpg "nav19") 分頁"關於我們"叫做aboutus，那就回到模組的widget logic中，寫is\_page('aboutus') ， ![寫入判斷語法](https://oberonlai.blog/wp-content/uploads/2011/09/nav20.jpg "nav20") 就是這樣 如果是同一組選單組，想要同時出現在兩個或三個分頁中該怎麼辦?我嘗試在google analytics裡面需要常常用到但也是把我搞得半死的正規表示法，或的表示法是，也就是你的鍵盤的shift+\\，所以如果我想讓同一個選單組出現在兩個分頁中，只要寫成這樣 **is\_page("aboutus")is\_page("contact")** 依照以上的解法，總算可以這個問題，但理想中還是希望可在設定Sidebar Widget的同時，多出一個欄位選擇出現位置，使用上會較為直覺，如果各位知道有更好的方法請務必讓我知道，不然只能期待下個版本推出時能夠有所改善了。 關於選單的部份，只要依循著上述的使用流程，基本上都能完成我們所需要的功能，但有時總是會有例外的情況，所以我再把關於wordpress的選單設定，解釋其程式碼所負責的部份，讓日後各位可針對自己的需求來修改。 首先先打開function.php，可以在第三行找到一個註解-增加選單功能 ![解釋nav function](https://oberonlai.blog/wp-content/uploads/2011/10/nav21.jpg "nav21") 這邊有句判斷式，當函式register\_nav\_menus存在時，增加一組陣列，第一個資料叫做header-menu，辨識的名稱為主選單，第二個資料叫做footer-menu，名稱為頁尾選單 大家在看這一段的時候，重點就是header-menu和footer-menu，而它們的名稱有沒有似曾相識? ![選單的容器](https://oberonlai.blog/wp-content/uploads/2011/10/nav22.jpg "nav22") 這兩個名字主選單與頁尾選單就是定義在這一頁之中會出現的佈景主題位置 在funtion.php之中定義的這兩個選單，主要是將選單的功能開啟，雖然它是以選單的功能和位置來命名，但在這邊的設定最主要是在告訴系統我們有兩套選單。 'header-memu' => '主選單' 引號中的名稱皆可自行定義，前面的英文是在調用這組選單時的辨識名稱，而後面的中文字則如上述所提到的是控制後臺選單的位置名稱，如果你覺得兩組不夠應付你網站中的選單，只要在這邊增加 'your-menu' => '你的選單', 即可，看需求來增加。 接著我們來看在開啟選單功能後，該如何把這些選單定義到我們希望它出現的位置，先以header-menu為例，這是主選單的部份，我習慣的做法是把主選單放在header.php，所以先打開這個檔案， 在第55行可以找到定義選單。 ![header-menu的位置](https://oberonlai.blog/wp-content/uploads/2011/10/nav23.jpg "nav23") wp\_nav\_menu是在wordpress中調用選單的函式，後面接的 'theme\_location' => 'header-menu' 則是代表套用這個佈景主題所擁有的選單，也就是我們自己定義的header-menu。 以此類推，foot-menu也是一樣的寫法，打開footer.php，在第5行可以看到同樣的語法 ![footer-menu的位置](https://oberonlai.blog/wp-content/uploads/2011/10/nav24.jpg "nav24") 跟上面一樣的wp\_nav\_menu，只是換成footer-menu 透過以上的解說，我們就能彈性控制選單的位置，譬如今天我的主選單是在側欄而不是在上方，就可以把wp\_nav\_menu這一段整行剪下貼到index.php裡面的某個你自己定義的div中，或是要定義一個頁面中的常用功能選單，都可以運用這樣的方式來做設定，相信已能滿足絕大多數的需求。

## <五>設計css

當把所有網誌分頁的html都建置完成後，最後再來設計css。wordpress的css很單純，就是一個檔案style.css，通常我習慣的做法是，使用chrome瀏覽器的檢視元素 ![chrome瀏覽器的檢視元素](https://oberonlai.blog/wp-content/uploads/2011/10/css01.jpg "css01") 使用google瀏覽器chrome，打開頁面，在空白的地方按右鍵，選擇最下方的檢視元素 ![檢視元素內容](https://oberonlai.blog/wp-content/uploads/2011/10/css02.jpg "css02") 當滑鼠移到某段div時，網頁中會出現相對應的區塊，而在檢視元素中點擊某段標籤，它也會呈現出相對應的css，並且可以勾選或關閉來即時檢視css的套用效果，十分的方便，要修改時再去尋找相對應的id或class即可。 在style.css的檔案中，我把初始化的語法用import來匯入reset.css，如果各位有自己的初始化語法，就貼在reset.css即可，剩下的就是在把你寫好的css全部貼過來吧!

## <六>結論

每一套open cms都有自己的特性，也都有自己的支持者，但以一位網頁設計師的角度而言，我還是會推薦wordpress，因為它建立頁面的方式最直覺，在選單的控制上也非常彈性，外掛的數量也是多到爆炸，要能夠讓我相信一套cms很好用，只要頁面好增加，選單好控制就夠了，其它都是附屬的附加價值。 不像xoops增加頁面是要用模組的概念，沒有裝模組你就沒有頁面，而joomla則是要分單元，分類、文章，選單的控制更是麻煩，相信有做過的人就能理解我在說什麼。 所以為了讓還未碰觸過open cms的朋友們少走些冤枉路，我以自身的經驗來分享wordpress如何快速的與我們現有的作業結合，希望能對大家有所幫助，有任何疑問或是有更好的做法也歡迎留言給我，畢竟我也完全沒學過程式，很多自己摸索的不一定是最好的方法，還請各位不吝指教，先感謝各位了!