---
title: 【 書摘 】PHP OOP Way
tags:
  - OOP
id: '3898'
categories:
  - - PHP
date: 2020-09-12 10:52:08
etoc: true
---

受惠於有無數的前人願意分享以及開源自己的程式碼，才能讓我這種只會去 Stackoverflow 找答案複製貼上的偽工程師開始逐漸學習 PHP，因為當貼過來的東西不能用或是需要修改的時候，就必須要去理解自己到底貼上了什麼碗糕。

對於長年下來習慣 trial and error 寫程式的我來說，當要開發一個功能時，我會先就這項功能的每個步驟開始 Google，譬如要做一個會員註冊系統的後端程式，我可能會先查這些問題：「如何將資料傳送進資料庫」、「如何進行資料庫連線」、「如何判斷資料庫資料寫入完成」等等，然後根據我查到的程式碼依序貼入編輯器裡面，接著見證奇蹟的時刻～

<!--more-->

想當然一定會到處噴錯無法執行，接下來就開始一行一行檢查出錯的地方，然後把錯誤訊息再丟進 Google 裡面查解決辦法，就這樣用老牛拖車的方式把這個功能給兜起來，我常覺得自己不是在「寫」程式而是在「拼」程式，但透過這樣「拼」的過程，也慢慢培養出一些嗅覺，知道哪樣拼不行、應該該怎麼喬才會 Work。

這就像完全不會說英文的人住在美國，為了生活必須要先學會用英文滿足自己吃喝拉撒睡的基本需求，我學程式的過程大概就是這樣的感覺，為了要完成工作領到薪水吃飯，用盡各種方法都要把客戶要的東西生出來，拜 WordPress 所賜，有太多神人貢獻自己的知識、Know How、以及「外掛」XD

但街頭智慧總是會有遇到瓶頸的時候，當一個專案的規模變大、功能變得複雜，或是要回去修改自己 N 年前寫的程式碼，總是會讓我痛到吱吱叫，所以為了要讓自己少叫一點，開始乖乖把 PHP 的基礎知識再重新學習，然後了解該如何組織檔案結構，讓未來的自己或是接手的人好過些。

學習基礎的過程很辛苦，因為我超不愛看一堆理論解釋，只要東西能跑得出來我要的效果就好，管他是怎麼實現的，但逼著吞的結果，卻常常有意外的發現，像是發現這個東西竟然可以這樣用、原來還有這樣的語法可以簡化原本的寫法，因此為了這些出乎意料的小驚喜，還是乖乖的 K 了下去...

但其中我對於物件導向以及設計模式這兩個東西我是怎樣也吞不下去，市面上的 PHP 書籍多半都是參考書，讀起來都很像考試用的，至於 Google 能找到的中文資料只讓我對這兩件事心生畏懼：

> 在自然語言中，我們理解抽象的概念是，一個物體的一種大的描述，這種描述對某類物體來說是共有的特性。那麼在PHP中也是一樣的，我們把一個類進行抽象，可以指明類的一般行為，這個類應該是一個模板，它指示它的子方法必須要實現的一些行為。 

...

為什麼每個中文字我都看得懂，但組在一起後我就沒半句看得懂...

後來我發現很多專有名詞都是中國用語，而這些寫法跟台灣用語完全不一樣，有時候就算是台灣工程師寫的文章，也會因為每個人的背景不同而有不一樣的講法，像是 Class 在中國叫做「類」，台灣通常叫做「類別」，OOP 中國叫做「面向对象」，台灣叫做「物件導向」，某些技術論壇會直接把國外的文章 Google 翻譯成簡體中文，然後再將其繁體中文化，這種東西我看得懂才有鬼啊 😱

後來才知道洋人的玩意兒就直接去看洋人寫的比較快，雖然英文閱讀對我來說很吃力，總是要拿著手機在旁邊查單字，一堆單字查了之後組成句子還是看不懂，但回想起拼湊程式碼的日子，這樣的學習模式自己再熟悉也不過了，所以我最早先從 WordPress Handbooks 開始看，為了護眼還整理成電子書版本，一共有七本，

