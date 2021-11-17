---
title: 一些堪用的 Vue Components 套件
tags: []
id: '148'
categories:
  - - Vue
date: 2019-02-20 09:51:29
---

![](https://i0.wp.com/oberonlai.blog/wp-content/uploads/2019/02/screely-1550626306459.jpg?fit=1024%2C651&ssl=1)

Vue.js Examples 這網站有點像是以前 jQuery Plugin Library 的概念，收集了很多各種封裝好的 UI 元件，內頁還有說明使用方法，但用了幾套後發現有些會跑不起來，可能是版本的問題，我把自己有用過的整理在這篇文章中。

## [Simple and smooth vue accordion](https://vuejsexamples.com/simple-and-smooth-vue-accordion/)

![](https://oberonlai.blog/wp-content/uploads/2019/02/vue-faq-accordion.gif)

```
/* 安裝 */
npm i vue-faq-accordion

/* 使用 */
<template>
  <VueFaqAccordion 
    :items="myItems"
  />
</template>

<script>
import VueFaqAccordion from 'vue-faq-accordion'

export default {
  components: {
    VueFaqAccordion
  },
  data () {
    return {
      myItems: [
          {
            title: '標題',
            value: '內文',
            category: 'Tab-1' // 有輸入 category 會自動分頁籤，不需要頁籤的話就拿掉它
          },
          {
            title: '標題2',
            value: '內文2',
            category: 'Tab-2'
          },
        ]
    }
  }
}
</script>
```

樣式的部分我是用 scss 直接去硬改，每個都下 !important 去覆寫它，它的 css 都是寫 inline，正規作法應該是要改了之後重新編譯它，但好麻煩，理論上可以用選擇器的權限去蓋過。

## [Hooper Slideshow](https://baianat.github.io/hooper/)

![](https://oberonlai.blog/wp-content/uploads/2019/02/CleanShot-2019-02-20-at-09.48.22.png)

```
/* 安裝 */
npm install hooper

/* 使用 */
<template lang="pug">
  div
    hooper(style='height: calc( 750 / 1920 *100% )' :centerMode='true' :wheelControl='false')
      slide( v-for='slide in slides' key='slide.id' )
        img(:src='slide.img_url')
    .uk-container.layout
      h1 Search
</template>
<script>
import { Hooper, Slide } from 'hooper'
import 'hooper/dist/hooper.css';
export default {
  name: '',
  data () {
    return {
      slides: [
        {id: 1, img_url: '../img/slide1.jpg'},
        {id: 2, img_url: '../img/slide1.jpg'}
      ]
    }
  },
  components: {
    Hooper,
    Slide
  },
}
</script>
```

如果要用 v-for 迴圈去產出 slide 的話，記得要加 key，不然會噴錯。另外還可以設置 breakpoints，如果想做成 carousel ，可以下 breakpoints 來各別設定 :itemsToShow。