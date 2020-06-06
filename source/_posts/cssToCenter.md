---
title: CSS设置居中的方法总结
date: 2020-6-6 19:55:50
tags: 
    - CSS
    - 布局
categories: 
    - [web前端]
    - [CSS]
---

html
```

<div class="parent">
  <div class="child"></div>
</div>

```

css
## 水平居中
### 行内元素
```
.parent {
   text-align: center;
}
```
### 块级元素
```
<!-- margin: auto -->
.parent {
    text-align: center; 
}
.child {
    width: 100px;
    margin: auto; 
    border: 1px solid blue;
}

<!-- 定位 -->
.parent {
  position: relative;
}
.child {
  width: 100px;
  position: absolute;
  left: 50%;
  margin-left: -50px;
}
```

## 垂直居中
### 行内元素
```
<!-- 行内元素（单行文字垂直居中）：设置 line-height = height -->
.parent {
   height: 200px;
   line-height: 200px;
}
<!-- 父元素未知高度：适用于多行文字垂直居中 -->
.parent {
  display: table;
}
.child: {
  display: table-cell;
  vertical-align: middle; /** 垂直居中 */
  text-align: center; /** 水平居中 */
}
<!-- 或者 -->
.parent {
    height: 300px;
    display: table-cell;        
    vertical-align: middle;
}
.child {
    vertical-align: middle;
    background: blue;
}
```

### 块级元素
#### 绝对定位
兼容性好，但是需要知道绝对定位元素的高度
```
.parent {
    position: relative;
}
.child {
    width: 80px;
    height: 80px;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-top: -40px;
    margin-left: -40px;
}
```

#### 绝对定位 + transform
不需要知道高度，但是IE9以下不兼容
```
.parent {
    position: relative;
}
.child {
    width: 80px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}
```

#### 绝对定位 + margin: auto;
不需要提前知道尺寸，兼容性好
```
.parent {
    position: relative;
}
.child {
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}
```

#### display: table-cell
```
.parent {
    display: table;
}
.child {
    display: table-cell;
    vertical-align: middle;
}
```
或者
```
.parent {
    height: 300px;
    display: table-cell;        
    vertical-align: middle;
}
.child {
    vertical-align: middle;
    background: blue;
}
```

#### display: flex
部分浏览器要兼容性处理
```
.parent {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  align-items: center; /*垂直居中*/
  justify-content: center;  /*水平居中*/
}

```

#### calc() + padding 
需要做兼容性处理，需要知道尺寸
```
.parent {
    width: 300px;
    height: 300px;
    border: 1px solid red;
}
.child {
    width: 100px;
    height: 100px;
    background: blue;
    padding: -webkit-calc((100% - 100px) / 2);
    padding: -moz-calc((100% - 100px) / 2);
    padding: -ms-calc((100% - 100px) / 2);
    padding: calc((100% - 100px) / 2);
    background-clip: content-box;
}
```