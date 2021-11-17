---
title: WordPress 加密後圖片 URL 的處理
tags: []
id: '3345'
categories:
  - - WordPress
date: 2018-02-12 11:05:35
---

把網站改成 https 後，最惱人的問題就是還有一堆沒加密網址造成混合內容而無法得到綠鎖頭，其中佔最大宗的就是圖片路徑，最直接的做法就是必須進到資料庫內做批次修改，但對不熟 SQL 語法的朋友進資料庫的修改就怕有東西改錯造成網站資料毀損，這時候就可以使用 Velvet Blues Update URLs 這支外掛來批次修改圖片路徑，事實上它不只可以修改圖片路徑，還可以修改全站的連結，這對於搬家換網址的情境下也非常適用。

\[mxp\_fb2wp\_display\_embed sender="449900911783585" item="share" post\_id="449900911783585\_1584951078278557" display="yes" title="V29yZFByZXNzIOWKoOWvhuW+jOWclueJhyBVUkwg55qE6JmV55CG" body="5oqK57ay56uZ5pS55oiQIGh0dHBzIOW+jO+8jOacgOaDseS6uueahOWVj+mhjOWwseaYr+mChOacieS4gOWghuaykuWKoOWvhue2suWdgOmAoOaIkOa3t+WQiOWFp+WuueiAjOeEoeazleW+l+WIsOe2oOmOlumgre+8jOWFtuS4reS9lOacgOWkp+Wul+eahOWwseaYr+WclueJh+i3r+W+ke+8jOacgOebtOaOpeeahOWBmuazleWwseaYr+W/hemgiOmAsuWIsOizh+aWmeW6q+WFp+WBmuaJueasoeS/ruaUue+8jOS9huWwjeS4jeeGnyBTUUwg6Kqe5rOV55qE5pyL5Y+L6YCy6LOH5paZ5bqr55qE5L+u5pS55bCx5oCV5pyJ5p2x6KW/5pS56Yyv6YCg5oiQ57ay56uZ6LOH5paZ5q+A5pCN77yM6YCZ5pmC5YCZ5bCx5Y+v5Lul5L2/55SoIFZlbHZldCBCbHVlcyBVcGRhdGUgVVJMcyDpgJnmlK/lpJbmjpvkvobmibnmrKHkv67mlLnlnJbniYfot6/lvpHvvIzkuovlr6bkuIrlroPkuI3lj6rlj6/ku6Xkv67mlLnlnJbniYfot6/lvpHvvIzpgoTlj6/ku6Xkv67mlLnlhajnq5nnmoTpgKPntZDvvIzpgJnlsI3mlrzmkKzlrrbmj5vntrLlnYDnmoTmg4XlooPkuIvkuZ/pnZ7luLjpgannlKjjgII=" pid="3345"\]