---
title: Scroll Progress Indicator Implement
tags: []
id: '667'
categories:
  - - F2E
date: 2020-02-03 16:08:55
---

```
#scrollIndicatior(style="height:6px;background-color:#ffe403;width:0;")
```

```
window.onscroll = function () { scrollIndicatior() };

function scrollIndicatior() {
  var winScroll = document.body.scrollTop  document.documentElement.scrollTop;
  var height = document.documentElement.scrollHeight - document.documentElement.clientHeight;
  var scrolled = (winScroll / height) * 100;
  vars.scrollIndicatior.style.width = scrolled + "%";
}
```

[https://www.w3schools.com/howto/howto\_js\_scroll\_indicator.asp](https://www.w3schools.com/howto/howto_js_scroll_indicator.asp)