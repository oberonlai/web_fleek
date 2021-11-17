---
title: Laravel mix 加入 svg sprite 並設計成 vue component
tags:
  - Laravel
id: '31'
categories:
  - - Vue
date: 2019-01-15 16:39:32
---

Svg Sprite 可以非常方便的合併多張向量圖，讓 request 能少一些，之前純 Vue 的環境下是採用 svg-sprite-loader，無奈丟進 Laravel 裡面不理我，只好找別的解決方案。

測試之後找到它--->svg-spritemap-webpack-plugin，個人覺得他比 svg-sprite-loader 優秀，優秀的點在於它不會一開始就把 svg sprite 塞進來，只有在需要他時再呼叫就好，

以下紀錄設定方法：

一、先移除舊版的 svg-spritemap-webpack-plugin，再裝新的，新版本要在 laravle-mix 4.0 以上才可以用

```
npm uninstall svg-spritemap-webpack-plugin
npm install svg-spritemap-webpack-plugin --save-dev
```

二、在 webpack.mix.js 宣告來源以及編譯後的路徑

```
const SVGSpritemapPlugin = require('svg-spritemap-webpack-plugin’); // 引入
const svgSourcePath = 'resources/js/icons/svg/*.svg';
const svgSpriteDestination= '/svg/sprite.svg’; // 不用加 public
```

三、webpackConfig 加入 plugin 相關設定

```
.webpackConfig({
    plugins: [
        new SVGSpritemapPlugin(
            svgSourcePath, {
                output: {
                    filename: svgSpriteDestination,
                    svgo: true
                },
            }
        )
    ]
})
```

四、新增 vue icon 元件，在 components 裡面新增一個資料夾 icon，裡面新增 index.vue，元件內容如下：

```
<template>
  <svg :width="width" :height="height">
    <use :xlink:href="iconName"/>
  </svg>
</template>

<script>
export default {
  name: 'Icon',

  props: {
    type: {
      default: 'sad'
    },

    width: {
      default: 50
    },

    height: {
      default: 50
    }
  },

  computed: {
    iconName () {
      return '/svg/sprite.svg#sprite-' + this.$props.type
    }
  }
}
</script>

<style scoped>
svg {
  fill: currentColor;
  overflow: hidden;
}
</style>
```

五、在 icons 的資料夾底下也就是 svg 檔案的地方，新增一個 index.js，內容如下：

```
import Vue from 'vue'
import Icon from ‘../components/icon'
Vue.component('icon', Icon)
// requires and returns all modules that match
const requireAll = requireContext => requireContext.keys().map(requireContext)
// import all svg
const req = require.context('./svg', true, /\.svg$/)
requireAll(req)
```

六、在 app.js 引入 icons/index，把所有的 svg 一次引入到各個頁面，省去每次重複引入的麻煩

```
import './icons/index’
```

七、在 vue 裡面就可以使用:

```
<icon type=“svgfilename” width=‘xxx’ height=‘xxx’></icon> 了
```