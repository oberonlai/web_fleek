---
title: 【專案】Flash Stories 的 Story
tags: []
id: '885'
categories:
  - - WordPress
  - - 商業模式
  - - 專案
date: 2020-03-19 20:14:48
etoc: true
---

[![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-16.23.52.jpg)](https://flashstories.co)

> 這篇文章要獻給台灣棒球幕後的重要推手、也是在國際賽中最關鍵的情蒐小組負責人、更是名球評、名教練 - 那就是～～～楊清瓏教練～～～的侄子 Gary Yang，他是我認識過幹話最多、髒話最多、最愛打電話給我、長得最像洪都拉斯、跟他吃飯從來不用付錢的朋友，也是我看過扛最多責任、最勇往直前、不知害怕為何物的一枚_猛男_！！！

時間回到半年前，上面開頭提到那位幹話最多的猛男每天都在跟我靠杯：「 [AMP Stories](https://amp.dev/about/stories/) 有多屌就有多屌」、「這一定是未來的趨勢」、「做 SEO 的都會搶這個」，腦波弱的我就這樣輕易被他說服，不知不覺下海跳進這個超級大坑，大概就跟馬里亞納海溝一樣深。
<!--more-->
![](https://oberonlai.blog/wp-content/uploads/2020/03/IMG_0735.jpg)

![](https://oberonlai.blog/wp-content/uploads/2020/03/IMG_0736.jpg)

對，跟後來長得完全不一樣XD

專案一開始我們自己用手繪的方式畫了一堆草稿，彼此在討論怎麼來呈現編輯 Amp Stories 的介面，大概有個雛形後，猛男的[設計師女友](https://knife.today) (幸福貌～) 以 Flash 般的速度就生出了第一版的操作介面，讓我一整個熱血噴發，大概花了幾天的時間就把 Layout 用程式把它寫出來，這讓猛男&女友也噴發了 x 2，殊不知背後的險途還在等著我們...

![](https://i0.wp.com/oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-16.26.49.jpg?fit=1024%2C284&ssl=1)

![](https://i0.wp.com/oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-16.27.07.jpg?fit=1024%2C283&ssl=1)

![](https://i0.wp.com/oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-16.27.25.jpg?fit=1024%2C283&ssl=1)

## 404 小時的汗與淚

淚是螢幕看太多揉眼睛跟沙子跑進去的...

這 404 個小時的時間，我們從商業模式、定價策略、介面設計到程式開發都一直不停地在討論，意見不合時我習慣寫萬言書來靠杯，字多到連 Notion 的免費方案都用完了

而猛男則是會直接打電話來，每次當他在電話說服我之後，我又會開啟新的萬言書來重新討論在電話中的話題，沒辦法，猛男口才太好，只要一聽到他的聲音就會被他帶有磁性的聲音給收服了。

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-12.50.38-1024x528.jpg)

404 小時的心血

這是我有紀錄時間以來，執行最多個小時的專案了，這段期間有苦、有酸、有甜、有屎(眼屎)，最要感謝的當然還是猛男&猛男女友，然後最最最重要的老婆支持，其他要感謝的感謝不完，但期待本篇是言情小說的朋友可以轉台了，因為接下來是技術文時間XD

* * *

做這個編輯器讓我的前端技術突飛猛進，尤其是 ES6，第一次可以不用靠 jQuery 來做網站，走過一遍原生 JS 的寫法、再搭配 404 個小時的服用就差不多可以熟悉了。

先介紹 Flash Stores 這個產品是在幹嘛的：它是一套線上的排版工具，可以自行插入文字、圖片、影片，新增封面、內頁與頁尾，最後呈現的結果會直接變成 Google AMP Stores 的瀏覽方式，AMP Stories 把它想成是 Instagram 的限時動態，只是它不用開啟 IG App 才能製作。

它還可以 Embed 在網頁之中，參考下方範例：

   

Google 設計了一堆網頁元件，用這些元件來製作網頁，就會變成 AMP Stories，只是要使用這些元件必須懂得網頁語法，所以我們開發了 Flash Stories 讓不會寫程式的朋友也可以設計自己的 AMP Stories。

技術部分前端使用 ES6，開發整合環境為 Gulp、HTML 語法採用 Pug、CSS 則使用 SCSS 撰寫、CSS 框架使用 UIKit ( I hate Bootstrap )、使用 LocalStorage 儲存使用者編輯的資料、組織檔案用 Module、每個元件用 Class 拆解、用 Fetch API 來跑 Ajax、用 Promise 來計算事件處理順序，

為了做這站買了三本 JS 書：[新一代 JavaScript 程式設計精解](https://www.books.com.tw/products/0010799123)、[深入學習 JavaScript](https://www.books.com.tw/products/0010811645) [模組化](https://www.books.com.tw/products/0010811645)[設計](https://www.books.com.tw/products/0010811645)、[現代 JavaScript 實務應用](https://www.books.com.tw/products/0010786463)，這三本都是超級大推的 JS 好書！

而第三方套件使用 Moveables 來做 Drag and Drop、串接 Unsplash API、圖床使用線上服務 Uploadcare、另外還有編輯器一定會用到的 Font Picker、Color Picker、Gradient Color Picker 等一堆有的沒的 Picker...

後端當然還是用我最愛的 WordPress，手刻會員登入註冊、新增編輯刪除文章、撈首頁文章、分類、第三方套件使用 ACF Pro 紀錄所有 AMP Stories 的欄位、用 WooCommerce 做會員頁，未來可以接入金流做訂閱服務。

## 前端技術介紹

### 一、**UIKit**

[https://getuikit.com](https://getuikit.com)

[![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-12.59.32.jpg)](https://getuikit.com)

凡遇到朋友有在寫程式的朋友我必推這套，特別是是有在用 Bootstrap 的，UIKit 是 WordPress Theme 開發商 YooTheme 所開發的框架，裡面整合了超級多爆炸實用的套件，

基本盤的 Slidershow、Slider 不用說，還有 Modal、Off-canvas、Sticky 更是每個專案都會用到的 UI 元件，Flash Stories 的編輯畫面就是用 UIKit 所兜出來的，所以可以很快速的來搭建整個介面的原型，而且互動功能就跟最後的完成品一模一樣，Code 也不用重寫。

UIKit 另一個我非常喜歡的地方就是它的組織方式，每個元件的分類非常清楚，而且 API 文件的說明與範例也十分完整，我已經用它用了兩年多了，還沒有碰過它無法處理的介面，唯一遇到的狀況就是要修改現有元件的狀況，覆寫 Style 跟利用 UIKit API 處理也很方便。

它還有一支 API 沒有出現在文件內但卻非常實用的就是 getComponent()，它會回傳這個元件當下所有的屬性，我就是用這個屬性來判斷 Slider 目前停在第幾則，當初卡了好一陣子才從原始碼查到這一隻 function。

// 第一個參數丟要取得資訊的元件DOM，第二個丟元件名稱
UIkit.getComponent(document.querySelector('\[uk-slider\]'), 'slider'))

### **二、Pug / Sass**

[https://pugjs.org/api/getting-started.html](https://pugjs.org/api/getting-started.html)

[https://sass-lang.com](https://sass-lang.com)

現在叫我寫 HTML 跟 CSS 會要我的命，因為有了 Pug 跟 Sass 可以簡化許多寫法，像是 Pug 讓我寫 HTML 的時候不會再忘記結尾符號、當 Div 太長時要修改縮排會錯亂、不用再打 class=xxx 或是 id=xxx，只要 div.class#id 就好，它還可以寫變數、迴圈、判斷式，可以讓我寫 HTML 像在寫 PHP 一樣可以用很多流程控制來輸出內容。

Pug 最強大的地方我覺得在於可以吃 JSON 格式，先把頁面的文字內容、圖片路徑、連結位址都寫在 JSON 裡面，然後直接用 Pug 的迴圈去輸出，就像 PHP 的 foreach 一樣，就不用重複的內容一直複製貼上，這樣做法的好處是在前端時就有 MVC 的邏輯在裡面，JSON 負責 Model，Pug 負責 V 跟 C。

下面是使用範例：先把資料的部分用 JSON 來定義，再透過 Pug 的 each 來呼叫

{
  "section\_cover":\[
    {
      "title": "未命名的故事",
      "desc": "故事描述",
      "src": "assets/img/image.jpg",
      "author\_src": "assets/img/avatar.jpg",
      "author": "作者名稱",
    }
  \],
}

div
  each i in section\_cover
    h1=i.title
    h2=i.desc
    img(src=i.src)
    img(src=i.author\_src)
    p=i.author

Sass 的部分我是習慣用 Scss，UIKit 的原始檔有提供 Scss 版本，所以 Import 要用的元件就好，因此覆寫的方法我也是比照 UIKit 的分類方式，這樣要改特定元件就非常好找。

![](https://i0.wp.com/oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-13.13.30.jpg?fit=1024%2C768&ssl=1)

Sass 資料夾結構

至於 Scss 的好處是可以巢狀結構、變數、Mixin、Function 跟一堆進階用法，這部分我預計會再開另外一篇講 Starter Theme 的文章來仔細說明。

### **三、Gulp**

[https://gulpjs.com](https://gulpjs.com)

![](https://i1.wp.com/oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-16.30.10.jpg?fit=1024%2C726&ssl=1)

Gulp 是一套任務自動化管理工具，它利用 Node.js 來處理 Pug 轉 HTML、Sass 轉 CSS、以及檔案壓縮跟其他一堆爆炸多的功能，它的設定檔清楚易懂、結構分明，現在雖然有很多更新更強大的套件，但我還是依舊選擇它作為前端開發的必備工具。

而這個專案是我第一次使用 JavaScript 的模組功能，雖然新的瀏覽器有支援 Module，但為了相容尚未支援的瀏覽器，我使用了 Browserify Gulp 版套件來處理 Module 的打包。

gulp.task('pug', function () {
  return gulp.src('./src/\*\*/\*.pug')
    .pipe(data(function (file) {
      return require(paths.data + 'data.json');
    }))
    .pipe(pug({
      pretty: true
    }))
    .pipe(remember('pug'))
    .on('error', function (err) {
      process.stderr.write(err.message + '\\n');
      this.emit('end');
    })
    .pipe(gulp.dest(paths.public));
});

gulp.task('sass', function () {
  return gulp.src(paths.sass + '\*.scss')
    .pipe(sass({
      includePaths: \[paths.sass\],
      outputStyle: 'compressed'
    }))
    .on('error', sass.logError)
    .pipe(prefix(\['last 15 versions', '> 1%', 'ie 8', 'ie 7'\], {
      cascade: true
    }))
  
    .pipe(gulp.dest(paths.css))
    .pipe(browserSync.reload({
      stream: true
    }));
});

gulp.task('js', function () {
  return browserify('src/js/script.js')
    .transform(babelify.configure({
      presets: \["@babel/preset-env"\]
    }))
    .bundle()
    .pipe(source('script.js'))
    .pipe(buffer())
    .pipe(streamify(uglify()))
    .on('error', function (err) { console.log(err) })
    .pipe(gulp.dest(paths.js))
    .pipe(browserSync.reload({
      stream: true
    }));
});

上面是我 Gulp 處理關於 Pug、SCSS、 JS 的內容，雖然 Gulp 已經出到 4 了，但因為環境設定實在很麻煩，還能用我就懶得更新了。

有一個新的打包工具我關注一陣子了，名字叫做 [Parcel](https://parceljs.org)，它的編譯速度比 Webpack、Broswerify、Gulp 都還要快上個好幾倍，而且號稱是免設定，什麼 code 都不用寫直接開箱即用，它會根據你寫的語言自動安裝相對應的套件，十分方便。

之前一些自己在玩的小專案我就有用它，真的是飛快，而且不用搞一堆設定檔，但唯一最大的壞處是它的 dist 資料夾無法進行分類，永遠只有一層，所以不管是圖片、CSS、JS 全部都會擠在一堆，實在不 OK。

目前 Parcel 已經出到 2 了，但又有一陣子沒有 release，當初開發者說會把 dist 分資料夾功能寫進去，但目前看起來不知道要等到何年何月了...

> 不能分 dist 的問題解決了，有強者大大寫了第三方套件 [parcel-plugin-custom-dist-structure](https://github.com/VladimirMikulic/parcel-plugin-custom-dist-structure/blob/master/README.md)，可以完美的照檔案類型來區分資料夾，設定方式也超簡單，最感動的是開發者超級熱心，回報 issue 都秒改好，而且還會提供 Branch 先讓我測，我現在已經把 Parcel 應用在我的正式專案中了，編譯速度一整個大心！！！

### **四、Modules**

不管是 Pug、Sass 或是其他任何一種的程式語言，基本上都會有 import 的功能，但在 JavaScript 則是到了 2015 年才有比較方便的 API 可以使用引入功能，使用 Modules 的好處是可以讓每一隻 .js 檔案只專注在特定的功能，才會比一隻超肥的大檔案的來得容易管理。

有了 Modules 就可以引入很多別人已經寫好的小功能，或是把自己常常用到的 Function 模組化，不管是之後要用到還是未來的其他專案，都可以再加以重複利用。

![](https://i2.wp.com/oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-16.39.44.jpg?fit=1024%2C651&ssl=1)

我把 JS 分成三大類：component、helper、lib 與 partial。component 是編輯器左邊的各種小工具，helper 放一些會重複使用的類別，lib 是第三方套件，而 partial 則是 Layout 相關的 JS。

所以假設今天我要修改文字小工具的功能，我就進去 component 找到裡面的 Text.js 修改即可，使用 Module 可以很方便的進行管理。

### **五、Class**

JS 是一門原型 ( Prototype-based ) 導向的程式語言，與其他物件導向的程式語言相比有本質上的不同，原型導向相對彈性、對於宣告資料型別沒有硬性規定，在 ES6 之前的宣告類別的方法就跟宣告函式一樣，所以為了讓廣大的物件導向使用者可以比較好手上 JS 的 OOP，新設計了 Class 這樣的語法糖。

JS Class 用法跟 PHP 的很像，都有 constructor、destructor、屬性、方法、私有成員、靜態方法這些寫法，也可以繼承以及複寫父類別，而用 JS Class 寫出來的東西，只要去看 Gulp 編譯出來的結果就知道，它還是一堆 Function 的組成，只是用 Class 語法來寫清楚多了，在記憶度上面也比傳統的寫法好記得多。

我設計了一個父類別叫做 Component.js，它掌管了所有編輯器元件的共同功能，像是新增、刪除元件、移動、旋轉、放大縮小，然後再根據不同的元件來建立子類別，而這子類別都繼承父類別的所有屬性與方法。

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-16.59.06-1024x464.jpg)

像是 Text.js 就是繼承了 Component.js，constructor() 裡面的 super() 則是繼承父類別的 constructor()，這個函式可以讓子類別直接存取到父類別建構式裡面的屬性與方法。

類別還可以應用在一些實用的功能，像是我為了要能把 RGB 轉換成 16 進位的色碼，寫了一隻轉換程式：

export default class {
  static transfer(rgb) {
    rgb = rgb.match(
      /^rgba?\[\\s+\]?\\(\[\\s+\]?(\\d+)\[\\s+\]?,\[\\s+\]?(\\d+)\[\\s+\]?,\[\\s+\]?(\\d+)\[\\s+\]?/i
    );
    return rgb && rgb.length === 4
      ? "#" +
          ("0" + parseInt(rgb\[1\], 10).toString(16)).slice(-2) +
          ("0" + parseInt(rgb\[2\], 10).toString(16)).slice(-2) +
          ("0" + parseInt(rgb\[3\], 10).toString(16)).slice(-2)
      : "";
  }
}

export default class 是當整個類別只有一個方法的時候，可以直接匯出使用，而 static 關鍵字則是宣告靜態方法，也就是不用 new 出一個實例就可以直接使用，應用如下：

import rgb2hex from "../helper/Rgb2hex"; // 引入色碼轉換程式
someInput.value = rgb2hex(255,255,255) // 值為 #ffffff

其他 Class 的應用還有我把所有的變數也拉出一個類別，方便管理

/\*\*
 \* 定義所有的變數
 \*/

export class Variable {
  constructor(){
    /\*\*
     \* 全站相關變數
     \*/
    this.btnSave = document.getElementById('btnSave')
    this.btnShare = document.getElementById('btnShare')
    this.btnEmbed = document.getElementById('btnEmbed')
    this.shareUrl = document.getElementById('shareUrl')
    this.embedCode = document.getElementById('embedCode')
    this.btnCopyShareUrl = document.getElementById('btnCopyShareUrl')
    this.btnCopyEmbedCode = document.getElementById('btnCopyEmbedCode')
    this.postStatus = document.getElementById('postStatus')
    this.templateUrl = insert\_params.templateurl;
  }
}

// 匯入
import { Variable } from "./lib/Variable";
const vars = new Variable()
vars.btnSave.addEventListener('click',()=>{...}) // 使用 vars.變數名稱即可使用

### **六、Storage**

ES6 提供了豐富的本地儲存 API，也就是把資料暫時儲存在使用者的瀏覽器內，這樣的本地儲存技術又分為 LocalStorage、SessionStorage、IndexDB，這個專案我使用的 LocalStorage，原因是因為我需要可以長久保存使用者編輯的內容，又可以直接將 JSON 序列化傳到 WordPress 的 ACF 欄位來進行儲存。

我另外寫了一隻 Class 來設計 Storage 的新增與取得，當使用者進入之前編輯過的文章時，會先從伺服器讀取之前上傳的 JSON，讀取到後寫入 Storage，再從 Storage 去拆解出來在每個頁面上所有元件的位置、尺寸、內容。

如果伺服器上沒有資料也沒關係，因為存檔都會存在本機端，即使關掉瀏覽器再重新打開，都還是可以看到前次的編輯紀錄。

這對我來說真的是一個超級大工程，如果沒有用 OOP 的方式把每個元件拆開，要一次處理所有元件的資料，真的是頭會爆炸，更不用說這專案還是只有我一人開發，如果還有其他工程師情況一定更複雜。

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-17.12.47-1024x459.jpg)

有興趣的朋友可以打開開發者工具的 Storage 找到 Local Storage 頁籤看看我把什麼東西塞進去XD

export class Storage {
  constructor(app,load){
    this.app = app;
    this.load = load;
    this.storage = localStorage; // optional sessionStorage
    this.data = JSON.parse(this.storage\[this.app\]  '{}');
  }
  // Get all data form storage
  getItem(){
    return this.data;
  }
  clearItem(){
    this.storage.clear();
  }
  // Add new data
  setItem(key, value){
    this.data\[key\] = value;
  }
  // Save new data to Storage
  save(){
    this.storage\[this.app\] = JSON.stringify(this.data);
  }
  loading() {
    this.storage\[this.app\] = this.load;
  }
}

這隻是我專門處理 Storage 的類別，當宣告 Storage 物件時，第一個參數拿來當 Local Storage 的主 key，當使用 setItem() 把資料存進 value 後，執行 save() 才會寫進 Local Storage，這樣做的好處是可以避免我的 key 跟其他網站的 key 打架而造成資料被覆寫。

實際運作方法如下：

import { Storage } from "./lib/Storage";  // 匯入 Storage 類別

// insert\_params.storage 是 wp-admin ajax 所定義的參數，它會讀取已經存在 DB 的資料
var fs\_storage = new Storage("flashstories", insert\_params.storage);

// 當 DB 有資料的話先清空 local storage 再重新寫入
if (insert\_params.storage) { fs\_storage.clearItem(); fs\_storage.loading() }

// 寫入各單元的設定內容，第二個參數的 getData() 定義在相對應的類別之中
fs\_storage.setItem('section\_cover', cover.getData());
fs\_storage.setItem('section\_page', Pages.getData());
fs\_storage.setItem('section\_page\_transition', Pages.getDataTransition());
fs\_storage.setItem('section\_bookend\_share', bookend.getShareData());
fs\_storage.setItem('section\_bookend\_desc', bookend.getBrandDesc());
fs\_storage.setItem('section\_bookend\_link', bookend.getLinkData());

// 按下儲存後把 value 序列化存入 storage
fs\_storage.save();

### **七、Promise**

JS 的 Promise 很好的解決了先跑 A、A 完成後再跑 B 的非同步行為 ( Asynchronous )，正常情況下程式就是一行一行跑，不會管你第一行要做的事情有沒有完成，都是同步的 ( Synchronou )，有了 Promise 可以讓我確保 A 任務真的有確實執行完成後才做其他事。

這專案我有用 Promise 的部分還不少，大部分都是為了介面操作上的體驗，像是確保新增頁面完成後才加入相對應的內容，不然頁面的 HTML 還沒跑完就插入文字進去就會噴錯，或是確定檔案上傳完成後才出現設定項目，其他的應用還有在刪除頁面時回傳目前所在頁面 Index，有了這 Index 才可以讓 Slider 停留在正確的頁面上。

以下範例為當頁面新增完成之後，讓 addNewPage method 回傳正確的總頁數，再交給其他程式去做相對應的計算與判斷。

let addNewPage = ()=>{
  this.num++;
  return new Promise((resolve,reject)=>{
    // 頁面本體
    var li = document.createElement('li');
    var temp = \`略\`;
    li.innerHTML = temp;
    if(load){
      li.setAttribute('data-page', \`page${id}\`)
    } else {
      li.setAttribute('data-page', \`page${this.pageId}\`)
    }
    this.vars.slideshowPage.querySelector('.uk-slider-items').appendChild(li);
    var index = this.vars.slideshowPage.querySelectorAll('.uk-slider-items li').length;
    resolve(index);   // 在 Promise 成功時把 slide item 的個數丟出來
  })
}

### **八、Fetch API**

如果不靠 jQuery 要寫 Ajax 很麻煩，因為我很討厭用 XMLHttpRequest，自從有了 Fetch API 後人生就變彩色的了～不得不說這一支的 API 介面設計得真的很好，非常易用。

按下右上角儲存按鈕後，會先把資料存到 Storage，然後把整堆序列化後丟到 WordPress 再去 decode，因為 Fetch API 是用 Promise，所以可以很好掌握當資料完成時要做哪些事，透過 .then 的形式很直覺的就知道接下來要做什麼，有了這一支是讓我可以捨棄 jQuery 的最大原因～

以下是儲存按鈕的簡化版範例：

vars.btnSave.addEventListener('click',e=>{

  let params = new URLSearchParams();
  params.append("action", "insert");
  params.append("nonce",insert\_params.nonce)
  params.append("is\_new\_post", getIsNewPost());
  params.append("storage", JSON.stringify(fs\_storage.getItem('flashstories')));
  params.append("post\_title", vars.inputStoryTitle1.value);
  params.append("post\_status", postStatus.querySelector('button\[data-option\]').dataset.option);
  params.append("post\_category", document.querySelector('input\[name="postCategory"\]:checked').value );
  
  fetch(insert\_params.ajaxurl, {
    method: 'POST',
    credentials: "same-origin",
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded; charset=utf-8'
    },
    body: params
  }).then((response) => {
    return response.json();
  }).then((jsonData) => {
    that.classList.remove('loading');
    that.classList.add('success');
    that.querySelector('.text').textContent = "儲存成功！"
    console.log(jsonData.output);
  }).catch((err) => {
    alert("因網路問題儲存失敗，請稍候再試！")
    // console.log(err)
    that.classList.remove('loading');
    that.querySelector('.text').textContent = "儲存"
  })
})

需要注意的地方是要傳給後端的 params 變數，之前寫的時候一直傳不過去，後來爬到了一位日本工程師的文章，才曉得原來要用 URLSearchParams 類別來處理，宣告物件後，再使用 append 的方式來塞入要傳的參數名與值，再把 params 指定給 body。

第一個 then 代表成功回傳，第二個 then 則是確認接收到回傳的 JSON 資料後來做一些處理，catch 則是發生錯誤時的處理，Fetch API 簡單清爽一目瞭然！

### **九、Uploadcare**

[https://uploadcare.com](https://uploadcare.com)

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-17.45.06-1024x634.jpg)

這是一間專門做圖床主機的服務，但它比一般圖床主機做得更多，差別在於它提供了 JavaScript API 來設計檔案上傳介面，還整合了其他第三方儲存空間，讓使用者不只可以透過本機上傳圖片，還可以從 Facebook、Dropbox、Google Drive 等一堆第三方服務來上傳照片。

此外它還整合了 CDN、圖片裁切的功能，只要網址的參數修改一下，就可以取得不同尺寸、甚至是帶有濾鏡效果的圖片，非常的強大，每個月的月費也還算在可接受範圍。

當初在評估圖床的時候也找到很多類似 Uploadcare 的服務，其中更找到有 Open Source 的上傳介面也是可以整合第三方服務的，但考量到自己去扛圖床機的成本，算一算初期使用 Uploadcare 還是比較划算的。

<!-- 在 head 加入這些 -->
<script>
  UPLOADCARE\_PUBLIC\_KEY = 'Your Key';
  UPLOADCARE\_LOCALE = 'zhTW';
  UPLOADCARE\_EFFECTS = 'crop,rotate,mirror,flip,enhance,sharp,blur,grayscale,invert';
  UPLOADCARE\_PREVIEW\_STEP = true;
  UPLOADCARE\_CLEARABLE = true;
</script>

<!-- 上傳按鈕 -->
<input type="hidden" role="uploader" data-maxsize="4194304">

<!-- 套用 uploadcare -->
<script>
uploadcare.Widget('\[role=uploader\]')
</script>

點擊上傳按鈕就會出現上傳視窗：

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-17.49.57-1024x670.jpg)

### **十、Unsplash API**

[https://unsplash.com/developers](https://unsplash.com/developers)

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-17.55.42-1024x509.jpg)

全世界最大的免費圖庫，為了要讓第一次使用 Flash Stories 的朋友能更方便上手，我們就決定要整合免費圖庫來給大家用，對於只是想用文字創作的朋友來說，就不用再費心自己去準備圖片，直接使用圖庫即可。

Unsplash API 還滿好接的，比較龜毛的地方是如果沒有通過線上申請，Demo 版只能每小時請求 50 個 Request，但現在還沒到這個量，就超過再說了。

// 先建立 Unsplash 物件
const unsplash = new Unsplash({
  applicationId: "Your APP ID",
  secret: "Your APP secret"
});

// 使用關鍵字搜尋圖片
vars.inputUnsplashSearch.addEventListener('keyup', e => {   
  setTimeout(() => {
    unsplash.search.photos(keyword, page)
      .then(toJson)
      .then(json => {   // 把搜尋到的圖片加入到清單中
        vars.loadingUnsplash.classList.remove("active");
        vars.wrapUnsplashList.innerHTML = "";
        var result = json\['results'\];
        loadUnsplashImage(result);
      });
  }, 2000);  // 暫停輸入兩秒後才會開始搜尋
})

### **十一、Moveables**

[https://daybrush.com/moveable/](https://daybrush.com/moveable/)

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-18.01.17.jpg)

沒有這個套件憑我一人之力是沒有辦法開發出 Drag and Drop 系統的，這套超級強大，支援超多功能，API 超豐富，而且更新更得很勤，頁面上新增的小工具我都是用它來進行控制。

每個動作都會產生相對應的 css style，所以我就把這些 css 寫入到 Storage 中，但雖然有了這工具還是卡關卡超久，主要在於當新增多個頁面、小工具的情況下，沒辦法把 Moveable 套用到新增的元件上。

這一關碰壁碰了超級無敵久，因為有新增、刪除等這些變數在，每當頁面或是元件數量有變動時，之前用累加的方式來判斷就會 GG，最後換條路走，不要用元件現有的 id 名稱來累加，而是先去判斷頁面上有多少元件，再以元件的總數來回指定元件 ID。

### **十二、Pickers**

[https://font-picker.samuelmeuli.com](https://font-picker.samuelmeuli.com)

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-18.11.10.jpg)

文字小工具的字型選擇我用 Font-picker，它主要是撈 Google Font 的字型，但不知道為何，照它的說明加入 Noto Serif TC 都失敗，所以只能用一些 Hack 的方式去把它加入。

// 取得選中的字型
Array.from(widgetText.widgetTextFamily.querySelectorAll('.font-list-item button')).forEach(element => {
  element.addEventListener('mouseover', e => {
    let family = window.getComputedStyle(e.currentTarget, null).getPropertyValue('font-family');
    if (family === '\\"Noto Serif\\"') {    // 手動塞中文 serif 字型
      widgetText.syncFamily(config, 'Noto Serif TC')
      document.querySelector('.dropdown-font-family').style.fontFamily = 'Noto Serif TC';
    } else {
      widgetText.syncFamily(config, family)
      document.querySelector('.dropdown-font-family').style.fontFamily = family;
    }
  })
});

[https://simonwep.github.io/pickr/](https://simonwep.github.io/pickr/)

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-18.13.49.jpg)

顏色選擇器我用了 Pickr，這是一套介面非常漂亮又實用的顏色選擇套件，API 也很豐富，但可惜沒有包含漸層顏色的設定，但同作者改寫了一套叫做 GPickr，完美搞定漸層顏色選擇。

[https://simonwep.github.io/gpickr/](https://simonwep.github.io/gpickr/)

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-18.17.21.jpg)

這專案前端的部分花了我大半的時間，好不容易搞定後以為後端會容易得多，想不到還是老話一句：細節藏在魔鬼裡...

## 後端 WordPress 技術

### **一、會員註冊登入**

WordPress 本身的會員系統就超強大，角色權限內建的就超夠用，API 拉一拉很容易就可以完成會員登入系統。會員註冊登入在以前的很多專案都有用過，這次為了讓使用者體驗好一些，就還是找了範例改寫了 Ajax 的版本，只是還是繼續用 jQuery 就是了XD

註冊登入 JS 部分

jQuery(document).ready(function ($) {

  var redirectURL = window.location.href;
  
  $(".phone-format").keypress(function (e) {
    if (e.which != 8 && e.which != 0 && (e.which < 48  e.which > 57)) {
      return false;
    }
    var curchr = this.value.length;
    var curval = $(this).val();
    if (curchr == 4) {
      $(this).val(curval + "-");
    } else if (curchr == 8) {
      $(this).val(curval + "-");
    } else if (curchr == 12) {
      $(this).attr('maxlength', '12');
    }
  });

$('form#login, form#register').on('submit', function (e) {
    e.preventDefault();
ctrl = $(this);
$('.uk-alert').fadeOut();
$('#btnLogin').text('登入中...');
$('#btnRegister').text('註冊中...');

    if ($(this).attr('id') == 'register') {
      document.getElementById('messageSignupError').style.display = "none";
      action = 'ajaxregister';
      username = $('#signonname').val();
      password = $('#signonpassword').val();
      phone = $('#phone').val();
      email = $('#email').val();
      security = $('#signonsecurity').val();
      $.ajax({
        type: 'POST',
        dataType: 'json',
        url: ajax\_auth\_object.ajaxurl,
        data: {
          'action': action,
          'username': username,
          'password': password,
          'email': email,
          'phone': phone,
          'security': security
        },
        success: function (data) {
          if (data.loggedin == true) {
            document.location.href = redirectURL;
            $("#btnLogin").text("轉頁中...");
          } else {
            ctrl.prev().fadeIn("fast");
            document.getElementById('messageSignupError').textContent = data.message;
            document.getElementById('messageSignupError').style.display = "block";
            $("#btnRegister").text("立即註冊");
          }
        }
      });
    } else {
      action = 'ajaxlogin';
      username = $('form#login #username').val();
      password = $('form#login #password').val();
      security = $('form#login #security').val();
      $.ajax({
        type: 'POST',
        dataType: 'json',
        url: ajax\_auth\_object.ajaxurl,
        data: {
          'action': action,
          'username': username,
          'password': password,
          'security': security
        },
        success: function (data) {
          if (data.loggedin == true) {
            document.location.href = redirectURL;
            $("#btnLogin").text("轉頁中...");
          } else {
            ctrl.prev().fadeIn("fast");
            document.getElementById('messageLoginError').textContent = data.message;
            document.getElementById('messageLoginError').style.display = "block";
            $("#btnLogin").text("登入");
          }
        }
      });
    }
});
});

PHP 部分

<?php

function ajax\_auth\_init(){ 
  // 註冊 js
  wp\_register\_script('ajax-auth-script', get\_template\_directory\_uri() . '/assets/js/ajax-login.js', array('jquery'), null,true ); 
  wp\_enqueue\_script('ajax-auth-script');

  // 一些要傳給 js 的 php 變數
  wp\_localize\_script( 'ajax-auth-script', 'ajax\_auth\_object', array( 
      'ajaxurl' => admin\_url( 'admin-ajax.php' ),
      'redirecturl' => wp\_get\_referer(),
      'loadingmessage' => \_\_('登入中...')
  ));
  add\_action( 'wp\_ajax\_nopriv\_ajaxlogin', 'ajax\_login' );
  add\_action( 'wp\_ajax\_nopriv\_ajaxregister', 'ajax\_register' );
}

// 只有在未登入狀態才引入登入註冊 js
if (!is\_user\_logged\_in()) {
  add\_action('init', 'ajax\_auth\_init');
}
  
function ajax\_login(){
  // nonce 驗證
  check\_ajax\_referer( 'ajax-login-nonce', 'security' );
  auth\_user\_login($\_POST\['username'\], $\_POST\['password'\], '登入成功！'); 
  die();
}

function ajax\_register(){
  // nonce 驗證
  check\_ajax\_referer( 'ajax-register-nonce', 'security' );
  $info = array();
  $info\['user\_nicename'\] = $info\['first\_name'\] = $info\['user\_login'\] = sanitize\_user($\_POST\['username'\]) ;
  $info\['nickname'\] = $info\['display\_name'\] = sanitize\_text\_field($\_POST\['nickname'\]) ;
  $info\['user\_pass'\] = sanitize\_text\_field($\_POST\['password'\]);
  $info\['user\_email'\] = sanitize\_email( $\_POST\['email'\]);
  
  // Register the user
  $user\_register = wp\_insert\_user( $info );
  if ( is\_wp\_error($user\_register) ){ 
      $error  = $user\_register->get\_error\_codes() ;
      if(in\_array('empty\_user\_login', $error))
        echo json\_encode(array('loggedin'=>false, 'message'=>\_\_($user\_register->get\_error\_message('empty\_user\_login'))));
      elseif(in\_array('existing\_user\_login',$error))
        echo json\_encode(array('loggedin'=>false, 'message'=>\_\_('該帳號已被註冊')));
      elseif(in\_array('existing\_user\_email',$error))
        echo json\_encode(array('loggedin'=>false, 'message'=>\_\_('該郵件已被註冊')));
  } else {
    // 發送註冊成功信
    $headers = 'From:  FlashStories<service@flashstories.co>' . "rn";
    $message = '<div>通知信內容</div>';
    wp\_mail( $info\['user\_email'\],'恭喜你完成申請 Flash Stories 會員資格！', $message,$headers );
    auth\_user\_login($info\['user\_login'\], $info\['user\_pass'\], '註冊完成！');
    update\_field('user\_phone',$\_POST\['phone'\],'user\_'.$user\_register);    
    update\_field('user\_status','paid','user\_'.$user\_register);    
  }
  die();
}

function auth\_user\_login($user\_login, $password, $login){
  $info = array();
  $info\['user\_login'\] = $user\_login;
  $info\['user\_password'\] = $password;
  $info\['remember'\] = true;
  $user\_signon = wp\_signon( $info, false );
  if ( is\_wp\_error($user\_signon) ){
    echo json\_encode(array('loggedin'=>false, 'message'=>\_\_('帳密有誤！請重新輸入')));
  } else {
    wp\_set\_current\_user($user\_signon->ID); 
    echo json\_encode(array('loggedin'=>true, 'message'=>\_\_($login.' 轉址中...')));
  }
  die();
}

### **二、前台管理文章**

因為每一篇 Story 我是用 Post 來做的，新增文章時直接用 wp\_insert\_post() 搭配 Ajax 來解決，而這邊我是搭配 Fetch API 來做新增，前端的部分在上文已有提及，後端的邏輯與登入註冊的寫法大同小異。

刪除文章的部分使用 jQuery Ajax 搭配 wp\_delete\_post()，因為這段 JS 是直接寫在 PHP 裡面，既然都有 include jQuery 就不用假掰的再用 Fetch API 來處理，不然環境問題好難搞。

<?php

function delete\_scripts() {
 
global $wp\_query; 

wp\_register\_script( 'delete\_post', '', '',null );
  wp\_localize\_script( 'delete\_post', 'delete\_params', array(
    'ajaxurl' => site\_url(). '/wp-admin/admin-ajax.php'
));
  wp\_enqueue\_script( 'delete\_post' );
 
}
 
add\_action( 'wp\_enqueue\_scripts', 'delete\_scripts' );


function delete\_ajax\_handler(){
$nonce = $\_POST\['security'\];
if (!wp\_verify\_nonce($nonce, 'ajax-delete-nonce')) {
wp\_send\_json\_error(array('code' => 500, 'data' => '', 'msg' => '錯誤的請求'));
}

  $post\_id = $\_POST\['postId'\];

  wp\_delete\_post($post\_id, true);
  
echo json\_encode(
    array(
'msg'=>"刪除成功！",
    )
  );
die;
}
 
add\_action('wp\_ajax\_deletepost', 'delete\_ajax\_handler');

$(function () {
  $('.btnDelete').on('click', function () {
    var r = confirm('確定要刪除？');
    if (r) {
      action = 'deletepost';
      postid = $(this).attr('data-id');
      security = $('#security').val();
      $.ajax({
        url: delete\_params.ajaxurl,
        type: 'POST',
        dataType: 'json',
        data: {
          'action': action,
          'postId': postid,
          'security': security
        },
        success: function (data) {
          alert(data.msg);
          window.location.reload();
        }
      });
    }
  })
})

### **三、WooCommerce 新增帳號頁面內容**

因為預設的 WooCommerce 沒有頁面可以拿來做文章列表、以及作者資訊頁的設定，所以使用 WC API 來新增這兩個節點，這樣就可以在 my-account 的網址下加入新的頁面。

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-18.44.42-1024x589.jpg)

![](https://oberonlai.blog/wp-content/uploads/2020/03/CleanShot-2020-03-19-at-20.10.20-1024x757.jpg)

此外還移除的不需要的頁面以及修改頁面名稱，主要是用 woocommerce\_account\_menu\_items 這個 filter 來修改。

// 加入新的會員帳號頁面
function add\_endpoint() {
  add\_rewrite\_endpoint('my-brand', EP\_ROOT  EP\_PAGES);
  add\_rewrite\_endpoint('my-stories', EP\_ROOT  EP\_PAGES);
  add\_rewrite\_endpoint('price', EP\_ROOT  EP\_PAGES);
}
add\_action( 'init', 'add\_endpoint' );

function add\_query\_vars($vars) {
  $vars\[\] = 'my-brand';
  $vars\[\] = 'my-stories';
  $vars\[\] = 'price';
  return $vars;
}
add\_filter('query\_vars', 'add\_query\_vars', 0);

function add\_link\_my\_account($items) {
  $items\['my-brand'\] = '品牌資訊設定';
  $items\['my-stories'\] = '我的影像故事';
  $items\['price'\] = '更改方案';
  return $items;
}
add\_filter('woocommerce\_account\_menu\_items', 'add\_link\_my\_account');

// 移除不需要的選單
function remove\_address\_my\_account($items) {
  unset($items\['edit-address'\]);
  unset($items\['downloads'\]);
  return $items;
}
add\_filter('woocommerce\_account\_menu\_items', 'remove\_address\_my\_account', 999);

// 修改選單名稱
function rename\_address\_my\_account($items) {
  $items\['orders'\] = '付款紀錄';
  $items\['edit-account'\] = '帳戶設定';
  return $items;
}
add\_filter('woocommerce\_account\_menu\_items', 'rename\_address\_my\_account', 999);

### **四、拆解 JSON 並寫入 ACF 欄位中**

這是後端我覺得最難處理的部分，因為欄位多再加上迴圈有兩層，ACF 我是用 Repeat 欄位當作頁面，然後每個小工具也是 Repeat，因為一個頁面可以有多個小工具，然後小工具裡面又有一堆屬性，像是尺寸、顏色、圖片路徑、座標等等。

按下儲存後我是先把 Slider 裡面所有的 HTML 用字串塞進 LocalStorage 裡面，然後在 ACF 開一個欄位存這些字串，接下來用正規表示法把字串中特定的 HTML div 先抓出來，每個 div 都是一個小工具，帶有屬性 data-config。

在這之前本來很不熟正規法，這樣玩過一次後就應該再也不會忘了，Mac 有一個很棒的工具叫做 [Expressions](https://www.apptorium.com/expressions)，隨便亂湊一通就能找到正確的寫法，大推！！！

用 preg\_match\_all 找出小工具的 div 後，再使用 PHP DOMDocument 去找小工具裡面帶有 style 屬性的 HTML 標籤，像文字小工具的標籤是 <p>，而 style 都是在 <p> 上面，所以必須要先找到它。

// 正規法篩選出文字小工具的 html
$reg\_text = '/<div(\\s)class="uk-.\*"(\\s)style=".\*"(\\s)href=".\*"(\\s)uk-toggle=""(\\s)data-config="pageText(\\d){0,}"(\\s)data-frame="frame(\\d){0,}"><p(\\s)style=".\*"(\\s)data-html=".\*">.\*<\\/p>/';
preg\_match\_all($reg\_text, $page\_content\[$i\], $page\_text);

// 找出 div 跟 p
$dom\_text = new DOMDocument();
$dom\_text->loadHTML(mb\_convert\_encoding($page\_text\[0\]\[$j\], 'HTML-ENTITIES', 'UTF-8'));
$div = $dom\_text->getElementsByTagName('div');
$p = $dom\_text->getElementsByTagName('p');

// 把 p 裡面的 style 轉成陣列
$page\_text\_p\_decode = style\_json($node->getAttribute('style'));

再來是要把 <p> 之中 style="" 裡面所有的 css 屬性抓出來，然後以分號去做陣列化，然後再用正規法去整理比較麻煩的 CSS 屬性，像是 font-family 字型名稱中間會有空格、transform rotate 屬性會有 deg 的字眼要移掉，都整理完後重新 json\_encode，最後再 return json\_decode 的結果。

// style\_json
function style\_json($css){
  $aa = explode("; ", $css);
  for($i=0; $i < count($aa ); $i++){
    $key\_value = explode(': ', $aa \[$i\]);
    $key\_value\[1\] = str\_replace("/font-family:\\s\\"\\"/","",$key\_value\[1\]);
    $key\_value\[1\] = preg\_replace('/rotate\\(g\\);/','',$key\_value\[1\]);
    $key\_value\[1\] = str\_replace("'",'',$key\_value\[1\]);
    $res\_array\[preg\_replace('/\\s-/','',$key\_value\[0\])\] = $key\_value\[1\];
  }
  $css\_encode = json\_encode($res\_array);
  return json\_decode($css\_encode);
}

然後每個小工具都經過上面的處理之後，再用 ACF 的 update\_field()&update\_sub\_field() 去做資料寫入的工作，除了儲存小工具的屬性外，其他還有頁尾設定、側欄選單、社群分享等其他設定也一併寫入，這才順利的把所有資料寫進資料庫。

### **五、AMP Stories**

資料寫入都搞定後，剩下的就是顯示 AMP Stories 的內容了，它的語法不難，因為 Google 用 Web Component 技術把所有元件都封裝完成了，只要根據官方文件引入相對應的 JS，就可以使用內建的標籤來呈現 AMP Stories 的頁面形式。

因為我是用文章當作 AMP Stories 的內頁，所以模板直接使用 Single.php，把該拿的資料拿到後再去根據資料類型去判斷要輸出的 amp tag，比較麻煩部分是在 Bookend 這一塊。

Bookend 包含了社群分享連結、自訂文字、自訂連結等內容，而它只能透過 JSON 格式來寫入，所以只要少一個逗號就會噴錯導致整個 Bookend 無法顯示。

但使用者不一定每個欄位都會填寫，然後有些欄位有因果關係，必須要有 A 才會有 B，這讓我在使用 PHP 來組成 JSON 的過程中非常棘手，後來睡了一覺後驚覺自己怎麼這麼傻，PHP 明明有 json\_encode 可以用，

先把 PHP Array 組好、判斷寫好後，再去輸出 encode 的結果，這樣處理省事多了，但我覺得根本解還是希望 Google 一樣可以把 Bookend 封裝成獨立的元件而不要用 JSON，就不用處理一堆逗號的問題了。

<?php
  $bookend\_link\_arr = \[\];
  if(get\_field('bookend\_link')){
    foreach (get\_field('bookend\_link') as $item) {
      $bookend\_link\_obj = \[\];
      $bookend\_link\_obj\['type'\] = $item\['bookend\_link\_type'\];
      $bookend\_link\_obj\['title'\] = $item\['bookend\_link\_h'\];
      $bookend\_link\_obj\['url'\] = $item\['bookend\_link\_url'\];
      $bookend\_link\_obj\['image'\] = $item\['bookend\_link\_src'\];
      $bookend\_link\_arr\[\] = $bookend\_link\_obj;
    }
    $bookend\_link\_arr = str\_replace('\[','',json\_encode($bookend\_link\_arr,JSON\_UNESCAPED\_UNICODE));
    $bookend\_link\_arr = str\_replace('\]','',$bookend\_link\_arr);
  }

  $bookend = array(
    'bookendVersion' => 'v1.0',
    'shareProviders' => get\_field('bookend\_share'),
    'components' => array(
      $bookend\_link\_arr,
      array(
        "type" => "cta-link",
        "links" => array(
          array(
            "text" => (get\_field('bookend\_button\_text'))?get\_field('bookend\_button\_text'):"了解 Flash Stories",
            "url" => (get\_field('bookend\_button\_href'))?get\_field('bookend\_button\_href'):"https://flashstories.co"
          )
        )
      )
    )
  );
?>
  
// HTML
<amp-story-bookend layout="nodisplay">
  <script type="application/json">
    <?php echo str\_replace(',",',',',str\_replace('}",','},',str\_replace('},"','},',str\_replace('\\"','"',json\_encode( $bookend,JSON\_UNESCAPED\_UNICODE ))))); ?>
  </script>
</amp-story-bookend>

## 小結

努力了這麼久，當然還是希望 AMP Stories 哪天突然大爆發，然後網站爆量要瘋狂加機器(幻想中)，但這一路走來的過程真的受到太多人幫忙，也因此學到了超多，很多都是第一次實作所以也踩了一堆雷，但只要還沒有被炸得粉身碎骨就一定會變得更強！

本來沒打算寫下紀錄的，但如果不好好看重自己已經完成的成就，很容易就會染上「[冒牌者症候群](https://www.justgirl.me/2020/03/blog-post.html)」。在寫這篇的同時看著之前寫的程式碼，覺得好陌生，邊梳理的過程中才慢慢回想起自己的思考方式以及解決問題的邏輯，甚至還可以看到自己的盲點。

現在覺得可以這樣退後一步重新省視自己寫的程式是非常重要的，不僅能加強自己的自信，還能分享自身的經驗來得到更多的交流，所以之後只要是客戶狀況允許，我都會把每個專案的心得&程式紀錄下來，就當作是一種結案的慶祝儀式吧XD

ps. 關於用 WordPress 製作 Web App 有太多東西可以研究了，現在每天以龜速在整理[這本書](https://oberonlai.blog/【-書摘-】building-web-apps-with-wordpress/)的讀書筆記，有興趣的朋友必讀！對於 Flash Stories 有任何想法的話都可以在下面留言喔！