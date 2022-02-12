---
title: nuxt.js使用swiper的经过及遇到的坑
date: 2020-12-12 17:04:45
tags: 
    - VUE
    - NUXT.JS
    - JS
categories: 
    - web前端
---

最近有一个企业官网项目用到了nuxt.js
需求：
首页有一个左右滑动的展示效果，果断使用swiper，直接上网搜索关键字“nuxt使用swiper”。
结果：

## 1. 安装

 

```bash
npm i swiper vue-awesome-swiper -S
```
我的版本：

```bash
"swiper": "^6.4.1",
"vue-awesome-swiper": "^4.1.1"
```

## 2. 引入

 在plugins文件夹下新建vue-swiper.js，在nuxt.config.js中引入
 

```bash
plugins: [
   { src: "~/plugins/vue-swiper.js", ssr: false }
],
```

看到网上两种vue-swiper.js写法
 - 第一种

```bash
import Vue from 'vue'
import VueAwesomeSwiper from 'vue-awesome-swiper'
import css from 'swiper/dist/css/swiper.css'
Vue.use(VueAwesomeSwiper, {css})

```
- 第二种

```bash
import Vue from 'vue'
import VueAwesomeSwiper from 'vue-awesome-swiper'
export default () => {
  Vue.use(VueAwesomeSwiper)
}
```
这种需要在nuxt.config.js中引入css

```bash
css: [
  "swiper/dist/css/swiper.css"
],
```

## 3. 经验

**第一个坑**：
npm run dev启动就报错
This dependency was not found:

swiper/dist/css/swiper.css in ./src/main.js
To install it, you can run: npm install --save swiper/dist/css/swiper.css

解决办法：
使用import 'swiper/swiper-bundle.css'， 替换import 'swiper/css/swiper.css'

在此感谢简书大佬[原文地址](https://www.jianshu.com/p/2b4fffc97de5)的文章

**第二个坑：**（当初使用了第一种vue-swiper.js引入的写法）

 - 刷新页面的一瞬间，横向布局的slide变成了竖向排列，相当于块状元素自然换行的效果，也没有了滑动效果，当时给父元素设置display: flex;解决了
 - 刷新页面的一瞬间，swiper中所有的slide项全部显示了，每一个slide的宽度都变为以内容撑开的最小宽度

最终用第二种vue-swiper.js写法，两个坑一并解决
 