然後又看了 Building Web Apps with WordPress 還整理了幾篇工作上能直接派上用場的書摘，另外又買了這本 [Discover object-oriented programming using WordPress](https://carlalexander.ca/object-oriented-programming-wordpress/)，逐漸對於 OOP 到底該怎麼在實務上運用有了一些些模糊的概念，也對於這個領域的英文單字有了基礎的熟悉度。

而這個月在 K 的這本由 [Segrey Zhuk](https://twitter.com/zhukserega) 寫的 [PHP OOP Way](https://leanpub.com/phpoopway) 好像有把我更敲醒了一點，秉持著有紀錄才是有確實內化成自己東西的精神，我會試著用實務上的經驗來整理書中的知識，希望可以帶領自己走過學習 OOP 的門檻。另外為了能正確表達專有名詞，我會註記原始的英文單字以便對照。

![](https://oberonlai.blog/wp-content/uploads/2020/09/hero.jpeg)

好長的前言，本篇主角終於現身了

## 使用方法來組合屬性

物件導向程式開發最為人所熟知的就是「封裝」的概念，關於它的定義不太好記，但常用的話大概會知道它是關於把事物的原理隱藏在一個黑箱裡面的過程，就像我在煮飯時按下電鍋的按鈕，時間到飯就會煮好了，而這中間的是如何實現的我不需要去理解，更不用去自己實作這個過程。

但為何這個概念如此重要？難道不能直接把所有屬性都讓人可以公開存取嗎？首先，我們要了解類別在物件導向程式開發中所扮演的角色：它是所有實體物件的設計圖，而它是被設計用在特定用途上的，所以我們必須要確保它能正確的被使用，而如何使用就是透過操作介面，就像是電鍋的煮飯按鈕一樣。

當我們決定哪些功能要做成可以被使用者操作的介面時，這樣的設計過程就是封裝，而封裝的主要目的就是要限縮介面，以避免使用者誤用，想像一下，如果電鍋設計師決定讓使用者自行控制溫度，那麼在煮飯時就要隨時調整溫度，萬一跑去炒個菜或是收衣服沒留意溫度，飯可能不是冷的不然就是燒焦了 > <

離開廚房回到程式開發現場，當我們有一個類別叫做 Client，然後根據這個 Client 類別設計出實體物件，而這個實體物件就能依照 Client 類別所設計的方式來讀取屬性或功能。想像這個 Client 是一位站在你眼前活生生的真人客戶，你可以看到他的臉、衣服、眼睛顏色，你可以跟他說話、問他問題或是得到答案，但你不會看到他的想法、體溫、以及血壓。

這些特徵對於客戶本身非常重要，但卻沒有必要讓別人知道，尤其你跟他只是在閒聊的時候...

舉例來說，我們有一個非常簡單的類別：User 來處使用者資訊，它有三個屬性：姓名、電子郵件與年齡，我們將在沒有封裝概念且完全公開屬性的條件下開始，在建立實體物件的時候我們希望這三個屬性是必要的，所以在建構式函式宣告時需要帶入這三個屬性的參數：

```php
class User {
  public $name;
  public $email;
  public $age;
  
  public function __construct( string $name, string $email, $int $age ) {
    $this->name = $name;
    $this->email = $email;
    $this->age = $age;
  }
}

$user = new User( 'John', 'john@example.com', 18 );
```

把類別設計完成後，我們現在有一個商業邏輯：檢查該名使用者是否已滿 18 歲，滿了才能瀏覽 18 禁的內容：

if( $user->age < 18 ) {
  throw new TooYoungException ( '等你滿 18 歲再來吧～' );
}

上面的寫法看起來很合理，直接存取 $user 的公開屬性 age，就能判斷是否有滿足特定年紀，而這樣的判斷式會遍佈整個程式裡面，只要有需要判斷 $user 年紀的地方就需要下這一個判斷式，假設到了下個月，需要改成 21 歲時該怎麼做？對，這時候就只能用編輯器進行全站搜尋取代 18 這個數字。

另一個問題在於任何人都可以隨意修改 $user 的年紀，因為這個屬性是可以公開存取的，所以在建立實體之後，還是能夠被進行複寫，這是不合理的：

$user = new User( 'john', 'john@example.com', 18 );
$user->age = 21; // 可以直接被覆寫

還有一個更嚴重的問題：雖然年齡這個屬性被強制要求於建構式函式中必填，但還是缺少了參數驗證的機制，雖然我們已經有用 int 來確認 $age 參數是整數，但卻無法驗證年齡是否為負數：

$user = new User( 'John', 'john@example.com',  -20);

以上這些問題的主要原因在於 User 類別沒有把屬性綁定在方法裡面，這個類別看起來比較像是某種類型的資料結構而非真正的類別。所以接下來，我們開始來設計介面，第一個步驟就是先把隱藏所有的屬性，以及新增方法來取得 $user 的年齡：

class User {
  private $name;
  private $email;
  private $age;
  
  public function \_\_constructor( string $name, string $email, int $age ){
    $this->name = $name;
    $this->email = $email;
    $this->age = $age;
  }
  
  public function getAge(): int {
    return $this->age;
  }

我們先透過宣告私有屬性，修正了公開屬性可以直接存取因而被覆寫的問題，下一步是把年齡的判斷邏輯隱藏在類別裡面，對於實體物件來說，它不用知道具體的年齡，只要知道他是否滿足特定年紀即可：

class User {
  ...
  public function isYoung(): bool {
  return $this->age < 18;
  }
}

新增 isYoung() 這個方法就能解決之後年齡要變動的問題。現在 User 這個類別看起來比較像真正的「類別」了，當我們要檢查 user 的年紀不用直接存取屬性來進行判斷，而改用 isYoung() 來檢查即可。

if($user->isYoung(){
  throw new TooYoungException;
}

透過以上的改寫就能把商業邏輯與實現細節隱藏在類別之中，而這就叫做「封裝」。我們把屬性的存取放在方法裡面，這些屬性對於實體物件來說是無從得知的，實體物件只能透過方法來讀取它需要的資料。但在實務上該如何判斷哪些屬性是公開、私有或是可繼承？我們應該全部都用私有屬性然後透過方法來設定其值嗎？

設計類別的時候我們的主要目標是要保護屬性避免被實體物件直接存取。所以我們可以從只有私有屬性開始，藉此來確保安全性。如果要提供公開屬性最好有明確且必要的理由。另一方面，千萬別認為只要用了私有屬性跟公開設定方法就可以不用做資料驗證動作：

class User {
  private $name;
  private $email;
  private $age;
  
  public function \_\_constructor( string $name, string $email, int $age ){
    $this->name = $name;
    $this->email = $email;
    $this->setAge($age);  // 改使用 setter 來設定年紀，才能加入驗證機制
  }
  
  ...
  
  public function setAge( int $age ): self {
    $this->age = abs($age);
    return $this;
  }
}

透過以上的修改，就能把不需對外公開的屬性隱藏，並且使用公開介面來設定這些屬性。

## 定義新的資料類型

關於物件導向的抽象化概念很多人都聽過，但卻很難解釋的很清楚，常聽到的解釋是：「抽象化是一種可以讓程式碼重複使用的概念」，而這也是我們從網路上學到關於物件導向程式設計的知識。如果你真的需要重用程式碼，建立抽象類別並且繼承它，這樣就寫完收工了，但，真的是如此嗎？這邊有個陷阱：有 N 種方法可以達成重複使用程式碼的目的，而使用抽象類別是最糟糕的一種XD

> 關於抽象化的概念光看名字就覺得很抽象不好理解，這邊借用科技島讀第 454 期的文章「[掌握複雜系統的利器 — 抽象化思考](https://daodu.tech/06-11-2020-manage-complex-system-abstraction)」來說明：中文將 abstraction 翻譯為「抽象化」不太貼切。這裡的 abstract 比較接近「摘要」，因此翻譯為「摘要化」或「本質化」會更精準。摘要的目標是提綱挈領，找出系統的核心，是具體的；而不是像「抽象」聽起來是模糊、不具體的。
> 
> 我喜歡科技島讀 Michael 對於 Abstract 的詮釋，很精準的點出抽象類別的真正含義，它是一種資料類型的呈現方式，就跟我們常用的字串、整數、布林值一樣，是一種很純粹且具體的事物，我們不會說字串、整數很抽象，因為我們每天都用到那頭去，因此要把 Abstract Class 看成是一種資料類型，後文會提及更多這樣的觀念。

讓我們從另外一個方向來思考：PHP 作為一種程式語言，它有許多不同的資料類型，像是整數、浮點數、字串、陣列等等，當需要做數學運算時，我們會用整數、浮點數，當需要邏輯判斷時，我們會用布林值，這是大家都知道的基本觀念，因為我們每天都在用，連想都不用想就知道該用誰。

有了這些基礎類型後，我們就能處理更複雜的資料。舉例來說，像是需要儲存使用者的資料，當然，我們可以用陣列的形式來儲存：

$user = \[
  'name' => 'John',
  'email' => 'john@mail.com',
  'age' => 30
\];

這樣的資料類型雖然簡潔但卻不實用，因為它只能存放屬性而沒有定義方法，這時候就是類別派上用場的時候。我們可以把它視為一種新的資料類型，在 PHP 內建的資料類型裡面，並沒有一種類型叫做使用者，所以我們使用類別來自行建立，我們可以設計屬性及方法，未來就可以像用字串或是陣列一樣使用它。

class User {
  ...
}

$user = new User();
$user->do\_method();

一個新的類別定義了介面來讓其他人可以使用這個資料類型，當我們設計了公開方法後，就像在對使用這個資料類型的工程師說：「嗨！歡迎使用這個新的資料類型，只要遵循特定規則你就知道怎麼使用了～」，當我們把類別視為資料類型時這是非常重要的觀念，類別的公開介面就是一連串的規則，它定義了該類型如何使用。

以 PHP 內建的資料類型來說，我們知道如何讀取陣列值的用法，如果不照著它的規則來用，就絕對會噴錯：

$var = 3;
echo $var\[0\]; // 應該不會有人這樣用吧？

$var\_arr = \['a','b','c'\];
echo $var\_arr\[0\]; // 陣列就是這樣用的啊

每一個資料類型都有自己的一套的行為，也就是公開介面所提供的操作方式，這些行為可不能被隨意改變，不然就會是場災難，保持一致的使用介面是身為資料類型最基本的觀念之一。

所以當我們把抽象類別也看做是一種資料類型時，上述的觀念也同樣適用在它身上，在我們設計類別的時候，我們就有責任必須要寫出一致的使用介面供他人使用，而 PHP 提供了一些規則協助我們設計類別，帶有關鍵字 abstract 的類別無法建立實體，而方法應該被定義在子類別之中。

當我們使用繼承時，我們的職責是要遵守父類別的介面，下一個章節會討論更多關於繼承的部分。總括來說，封裝與抽象化是互相關聯的，抽象化是移除不需要的細節以聚焦在事物的本質( 本質化 / 摘要化 )，而封裝是在抽象化的過程中會使用到的一門技術，它將細節隱藏起來，只提供公開介面來讓使用者進行操作。

抽象化是一個較大的範疇，如果沒有封裝是無法實現抽象化的。

## 關聯資料類型家族

當聽到繼承的時候你腦海中第一個浮現的印象是什麼？無疑是程式碼的重複使用，對吧？讓我們忘記這事從頭開始吧～

在上一個章節中提到，我們把抽象化看做是定義一個新資料類型的過程，然後使用 extend 關鍵字來建立子類別，而這也就是所謂的繼承，透過繼承可以判斷類別之間是否有親戚關係，子類別會擁有父類別所有非私有的屬性與方法，所以子類別是其父類別更明確具體規格的版本( 青出於藍勝於藍XD )。

> 晚上睡不著的時候想到：如果 Abstract 是本質化、摘要化的意思，那麼繼承抽象類別的子類別，比較像是把抽象類別的細節完整描述出來，就像做報告的時候一樣，第一頁通常會是大綱或目錄，然後再從目錄去延伸內容，而這延伸的內容就是子類別要做的事，如果以這樣的關係來發想，父類別可能比較適合稱作為「根類別」或「主類別」，所有繼承它的類別則叫做「延伸類別」。

雖然子類別擁有自己具體的規格，但使用者基本上還是認定它是父類別的延伸，因此父類別能用的方法也能完全無痛的用在子類別身上，以程式碼來說明：

class Task {
  private $isClosed;
  private $closedAt;
  
  public function close(){
    $this->isClosed = true;
    $this->$closedAt = date("Y-d-m H:i:s");
    
    return $this;
  }
}

class Project extends Task { }

範例中有一個父類別叫做 Task，子類別 Project 繼承了所有非私密屬性與方法，使用者會認定這兩個類別都是一種新的資料類型叫做 Task，而能使用的方法為 close()。接下來考慮另外一個類別 User 要使用 Task：

class User {
  public function completeTask( Task $task ){
    $task->close();
  }
}
$user = new User()
$user->completeTask( new Task() );
$user->completeTask( new Project() );

當子類別 Project 沒有定義任何新的方法時，上述的程式碼運作正常，但設計子類別就是為了更多更具體的功能所以才需要它，但把新的方法加入子類別之後，很容易就會開始造成問題：

class Task {
  private $isClosed;
  private $closedAt;
  
  public function close(){
    $this->isClosed = true;
    $this->$closedAt = date("Y-d-m H:i:s");
    
    return $this;
  }
}

class Project extends Task { 
   public function close(){
     parent::close();
   }
}

// Client Code
class User {
  public function completeTask( Task $task ){
    $task->close();
  }
}
$user = new User()
$user->completeTask( new Task() );  // 正常運作
$user->completeTask( new Project() ); // 直接炸裂

發現到最後一行為何會炸裂嗎？Project 新增了與 Task 相同的方法名叫做 close，然後執行父類別的 close，但因為 Project 的 close() 沒有回傳值，而父類別是有的，在子類別沒有遵循父類別的情況下因此造成錯誤。子類別繼承父類別的當下，子類別就應該要遵守父類別的規則，所以當我們在子類別加入新的方法時要非常小心。

這種回傳資料的錯誤可以加上類型提示來讓除錯容易得些，程式碼編輯器可以透過這些宣告來提醒你錯誤的地方：

class Task {
  private $isClosed;
  private $closedAt;
  
  public function close(): self { // 加入 self 來提示回傳的是類別本身
    $this->isClosed = true;
    $this->$closedAt = date("Y-d-m H:i:s");
    
    return $this;
  }
}

class Project extends Task { 
   public function close(): void { // 加入 void 來提示沒有回傳任何值
     parent::close();
   }
}

另外一個關於繼承的重要議題是繼承的深度，也就是這個家族有幾代同堂，除了子類別外，是否還有孫類別、曾孫類別、甚至是曾曾孫類別，實務上的建議是能夠越小越好，小家庭會讓事情比較單純，不然要繼承遺產的的話就很麻煩XD

減少繼承的深度可以比較容易掌握每一代之間的關係，如果上面 Project 的範例有繼承到曾曾孫類別，這樣我們必須要橫跨好幾代才能找到原來問題是出在子類別，當程式龐大時可就不這麼容易找到了。

或是我們也有可能會在繼承的類別中覆寫父類別的方法，以及忘記寫父類別中必要的方法，當這些錯誤發生而繼承的深度又太深時，都會讓除錯非常痛苦。所以理想的深度大概是兩到三代，再多就失控了。

繼承有很多好處，像是將類別群組化以及覆寫被繼承的方法，接下來我們要討論繼承最為人所熟知的用途：程式碼重用。對於程式碼的重複利用之所以會用繼承的方式來實作，是因為很多 MVC 的框架都是這樣做的，想要新增一個 controller？繼承 contorller 父類別即可～

namespace App;

use Illuminate\\Database\\Eloquen\\Model;

class User extends Modle {
  ...
}

使用框架的好處是當需要某些功能時，使用 extends 關鍵字來繼承框架已經設計好的父類別，在大部分情況下這樣做沒問題，但繼承的子類別未必會變成 model 或 view，所以當下次為了要使用某些實用的方法而繼承父類別建立子類別時，要記住這些類別都是一種資料型態，不然很容易會遇到上面提及的問題。

同時要記得要盡量維持淺薄的繼承關係，只有父與子兩代是最好的深度。當然，不是所有情境都能符合這個原則，但如果要發展更多階層，最好有明確的理由以及知道自己在做什麼。

## 盡可能使用 Final 關鍵字

當新增類別的時候，全部使用 final 關鍵字來進行宣告，來確保該類別是不能被繼承的，除非這個類別是被設計來給其他類別來繼承的：

final class User {
  ...
}

使用 final 關鍵字最大的好處是不允許子類別的存在，因此沒有任何人可以改變它的行為。當沿用一個類別的時候就會把封裝的概念給破壞掉，因為子類別可以看到父類別的內部行為並且複製取代，就像前面提到的 Projcet 與 Task 這兩個類別一樣，當子類別複寫父類別的方法時就很容易造成錯誤。

同時，final 關鍵字也避免讓我們使用繼承作為程式碼重用的手段，當我們不能透過繼承來重用程式碼，我們就會開始思考該如何設計更有邏輯與組織的類別。當然，只要我們有需要，隨時都可以移除 final 關鍵字，但如果先把 final 類別作為預設值，我們就會考慮到要移除它的理由，final 就像是一個提醒功能，提醒我們深思熟慮繼承類別的必要性。

## 不同的類型擁有相同的行為

在進入主題之前，我們先來複習一下關於介面的定義方式：

interface InterfaceName {
  public function method( $parameter );
}

介面可以包含方法與常數，但卻不能有任何的屬性。所有在介面裡面定義的方法都必須是公開而且沒有實作細節，在 PHP 中，可以使用 extends 關鍵字來繼承另外一個介面：

interface ParentInterface {
  public function method( $parameter );
}

interface ChildInterface extends  ParentInterface {
  public function another\_method( $parameter );
}

與類別不同之處在於介面可以從其他多個介面進行繼承：

interface LoggerInterface extends WritableInterface, ReadableInterface {
  ...
}

當我們建立介面的時候，代表有使用這個介面的類別都必須要把該介面裡面的方法實作出來，舉例來說，當我們需要確保一個物件必須執行某些特定的方法時，我們就可以使用介面，介面是不同程式開發者之間的協議規範，就跟公司與公司之間的合約一樣，不履行就會發生問題。( 上法院或是程式爆炸 )

interface Flyable {
  public function fly( int $distance );
}

final class Bird implements Flyable {
  public function fly( int $distance ){
    // implementation
  }
}

final class Plane implements Flyable {
  public function fly( int $distance ){
    // implementation
  }
}

當類別決定實作某個介面時，這個類別就必須要有這個介面的公開方法，如同上面的範例一樣，鳥跟飛機都實作了 Flyable 這個介面，所以這兩個類別都必須要有 fly 這個公開方法，不然就會發生錯誤。要注意的是如果一個類別同時實作了兩個介面，那這兩個介面不可以有相同的方法名稱，不然會造成混淆。

而抽象類別也有提供介面，但關鍵的不同之處在於抽象類別直接提供了實作邏輯，以下使用抽象類別來改寫上面的例子：

abstract class FlyableEntity {
  protected $wings;
  abstract public function fly( int $distance );
}

當使用了抽象類別即代表我們設計了一個全新的資料類型，這個資料類型提供了一個抽象方法來作為介面，所以在子類別中可以進行介面的實作：

final class Bird extends FlyableEntity {
  public function fly( int $distance ){
    // child class implementation
  }
}

以真實世界的邏輯來看上面的例子可能有點奇怪，因為 FlyableEntity 不是一種本質化或是摘要化，這邊只是為了要示範抽象類別作為介面使用的範例。

如果使用抽象類別就可以達到介面的效果，那為何還需要宣告 Interface 呢？下面會繼續探討兩者之間的關係與差異。

## 介面與抽象類別

抽象類別是以一種新的資料類型呈現，而類別定義了物件的藍圖，我們知道如何透過類別來建立物件，就像我們知道如何使用字串或陣列一樣，當我們繼承一個類別時，就等於是創造了一個新的次要類型，我們不需要學新的用法就能使用它，因為只要能知道父類別怎麼用即可，下面以一個 Cahce 類別作為範例：

namespace Cache;

abstract class Cache {
  abstract public function get($key);
  abstract public function set($key,$value);
}

namespace Cache;
final class FIleSystemCache {}

namespace Cache;
final class RedisCache {}

namespace Cache;
final class NullCache {}

> 命名空間 namespace 的用途是避免相同的類別名稱而造成衝突，第一段程式中的 namespace Cache 宣告這個檔案的命名空間為 Cache，而下面三段的開頭都使用了 _namespace Cache_，即代表接在後面的類別都屬於 Cache 這個命名空間之中，因此類別 FileSystemCache、RedisCache、NullCache 這三個類別都擁有父類別 Cache 的 get 與 set 方法。
> 
> 更多命名空間介紹可以參考[這篇文章](https://medium.com/verybuy-dev/namespace命名空間介紹-e50a7fef029d)。

上面所有的類別都應該遵守它們的父類別 Cache 的行為，每一個子類別都與父類別相關聯，雖然它們可以用自己的方法來實作快取機制，但它們都用共同的公開介面：使用 get 來設定快取的值、使用 set 來儲存需要被快取的內容，這兩個方法都需要帶入 $key 來傳送給這兩個方法。

想像一下有個類別叫作 Product，我們需要把它加入快取之中，我們可以使用前綴 prodcut\_ 來組合它的 ID 來作為快取的 key：

$product = new Product();

//...

$cache->set( 'product\_'.$product->id, $product )

簡單明暸，但假設今天我們也想要把商品分類加進快取之中，我們就會比照辦理：

$cache->set( 'category\_'.$category->id, $category )

然後再次假設如果我們因為業務需求改變，因此決定要變更 Proudct 的快取 key，這件任務就會非常的阿雜，我們必須要翻遍所有程式碼然後搜尋取代 product\_.$product->id 這個地方，有很高的機率會漏掉它並且造成臭蟲，而且這樣的蟲非常不好抓。

如果我們讓類別 Cahce 不用設定 key、或是將其封裝於 Product 裡面呢？舉例來說，每一個需要被快取的 Product 或是 Category 類別都有一個方法叫做 getCacheKey，這方法會回傳要用在 Cache 裡面的 key，未來當需要修改 key 值時，只要變更 getCacheKey 這一個方法即可。

class Product {
  public function getCacheKey(): string {
    return 'product\_'.$this->id;
  }
}

class Category {
  public function getCacheKey(): string {
    return 'category\_'.$this->id;
  }
}

看起來比較好一些，但還有另外一個問題：我們必須確保每一個要使用快取類別的物件都有 getCacheKey 這個方法，回到 Cache 類別來加入一些判斷：

abstract class Cache {
  public function get( $key ) {
    $key = $this->resolveKey( $key );
    return $this->getFromCache( $key );
  }
  
  abstract protected function getFromCache( $key );
  
  protected function resolveKey( $key ) {
    if( is\_object( $key ) && method\_exists( $key, 'getCacheKey') ) {
      return $key->getCacheKey();
    }
    throw new CacheException( 'Object should implement method getCacheKey()' );
  }
  
  public funciton set( $key='', $entity ) {
    $key = $this->resolveKey( $key );
    return $this->storeInCache( $key );
  }
  
  abstract protected function storeInCache( $key, $data );
}

為了要檢查傳進去 Cache 的物件是否帶有 getCacheKey 方法，必須增加了一些條件來判斷，這讓程式變得又臭又長。根據 Cache 的業務邏輯，它必須要有 Key 才能作後續的動作，這時後類型提示就派上了用場，舉例來說，當我們要求參數必須是陣列的時候，就可以使用 array 來提示參數的資料類型。

在我們的範例中，我們擁有 Product 與 Category 這兩個類別，他們是兩種不同的資料類型，我們沒有辦法建立像是 Cache 這種抽象類別的階層關係，當然，雖然可以另外再新增一個抽象類別叫做 CachedEntity 並擁有 getCacheKey 方法，然後讓它的子類別實作 getCacheKey。

但這不是一個好主意，因為這些類別並不是真的彼此關聯，它可能看起來很彈性但這類的解決方法最後都會變成包山包海的超大物件，所以，讓我們來考慮使用介面：Interface。

介面不會產生新的資料類型，取而代之的是描述一個資料類型的單ㄧ面向，它只提供方法的骨架，這骨架裡面沒有任何的內容，我們可以建立一系列方法的骨架，最後在物件裡面來實作細節，根據上面的範例，我們定義一個介面叫做 Cacheable，然後讓 Product 與 Category 來實作 Cacheable 裡面的方法：

interface Cacheable {
  public function getCacheKey(): string;
}

class Category implements Cacheable {
  public function getCacheKey: string {
    return 'category\_'.$this->id;
  }
}

class Product implements Cachebable {
  public function getCachekey: string {
    retrun 'product\_'.$this->id;
  }
}

現在我們可以用類型提示來檢查要傳進去 Cache 的參數是否為 Cacheable：

abstract class Cache {
  abstract public funciton get(Cacheable $entity);
  abstract public funciton set(Cacheable $entity, $data);
}

另外 Cacheable 介面還可以定義另外一個方法來取得快取資料：getCacheData()，這個方法會回傳已快取資料的陣列。

interface Cacheable {
  public function getCacheKey(): string;
  public function getCacheData();
}

class Category implements Cacheable {
  public function getCacheKey: string {
    return 'category\_'.$this->id;
  }
  public function getCacheData(): array {
    return \[
      'name' => $this->name,
      'slug' => $this->slug,
      'productsNum' => $this->products->count()
    \];
  }
}

class Product implements Cachebable {
  public function getCachekey: string {
    retrun 'product\_'.$this->id;
  }
   public function getCacheData(): array {
    return \[
      'name' => $this->name,
      'slug' => $this->slug,
      'images' => $this->images,
      'productsNum' => $this->products->count()
    \];
  }
}

透過這樣的改寫來把所有的邏輯都封裝在 Cacheable 裡面，實作 Cacheable 這個介面的類別都擁有自己的 getCacheKey 與 getCacheData 的方法，抽象類別 Cache 完全不知道快取儲存與讀取的細節，只要用原本的 set 與 get 方法就能操作快取，而原本的 set 方法也不需要傳第二個 $data 參數了：

abstract class Cache {
  abstract public funciton get(Cacheable $entity);
  abstract public funciton set(Cacheable $entity);
}

Cache 類別完全信任 Cacheable 介面，因為實現 Cacheable 的類別一定會把介面定義的一堆方法完全實作出來，也因為這樣，Cache 類別可以完全信賴 Cacheable。

可以注意到介面的命名規則通常是用 able 或是 ing 的形容詞結尾，更進一步的說，它可以適用在所有不同的資料類型上，就像形容詞可以用在形容各種名詞，上面的範例我們有兩個類別叫做 Category 跟 Proudct，而我們用「可快取的 - Cacheable」 來形容它們，因此在實務上會慣用形容詞來幫介面命名。

當使用抽象類別的時候，我們的常數可能會有許多不同的值：

abstract class AbstractTable {
  const TABLE = null;
}

class UsersTable extends AbstractTable {
  const TABLE = 'users';
}

class OrdersTable extends AbstractTable {
  const TABLE = 'oreds';
}

我們可以在子類別裡面複寫或是重新宣告父類別中的常數，有時候很方便，就像上面的例子一樣，但這樣就不是常數了，假設我們真的需要一個不能被變動的常數，就可以把它定義在介面之中：

interface MathInterface {
  const PI = 3.14159;
}

class MathOperations implements MathInterface {
  const PI = 3.14 // Fatal Error!
}

在 PHP 中關於介面的常數還有另外一個特性：使用該介面的類別雖然無法重新宣告其常數，但如果又有一個子類別繼承了使用該介面的類別，那麼在這個子類別中又可以重新宣告這個常數：

interface MathInterface {
  const PI = 3.14159;
}

class MathOperations implements MathInterface {}

class MultiplicationOperations extends MathOperations {
  const PI = 3.14; // This works fine
}

總結：使用抽象類別就是我們要定義新的資料型態的時候，在繼承抽象類別的子類別中，我們可以複寫或是實作父類別中方法的細節，最重要的是我們要確保這些子類別與父類別是真的有血緣關係（同資料型態），而非為了要取得父類別的某些好處（重用程式碼）而去繼承它。

至於介面只提供公開的方法，使用介面可以確保實作該介面的類別具有介面指定的方法，而方法的實作細節都在類別裡面，因此可以根據業務邏輯作調整，同時又能達到資料型別提示的作用，省去寫一堆驗證物件的判斷。

## 複製與貼上程式碼

在 PHP 裡面沒有提供複數繼承的功能，一個子類別只能繼承一個父類別，有些人會覺得這樣不好，因為就不能同時繼承多個帶有實用的方法的類別，看到這邊我們已經理解為何透過繼承來達到程式碼重用的作法是最糟的，但假設我們在某些情境下真的需要重用之前寫過的程式碼，難道真的要複製貼上嗎？

舉例來說，我們有一個購物商城，它有商品型錄，型錄裡面有商品分類與商品品項，我們想要讓網址不要用 id 顯示，而是用人類看得懂的名稱，所以我們需要讓分類與品項都擁有自己的網址代稱：

class Product {
  private $name;
  
  public function slug(): string {
    $cleared = preg\_replace('/\[^A-Za-z0-9\]+/','-',$this->name);
    return strtolower($cleared);
  }
}

class Category {
  private $name;
  
  public function slug(): string {
    $cleared = preg\_replace('/\[^A-Za-z0-9\]+/','-',$this->name);
    return strtolower($cleared);
  }
}

很明顯的 slug 方法完全一模一樣，沒有人喜歡重複，因為很容易產生臭蟲，所以上面的寫法不推薦。如果另外寫一個類別叫做 BaseModelWithSlug 來處理的話，也不是一個好主意，因為這樣就只是為了功能而去做類別的繼承，還記得上面提到的基本觀念嗎？繼承一定要有血緣關係（同資料型態），BaseModelWithSlug 類別連資料型態都稱不上。

在這種情境下最適合的作法就是使用關鍵字 Trait，根據 php.net 的定義：Trait 是一套程式碼重用的技術，並減少單一繼承的限制，讓開發者可以自由地在不同的類別之中重複使用方法。

換句話說，Trait 可以讓我們用一個可重用的物件來整合相關的方法，然後這些重用物件會把裡面定義好的方法貼入到類別裡面。這跟繼承有什麼不同？差別在於 Trait 跟繼承一點關係都沒有，它比較像是 Mixin 混搭的概念。

Trait 也提供了很多方式來避免在繼承情況下所造成的衝突問題，如果一個方法同時被宣告在多個 Trait 裡面，那麼就會產生錯誤提示。只要記住使用 Trait 就是要取代複製貼上程式碼的行為，當一個類別使用 Trait 的時候，就好像是把 Trait 裡面的方法複製下來然後貼到類別裡面。

下面我們來建立一個產生網址代稱的 Trait：

trait HasSlug {
  public function slug(): string {
    $cleared = preg\_replace('/\[^A-Za-z0-9\]+/','-',$this->name);
    return strtolower($cleared);
  }
}

class Product {
  use HasSlug;
  private $name;
}

class Category {
  use HasSlug;
  private $name;
}

首先使用關鍵字 trait 宣告後，在類別裡面使用 use 來貼上 Trait 裡面的方法，這樣就能夠重複使用 slug 方法了，記得 use 要在類別的最前面，因為 PHP 需要找出類別的結構。但假設我們想要讓網址代稱是用類別的其他私有屬性呢？上面的範例是直接寫死 $this->name，如果要保持彈性可以新增一個方法來取得 name：

trait HasSlug {
  public function slug(): string {
    $string = $this->getStringForSlug();
    $cleared = preg\_replace('/\[^A-Za-z0-9\]+/','-', $string);
    return strtolower($cleared);
  }
  abstract protected function getStringForSlug(): string;
}

class Product {
  use HasSlug;
  private $name;
  protected function getStringForSlug(): string {
    return $this->name;
  }
}

class Category {
  use HasSlug;
  private $name;
  private $shortName;
  protected function getStringForSlug(): string {
    return $this->shortName;
  }
}

上面的範例中我們在 trait 裡面多加入了一個方法叫做 getStringForSlug()，它會返回字串，而真正產生網址代稱是透過這個方法，因此在不同的類別中，我們可以定義不同的網址代稱取得方式，這樣就能同時達成動態產生代稱且又不重複程式碼的目標了。

現在我們重新思考一下關於資料型別與介面的問題，我們把 Cateogry 變得更複雜一些：

class Category {
  use HadSlug;
  private $name;

  protected function getStringForSlug(): string {
    return $this->shortName;
  }
  
  public function parent(): self {
    // returns a parent category
  }
  
  public function products(): array {
    // returns products
  }
 
}
  

萬一使用 trait 的類別有自己的公開介面會發生什麼事？在使用 trait 前有兩個方法：parent() 與 products()，而透過 trait 新增加了 slug() 方法，而沒有人可以保證實作 Category 的類別有 slug 方法，所以就必須要去爬類別的原始碼，然後找到 trait 裡面帶有的方法。

因此我們需要建立一個新的介面來確保該類別是有帶有 slug 方法的：

interface Sluggable {
  public function slug(): string;
}

trait HasSlug {
  // ...
}

class Category Implements Sluggable {
  use HasSlug;
  
  // ...
}

class Product Implements Sluggable {
  use HasSlug;
  
  // ...
}

有了 Sluggable 介面，我們就可以利用它做資料類型提示，這樣就能確保有使用 HasSlug 的類別是擁有 slug 方法的，這樣的用法在大型且複雜的應用程式中是非常重要且不可或缺的。

關於 trait 還有一個可能發生的問題是如果有使用屬性並且預期它會出現在類別裡面的時候，以最上面第一個把代稱寫死的範例來說，trait 直接利用類別的 $name 來作為代稱的初始值，因此 trait 會認定有用它的類別一定會有 $name 這個屬性。

但假設在 Product 類別裡面要拿來做網址代稱的屬性名稱叫做 $titile，那麼這個 trait 就會出錯，要解決這個方法我們可以多加一個 property\_exists 判斷來檢查屬性是否存在：

trait HasSlug {
  public function slug(): string {
    if(!property\_exists($this,'name')){
      throw new Exception(
        'No property "name" to create slug'
      )
    }
    
    $cleared = preg\_replace('/\[^A-Za-z0-9\]+/','-',$this->name);
    return strtolower($cleared);
  }
}

最後要注意的是 trait 可以取得類別的私有屬性與方法，在某些情況下對於 trait 來說很方便，但如果是需要繼承就拿不到了：

trait Printable {
  function print(): void {
    echo $this->title;
  }
}

class Book {
  use Printable;
  
  private $title = "A very interesting book";
}

$book = new Book;
$book->print(); // A very interesting book

上面的範例不是好作法，因為私有屬性就是私有的，是不能讓類別以外的人所存取的，縱使是 trait 也是一樣，如果要這樣做最好有明確的理由，並且在使用時要特別小心。

Trait 是非常強大且彈性的工具，用來取代複數繼承的機制，它可以改善你的程式碼並且避免多餘的程式碼重複，但換個角度看，它也有可能變得越來越複雜，所以在使用上還是要盡量小心。

## 關於多型

在物件導向中，多型是最重要的概念，關於多型 Polymorphism 這個字在程式開發社群常被解釋為是一種單一介面到不同資料型態實體間轉換的規則，是指在程式執行階段，物件能夠依照不同的情況改變資料型態，多型有很多種，但有些因為 PHP 本身的限制而無法達成。

### 帶有特定目的的多型 Ad-hoc Polymorphism

這一類的多型常見的例子是宣告相同的方法名稱但卻帶有不同類型的參數，程式會自動判斷帶入參數的資料類型並決定要用哪一個方法：

class Calculator {
  public function sum(int x, int y){
    return x + y;
  }
  public function sum(float x, float y){
    return x+y;
  }
}

如果參數是整數，會用第一個方法，如果是浮點數，則用第二個方法，但可惜的是在 PHP 裡面不能這樣寫，會出現重複宣告的錯誤。

### 子類別的多型 Subtype Polymorphism

這一類多型是最常見的，它的意思是當我們在類別裡面有不同的方法但卻做著類似的事情的時候，我們應該把這些方法使用相同的名稱來命名。我們確信所有類別都有相同的介面、方法與使用相同的參數。

當我們的介面被實作的時候，我們不需要去關心這些類別是如何運作的，根據他們共有的介面，我們就可以知道它們的方法與行為，所以我們完全清楚如如何使用它們，下面範例先示範沒有使用多型的寫法，在之前的範例我們有一個基本的抽象類別叫做 Cache，以及一些衍伸該類別的類別，像是 MemcacheCache、FileCache 以及 RedisCache，接下來我們讓一個類別 Application 來使用 Cache，並且加入清除快取的邏輯在裡面：

final class Application {
  private $cache;
  
  public function setCache(Cache $cache):void {
    $this->cache = $cache;
  }
  
  public function flushCache(){
    switch(get\_class($this->cache)){
      case "MemcacheCache":
        $this->cache->clearMemcache();
        break;
      case "FileCache":
        $this->cache->unlineFile();
        break;
      case "RedisCache":
        $this->cache->flushRedis();
        break;
    }
  }
}

可以發現當不同類別有著類似卻沒有提供一個共用介面的時候，我們就必須要用這種醜醜的 switch 或是 if else 條件式來處理這些行為，如果日後多了一種快取機制又必須進到這個類別來新增一筆條件。

為了改善這個問題，我們需要一套可以共用的介面，我們可以把每個判斷條件拆解成獨立的類別，並且將實作細節隱藏在共用介面之中，看起來我們需要在 Cache 類別加入一個方法叫做 flush()，然後每個子類別來實作這個 flush() 方法：

abstract class Cache {
  abstract public function flush():void;
}

final class MemcacheCache extends Cache {
  public function flush():vod {
    $this->memcache->flush();
  }
}

final class FileCache extends Cache {
  public function flush():vod {
    unlink($this->file);
  }
}

final class RedisCache extends Cache {
  public function flush():vod {
    $this->redis->flush();
  }
}

然後我們可以修改原本在 Application 裡面的 flushCache 方法，並且移除掉所有的 switch 條件式判斷，因為我們不用再去處理不同快取間的清除方式，這些快取清除的定義都在子類別之中：

final class Application {
  private $cache;
  
  public function setCache(Cache $cache):void {
    $this->cache = $cache;
  }
  
  public function flushCache(){
    $this->cache->flush(); // 一行搞定
  }
}

修改之後 flushCache 變得簡單又清楚，如果要之後要新增一個 NullCache 就不會動到 Application 類別，只要新建一個 extends Cache 的類別即可。這就是所謂的多型，我們把不同的邏輯隱藏在一個方法後面 ( flushCache )，因此我們可以安全地新增實作細節，並且每一個類別都知道該如何使用它。

另一方面，如果各類別之間是沒有從屬關連的，我們也可以使用介面來實現上述的多型，舉例來說，我們有一個 Cache 類別使用 cacheable 物件來存放快取的內容，每一個使用 cacheable 介面的類別都必須實作 getCacheKey 的具體細節：

interface Cacheable {
  public function getCacheKey(): string;
}

final class Product implements Cacheable {
  public function getCacheKey(): string {
    return 'product\_'.$this->id;
  }
}

final class Category implements Cacheable {
  public function getCacheKey(): string {
    return 'category\_'.$this->id;
  }
}

Product 跟 Category 是兩個沒有從屬關係的類別，但因為他們使用了相同的介面，讓我們可以用相同的方法正確的使用它們。

final class Cache {
  public function store(Cacheable $cacheable): void {
    $this->store($cacheable->getCacheKey(), $cacheable);
  }
}

$cache = new Cache();
$product = new Product();
$category = new Category();

$cache->store($product);
$cache->store($category);

在這裡 Cache 類別完全不用擔心當參數傳入的物件是什麼，因為透過介面已約束傳入的類別必定會帶有 getCacheKey 方法，所以 Cache 可以無後顧之憂的使用它們。

不管是帶有從屬關係的 abstract 類別或是彼此沒有關聯而透過介面來把不同邏輯隱藏在同一個方法後面的過程，都是多型的展現。

### 參數式的多型 Parametric Polymorphism

不像上文第一個介紹的帶有特定目的的多型，參數式多型是 PHP 特有的，因為 PHP 的寬鬆資料類型而產生的，當沒有做類型提示時，我們可以傳入我們想要的函式或是方法：

function sum($a, $b){
  return $a + $b;
}

echo sum((int)2,(int)3); //5
echo sum((float)1.6,(float)2.12); // 3.72
echo sum('abc',4); //4

這一類的多型我們不在意傳入參數的資料類型，不管傳入什麼都能順利輸入結果，讓我們來重新看一次子類別多型的範例：

final class Cache {
  public function store(Cacheable $cacheable): void {
    $this->store($cacheable->getCacheKey(), $cacheable);
  }
}

這邊的 $this->store 方法就是使用參數式的多型，只要它接收到的參數是 Cacheable 的類型，它就能正常運作，就跟上面第一個範例中的 sum 函式一樣。

可以發現子類別多型與參數式多型彼此間關係密切，我們透過繼承或是介面建立了不同的子類別，然後根據它們的基礎類型來使用它們。

## 依賴注入 Dependency Injection

以物件導向開發的應用程式裡面，每個物件彼此都互相依賴。假設我們現在有一個專門在做資料庫 Query 的類別叫做 QueryBuilder，然後有天我們發現某筆資料的 Query 速度非常慢，我們想要增加 Log 紀錄來追查緩慢的原因，所以我們需要一個 logger 物件來做這件事情：

final class QueryBuilder {
  public function execute(string $sql, array $params): void {
    //...
    $logger = new Logger();
    $logger->info(
      'DB:'.$sql,';'.implode(',',$params);
    );
  }
}

我們使用 $logger 物件來紀錄每一次 Query 的 SQL 執行語句與參數，看起來好像這樣就能解決了，但假設你的同事某天修改了 Logger 類別的建構式，並且在建立物件時需要多帶一個參數叫做 $filename，那麼這支 QueryBuilder 就會爆炸噴錯，然後整個網站的資料庫 Query 也會跟著 GG。

當然，這樣的修改可以透過很多方式來避免，但只因為多了一個參數就能讓整個網站爆炸，這代表了程式背後的結構設計有很大的問題，同時顯現出整體架構的脆弱性。

### Hardcoded dependencies

每當我們在一個類別之中使用另外一個類別時，如果這個類別是抽象類別是 OK 的，但如果是實作一般類別就要避免，最危險之處在於一個物件在它不應該被實體化的地方被建立，就像上面範例中的 Logger 類別，那麼哪裡才是適合的地方呢？

讓我們先從反面的例子來找出錯誤的地方。我們已經看過上面的錯誤範例，如果 Logger 修改了建構式就要搜尋所有有用到 Logger 的類別來進修改，雖然現在的程式編輯器都有全站搜尋取代的功能，但 PHP 一門擁有動態類型的程式語言，下面這個範例會讓編輯器不容易找到：

function makeInstance($class){
  return new $class;
}

$className = 'Logger';
$logger = makeInstance($className);

類別名稱有可能使用變數來存放，然後透過函示來進行實體化，在這種情況下只有當程式出錯時才會發現原來這邊也有宣告 Logger。所以為了修正這個問題，我們必須把在類別中 new Logger() 移除，並使用參數的方式來傳入：

fincal class QueryBuilder {
  public function execute(string $sql, array $params, Logger $logger=null): void {
    //...
    if($logger){
      $logger->info(
        'DB:'.$sql.';'.implode(',',$params)
      );
    }
  }
}

我們可以讓 Logger 利用類型提示的方式以參數帶入，並透過判斷式來確保當有傳入 $logger 的時候才執行 Logger 類別的 info 方法，透過這樣的寫法改善了以下問題：

*   QueryBuilder 不用依賴 Logger 的建構式
*   QueryBuilder 不用依賴任何類別的實例化，Logger 能安全的被使用
*   可以設計一個假的 Logger 類別來做測試，當多人開發時如果 Logger 還沒被寫好，撰寫 QueryBuilder 類別的工程師就能先自己做一個暫時的 Logger 類別

當我們用參數的方式把依賴的類別傳入後，這樣的技術就叫做依賴注入，把需要依賴的類別寫死在需要它的類別之中，就會違反了單一職責原則，如果我們把依賴類別直接在類別中建立，這樣至少有兩個需要改善的理由。

我們來看看另外一個例子，類別 Delivery 負責傳送訂單到物流系統的 API，它有一組 HTTP 方法來負責 API 的請求：

final class Delivery {
  private $client;
  
  public function \_\_construct(){
    $this->client = new HttpClient();
  }
  
  public function send(Order $order): array {
    $response = %this->client->post('/orders/create',$order->toJson());
    return json\_decode($response,true);
  }
  
  public function getStatus(int $orderId): array{
    $response = $this->client->get('/orders/info',$orderId);
    return json\_decode($response,true);
  }
}

看到 new HttpClient() 了嗎？我們再次把依賴的類別寫死在裡面，現在我們已經知道正確的做法是用參數的方式傳入才對。但這個物件在每一個方法都有用到，如果每個方法都要多傳一個 new HttpClient 參數顯得有些累贅，在建構式宣告時就傳入是比較好的作法，根據經驗法則：如果一個類別沒有這個依賴物件就無法運作，那麼在宣告建構式時就應該把這個依賴物件用參數的方式傳入。

Delivery 類別修改如下：

final class Delivery {
  private $client;
  
  public function \_\_construct(HttpClient $client){ // 在這邊傳入
    $this->client = $client;
  }
  
  public function send(Order $order): array {
    $response = %this->client->post('/orders/create',$order->toJson());
    return json\_decode($response,true);
  }
  
  public function getStatus(int $orderId): array{
    $response = $this->client->get('/orders/info',$orderId);
    return json\_decode($response,true);
  }
}

### 在建構式或是新增方法傳入

在上面的範例中，我們在 Delivery 的建構式中來傳入 HttpClient 物件，但為何是用建構式來做依賴注入？為何不新增一個叫做 withClient() 的方法來管理 HttpClient 物件？這樣看起來比較彈性，同時也能保持建構式的簡潔，以及如果之後還需要對 HttpClient 物件做設定會來得更方便，但這會造成另一個嚴重的問題：

final class Delivery {
  
  private $client;
  
  public function withClient(HttpClient $client): void {
    $this->client = $client;
  }
  
  public function send(Order $order):array {
    $response = $this->client->post('/orders/create',$order->toJson());
    return json\_decode($response,true);
  }
  
  public function getStatus(int $orderId): array {
    $response = $this->client->post('/orders/info',$ordId);
    return json\_decode($response,true);
  }
}

當我們把依賴類別從建構式裡面移除並使用獨立的方法來傳入時，我們就成功建立了一個可以保持設定彈性的物件，但問題就會來了，參考下面的範例：

final class DeliveryController {
  private $delivery;
  public function \_\_constructor(Delivery $delivery){
    $this->delivery = $delivery;
  }
  public function sendOrder(int $orderId): void {
    $order = Order::find($orderId);
    $delivery->sendOrder($order);
  }
}

$delivery = new Delivery();
$controller = new DeliveryController($delivery);

$controller->sendOrder(111); // 會噴錯

雖然 DeliverController 類別已經傳入了 Delivery 類別的物件，但 DeliverController 類別無法得知 Delivery 物件是否已經準備妥當，因為它沒有辦法知道 withClient 方法是否已被呼叫，另外在這邊已經違反了封裝原則，DeliverController 不應該去關心所傳入的 Delivery 物件內部是如何運作的。

我們應該要避免寫出這一類不完整的類別，他們常造成臭蟲並且很難除錯與測試，因此這就是為什麼當一個類別如果不依賴另外一個類別就無法運作的話，這個依賴的類別必須要在建構式裡面就傳入的原因。

如果不是不依賴就無法運作的情況，我們就可以安心的使用方法來傳入依賴類別。讓我們來改寫上面 QueryBuilder 的範例，使用 setter 方法來傳入依賴類別：

fincal class QueryBuilder {
  private $logger;
  
  public function withLogger(Logger $logger):void {
    $this->logger = $logger;
  }
  
  public function execute(string $sql, array $params): void {
    //...
    if($this->logger){
      $this->logger->info(
        'DB:'.$sql.';'.implode(',',$params)
      );
    }
  }
}

在這範例中，依賴類別在 withLogger 這個 setter 方法中傳入，這樣就不會有預期外的副作用了，因為依賴的物件並沒有被封裝在 QueryBuilder 裡面，當我們提供一個 setter 方法給依賴類別，使用時就能透過該方法來置換依賴物件，但要小心如果修改了依賴類別的狀態，很容易就會破壞了封裝原則。

關於 Setter 注入的準則：對於一個物件來說，使用 Setter 方法來注入的依賴類別不是必須的，這些依賴不應該取代外部物件的功能，而應該是直接沿用。

### 依賴注入容器

現在你可能會有一個疑問：我們該在哪裡建立所有的依賴？很顯然我們應該透過參數的方式傳入，但在那之前我們需要先建立他們，如果只是像下面這樣宣告 Logger 是沒有太大改善的：

$queryBuilder->query( $sql, $params, new Logger() );

如果是在方法裡面來建立依賴，這樣又會遇到一樣的問題。我們需要一個地方來存所有的依賴。我們應該不用每次都還要重複宣告依賴類別，只要多一個 new 就會多一個潛在問題，因此這就是依賴注入容器登場的時候了。

我們知道一個類別不應該靠它自己來設定依賴，而是用注入的方式來處理，但如果這些依賴本身也有自己的依賴，我們應該透過建構式來傳入這些依賴，所以我們要在哪裡建立這些依賴？如果在每次需要他們時才建立一次感覺怪怪的，而且要管理這些依賴會變得很棘手。

在上述範例 Delivery 類別中，我們有一個在建構式宣告的依賴 HttpClient，HttpClient 本身帶有一個網址參數，必須在宣告實體時帶入，所以當每次我們需要 Delivery 物件的時候，都要寫成這樣：

$httpClient = new HttpClient( 'https://api-url.com' );
$delivery = new Delivery( $httpClient );

現在看起來已經有點複雜了，如果我們想把 HttpClient 的回應寫進 Log 裡面，會再需要一個 Logger 物件，而它也要用注入的方式處理，結果會變成這樣：

$logger = new Logger();
$httpClient = new HttpClient( 'https://api-url.com' );
$delivery = new Delivery( $httpClient, $logger$ );

程式變得越來越複雜而且醜醜的，依賴注入容器的核心概念是把建立依賴的邏輯隱藏起來，只要寫過一次之後就能直接使用，當我們需要依賴的時候，我們跟該容器做請求，剩下的細節就靠這個容器幫我們完成：

final class Container {
    public function makeDelivery(): Delivery {
        $logger = new Logger();
        $httpClient = new HttpClient( 'https://api-url.com' );
        return new Delivery( $httpClient, $logger$ )
    }
}

// 實作
$container = new Container();
$delivery = $container->makeDelivery();

上面的範例很簡單，主要的用意是隔離依賴類別實例化的邏輯，所以如果依賴有所變動的話，只要修改這個容器就好，而在實作時我們也不用擔心 Delivery 該如何設定、要引入哪些依賴，只要呼叫容器即可。

當然，除非是為了學習，你不用寫自己的依賴容器，有許多現成的套件可以使用：

*   [PHP-DI](https://php-di.org)
*   [Pimple](https://github.com/silexphp/Pimple)
*   [Symfony THe Dependency Injection Component](https://symfony.com/doc/current/components/dependency_injection.html)

挑你喜歡的即可，大部分這一類的工具都是使用 [PHP Reflection API](https://www.php.net/manual/en/class.reflection.php)，能大大的減少自己開始的時間並運用套件提供的功能更有效率的管理依賴。

### 依賴注入要注意的地方

透過上述的說明我們已經了解依賴注入的使用方法，現在該是來看看在什麼時機下不適合用它：

#### 太多依賴

當一個類別在建構式中有很多的依賴，這就是第一個警示告訴我們這個類別管太多了（違反單一職責原則）

final class OrderController {
    public function \_\_construct(
        OrderRepository $ordersRepo,
        PaymentGateway $payments,
        ShippingService $shipping,
        Logger $logger,
        Email $mailer
    ){
        $this->ordersRepo = $ordersRepo;
        $this->paymentGateways = $payments;
        $this->shipping = $shipping;
        $this->logger = $logger;
        $this->mail = $mail;
    }
}

首先，檢查建構式裡面的依賴項目，也許有些並非是必要的依賴，可以透過 Setter 方法來傳入。舉例來說，在處理 payment 的時候，正常情況下不會管到 shipping 的業務，另外一個做法是用一個 Container 傳到建構式裡面，然後再來處理這些依賴：

final class OrderController {
    public function \_\_construct( Container $container ){
        $this->ordersRepo = $container->get( 'ordersRepo');
        $this->paymentGateways = $container->get( 'paymentGateways');
        $this->shipping = $container->get( 'shipping');
        $this->logger = $container->get( 'logger');
        $this->mail = $container->get( 'mail');
    }
}

但這樣的作法違反了 Server Locator 設計模式，透過一個 Container 來隱藏所有依賴的資訊，如果想要測試這個類別，就必須要檢查所有的依賴，還需要給這些依賴提供測試資料，同時我們也無法確認這些依賴是否有在 Container 裡面被正確註冊。

在這樣的情況下，就失去了依賴注入的優點，物件本身不應該要去關心 Container 的實作細節。

**待續**