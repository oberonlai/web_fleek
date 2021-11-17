---
title: Practical Note of Developing
tags: []
id: '615'
categories:
  - - uncategorized
---

## data.json structure

```
"header":[
    {
      "top":[
        {
          "src":"img/icon-hot_news.svg",
          "title":"新聞快訊",
          "text":"聯合競選總部成立"
        }
      ],
      "navbar":[
        {
          "src":"assets/img/logo.svg",
          "nav":[
            {
              "text":"即時",
              "href":"category.html"
            },
          ],
          "navVideo":[
            {
              "src":"assets/img/default_image.png",
              "icon":"assets/img/icon-video_play.svg",
              "href":"single.html"
            }
          ]
        }
      ],
      "keyword":[
        {
          "title":"熱搜關鍵字",
          "list":[
            {
              "text":"兩岸",
              "href":"search.html"
            },
            {
              "text": "2020大選",
              "href": "search.html"
            }            
          ]
        }
      ]
    }
  ]
```

VScode clone shortcut: shift + option + arrow up or down

1.  use data.json to oragnize the data structure and logic
2.  \_layouts/temp.pug head meta, include assets path
3.  \_partials/header.pug: use pug comment // to define the section scope
4.  design the DOM order with comment mark
5.  Make the component after dom structure building

### pug loop snippet: header is json node, top is the child node of header

```
each i in header
  each j in i.top
    p
      img.uk-margin-small-right.uk-text-primary(src=j.src uk-svg)
      span.uk-text-bold.uk-position-relative(style="top:-5px;")=j.title
    p.uk-margin-left
      span.uk-text-bold.text-red.uk-margin-small-right 08:52 AM
      a.text-dark.link-primary(href="single.html")=j.text
```

### pug include snippet

```
include _partials/header/headerTop
```

### check the order in iteration loop

```
each k,index in j.nav
  if index===0
    li.uk-active
  else
    li
```

### pug search form for wp

```
form.uk-position-relative(method='get' action="")
  input.uk-input(type='text' max='100' name='s' placeholder='輸入關鍵字...')
  button.uk-position-right-center(uk-icon='search' type='submit')
```