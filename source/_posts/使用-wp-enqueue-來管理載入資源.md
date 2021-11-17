---
title: 使用 WP Enqueue 來管理載入資源
tags: []
id: '3127'
categories:
  - - WordPress
date: 2017-10-30 11:49:59
---

  WordPress 提供很多 API 來管理網站的資源，WP Enqueue 就是其中最常用的一種。以往在寫網頁的時候，當要引入 javascript 或是 css 檔案時，習慣會用 HTML 標籤 script src 或 link stylesheet，但有些檔案並非在每一頁都會用到，只把需要用到的資源載入是能有效增加頁面速度的方法。而 WP Enqueue 就是使用 php 的方式來管理，先使用 wp\_register\_script 來註冊即將要載入的檔案，再使用 wp\_enqueue\_script 來進行載入，這樣就可以再下判斷條件來決定何時要載入資源，wp\_register\_script 還有許多參數可以使用，像是增加版本號、載入順序、在 head 或 foot 載入等等，能更彈性管理網站資源。
<!-- more -->
  \[mxp\_fb2wp\_display\_embed sender="449900911783585" item="share" post\_id="449900911783585\_1483116688461997" display="yes" title="5L2/55SoIFdQIEVucXVldWUg5L6G566h55CG6LyJ5YWl6LOH5rqQ" body="V29yZFByZXNzIOaPkOS+m+W+iOWkmiBBUEkg5L6G566h55CG57ay56uZ55qE6LOH5rqQ77yMV1AgRW5xdWV1ZSDlsLHmmK/lhbbkuK3mnIDluLjnlKjnmoTkuIDnqK7jgILku6XlvoDlnKjlr6vntrLpoIHnmoTmmYLlgJnvvIznlbbopoHlvJXlhaUgamF2YXNjcmlwdCDmiJbmmK8gY3NzIOaqlOahiOaZgu+8jOe/kuaFo+acg+eUqCBIVE1MIOaomeexpCBzY3JpcHQgc3JjIOaIliBsaW5rIHN0eWxlc2hlZXTvvIzkvYbmnInkupvmqpTmoYjkuKbpnZ7lnKjmr4/kuIDpoIHpg73mnIPnlKjliLDvvIzlj6rmiorpnIDopoHnlKjliLDnmoTos4fmupDovInlhaXmmK/og73mnInmlYjlop7liqDpoIHpnaLpgJ/luqbnmoTmlrnms5XjgILogIwgV1AgRW5xdWV1ZSDlsLHmmK/kvb/nlKggcGhwIOeahOaWueW8j+S+hueuoeeQhu+8jOWFiOS9v+eUqCB3cF9yZWdpc3Rlcl9zY3JpcHQg5L6G6Ki75YaK5Y2z5bCH6KaB6LyJ5YWl55qE5qqU5qGI77yM5YaN5L2/55SoIHdwX2VucXVldWVfc2NyaXB0IOS+humAsuihjOi8ieWFpe+8jOmAmeaoo+WwseWPr+S7peWGjeS4i+WIpOaWt+aineS7tuS+huaxuuWumuS9leaZguimgei8ieWFpeizh+a6kO+8jHdwX3JlZ2lzdGVyX3NjcmlwdCDpgoTmnInoqLHlpJrlj4Pmlbjlj6/ku6Xkvb/nlKjvvIzlg4/mmK/lop7liqDniYjmnKzomZ/jgIHovInlhaXpoIbluo/jgIHlnKggaGVhZCDmiJYgZm9vdCDovInlhaXnrYnnrYnvvIzog73mm7TlvYjmgKfnrqHnkIbntrLnq5nos4fmupDjgII=" pid="3127"\]