---
title: 使用 Vuex 來控制燈箱效果狀態
tags: []
id: '139'
categories:
  - - Vue
date: 2019-02-19 23:06:26
---

![](https://i2.wp.com/oberonlai.blog/wp-content/uploads/2019/02/Carbonize-2019-02-19-at-9.59.07-PM.png?fit=1024%2C807&ssl=1)

今天碰到一個難題，我把每一個燈箱的顯示視窗獨立成元件，我想要用 A 元件裡面的按鈕去觸發燈箱元件的顯示，然後再用燈箱元件裡面的關閉按鈕來隱藏它，這就牽涉到一些問題：

1.  燈箱元件如果是 A 元件的子組件，我可以用 props 去傳值給燈箱，但問題在於關閉的時候必須要從燈箱元件去控制父元件的參數，似乎可以用 $emit 但感覺好像又行不通
2.  如果燈箱元件跟 A 元件並不是父子關係，props 就無用了
3.  我的燈箱元件不只一個，而且 B 燈箱裡面會有觸發 C 燈箱的按鈕

好吧，事實上我心裡早就有逃不掉用 Vuex 的打算了，只是因為一直看不懂它，所以就能閃則閃，研究了一天後，大概只了解了三成，但也夠我解決問題了，剩下的部分就等有遇上再說吧。

我的目標流程是這樣：

*   一個主要模板 Layout.vue，用來引入燈箱 A、燈箱 B 以及 Header.vue
*   Header.vue 裡面有按鈕，點擊後會觸發燈箱 A
*   燈箱 A 裡面有按鈕，點擊後會關閉燈箱 A 並且開燈箱 B

Vue 最大好處是中國人開發的，有很多中文資料，但他們的中文我真的有看沒有懂，雖然也有很多資料是繁體中文的整理，但大部分就是把官方文件繁體化，語意還是原本的簡中版本，所以還是看得霧煞煞。

經過一天的實驗以及碰壁後，總算釐清了一些關於 Vuex 的基本概念，以下是我自己理解的繁中白話文解釋：

*   Vuex 是把變數變成全域的元件，相較於寫在元件裡的 data 只能給元件自己用，Vuex 的變數是所有元件都能存取
*   state 就等於元件裡面的 data，可以是物件
*   mutation 是唯一能改變 state 的方法，元件不能直接修改 state，會噴錯
*   modules 是可以把 state、mutations 這些 Vuex 的狀態變成物件，萬一有一堆東西要用 Vuex 來管理才不會超亂
*   store.js 是引入 modules 的母檔案，而主要全站入口 app.js 會引入 store.js，所以所有的元件都能讀到 state 裡面的變數
*   元件裡面是用 computed 來取得 state 值，因為我是用 v-model 來決定燈箱狀態，所以需要設 get 與 set function，不然會噴 Computed property "modalState" was assigned to but it has no setter 這個錯誤
*   至於 actions、getters、namespace 那些還看不懂...

接下來進行實作，首先是增加一個 module 叫做 modals.js，裡面是控制所有燈箱的顯示狀態布林值，以及修改狀態的 mutations，然後把 store.js 引入到全站入口 app.js，接著去 A 燈箱使用 computed 去讀取 state、使用 methods 去定義 mutations，最後在 header.vue 也加入 mutations 方法，讓相對應的按鈕 click 後觸發。

```
/** modals.js **/
export const modals = {
  state: {
    isModalaActive: false, // Ａ燈箱狀態
    isModalbActive: false, // B燈箱狀態
  },
  mutations: {
    // 切換登入彈跳視窗狀態
    modalAToggle ( state ) {
      state.isModalAActive = !state.isModalAActive // 當 isModalAActive 為 false 時變成 true，反之為 true 時變成 false
    },
    // 切換登入彈跳視窗狀態
    modalBToggle ( state ) {
      state.isModalBActive = !state.isModalBActive
      state.isModalAActive = false // 出現 B 燈箱時就要隱藏 A 燈箱
    },
  }
}
```

接下來是把 modal.js 引入到 store.js

```
/** store.js **/

// 給 IE11 用的 polyfill
require ('es6-promise').polyfill()

import Vue from 'vue'
import Vuex from 'vuex'

Vue.use( Vuex )

// 我的 modal.js 是放在 modules 資料夾底下
import { modals } from './modules/modals.js'

export default new Vuex.Store({
  // 引入 modal.js
  modules: {
    modals
  }
})
```

再把 store.js 引入到全站入口 app.js

```
/** app.js **/

// 略...
import store from "./store.js";

new Vue({
  router,
  store
}).$mount('#app')
```

在 A 燈箱元件加入 state 與 mutations，framework 我是用 uikit 的 vue 版本 [vuikit](https://vuikit.js.org)，原始版本用得很開心，但 vuikit 很多元件都沒整合，很多 UI 不是要自己刻就是要另外找套件，vk-modal 是它的燈箱元件，使用 :show.sync 去決定顯示與否。

```
<template lang='pug'>
  div
    vk-modal.modal-small-width.modal-remove-padding(:show.sync='modalState', center, stuck)
      vk-modal-close(@click='modalAToggle')
      a.text-dark.link-underline(href='#' @click='modalBToggle') 連到 B 燈箱的按鈕
</template>
<script>
export default {
  computed: {
    // modalState 是儲存從 store 裡面拿到的 state 值，寫法是 this.$store.state.module名.變數名，這邊卡超久@@
    modalState: {
      // 沒有 set 會出錯，get 記得要有 return
      get(){
        return this.$store.state.modals.isModalAActive
      },
      set(){
        this.$store.state.modals.isModalAActive
      }
    }
  },
  methods: {
    // 註冊 mutations 事件，就可以給元件使用
    modalAToggle(){
      this.$store.commit('modalAToggle');
    },
    modalBToggle(){
      this.$store.commit('modalBToggle');
    },
  },
}
</script>
```

最後是 Header.vue，幫它也加入 modalAToggle 的 methods，這邊不需要 computed，因為沒有要取得燈箱狀態的功能，只要有 mutations 可以改變它就好。

```
<template lang='pug'>
  div
    a(href='#' @click='modalAToggle') 要開啟 A 燈箱的按鈕
</template>
<script>
export default {
  methods: {
    modalAToggle(){
      this.$store.commit('modalAToggle');
    },
  },
}
</script>
```

用 Vuex 就可以很有組織的完成這些需要參數傳來傳去的複雜問題，元件之間的溝通也容易了許多，相信再多用一陣子也逃不了要面對 actions 跟 getters 了啊啊啊～