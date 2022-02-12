---
title: 用css实现三角形、聊天对话框
date: 2020-10-30 14:45:35
tags: 
    - CSS
categories: 
    - web前端
---

@[TOC](文章目录)

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

# 前言

<font color=#999AAA >
常见的聊天对话框，如微信，都是由一个长方形加一个三角形组合而成，重点就在于三角形的制作
</font>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

# 一、css制作三角形的实现原理？

三角形的实现原理是元素宽高设置为0，再设置边框宽度、样式和颜色。
<font color="#999">例如：</font>
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201030140633548.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)

# 二、单个三角形
## 1.原理
只设置一条边的颜色，其他三边颜色设置为透明
<font color="#999">例如：</font>

 - 👇 向下三角
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020103014120835.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)

 - 👉 向右三角
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201030141443538.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)
- 👆  向上三角
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201030141646693.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)

- 👈 向左三角
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201030141837337.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpblRpYW5vdTEyMw==,size_16,color_FFFFFF,t_70#pic_center)

## 2.空心三角形
<font color=#999AAA >只能看见边线，内部透明的三角形该如何用css实现呢</font>

实现思路：使用css伪元素定位，用两个不同颜色、不同大小的实心三角形叠加，以达到“空心”的效果

```html
<div class="box"><div>

<style>
.box {
  width: 200px;
  height: 50px;
  background-color: #fff;
  border-radius: 6px;
  position: relative;
  border: 1px solid red;
}
.box::before{
  position: absolute;
  top: 15px;
  left: 200px;
  content: '';
  border-width: 10px;
  border-style: solid;
  border-top-color: transparent;
  border-right-color: transparent;
  border-bottom-color: transparent;
  border-left-color: red;
}
.box::after {
  position: absolute;
  top: 15px;
  left: 198.5px;
  content: '';
  border-width: 10px;
  border-style: solid;
  border-top-color: transparent;
  border-right-color: transparent;
  border-bottom-color: transparent;
  border-left-color: #fff;
}
</style>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201030144430157.png#pic_center)
