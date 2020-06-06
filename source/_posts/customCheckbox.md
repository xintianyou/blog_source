---
title: 自定义复选框
date: 2019-04-24 22:23:35
tags:
    - 布局
    - CSS
categories: 
    - [web前端]
    - [CSS]
---

### 难题
&emsp;&emsp;设计师对网页中各种元素的控制欲是永无止境的。当 CSS 经验不足的UI设计师接到一个网页设计任务时，Ta几乎一定会为各种表单元素设计一套自己的样式，这会让前端工程师感到崩溃。 当CSS最初出现时，它对表单元素的样式控制力是极为有限的，而且现在仍然没有哪个 CSS 规范明确定义了这方面的行为。不过这些年来，各种浏览器已经在逐步放开CSS属性对表单控件的作用范围，从而允许我们设置大多数表单控件的样式。 
&emsp;&emsp;不幸的是，复选框和单选框不在此列。直到今天，这两种控件在绝大多数浏览器中仍然是几乎或完全无法设置样式的。这最终导致网页开发者接受默认样式，或者求助于一些糟糕透顶、可访问性极差的 hack 方案，比如用 div 和 JavaScript 来模拟这两种控件。
&emsp;&emsp;有没有一种方法，既可以克服这些限制、自由定制复选框的外观，同时又可以摆脱臃肿的代码、保全结构层的语义和可访问性呢？

### 解决方案
&emsp;&emsp;几年前，这个任务还无法在脱离脚本的情况下完成。不过，在选择符（第三版）[http://w3.org/TR/css3-selectors](http://w3.org/TR/css3-selectors)中，我们得到了一个新的伪 类 :checked。这个伪类只在复选框被勾选时才会匹配，不论这个勾选状态 是由用户交互触发，还是由脚本触发。
&emsp;&emsp;如果直接对复选框设置样式，那么这个伪类并不实用，因为（前面已经 交待过了）没有多少样式能够对复选框起作用。不过，我们倒是可以基于复 选框的勾选状态借助组合选择符来给其他元素设置样式。
&emsp;&emsp;你可能还没明白，要根据复选框的勾选状态来给哪个元素设置样式？其实有一个元素总是跟复选框形影不离、息息相关，它就是 label（标签元素）。当 label 元素与复选框关联之后，也可以起到触发开关的作用。
&emsp;&emsp;由于 label 不是复选框那样的替换元素，我们可以为它添加生成性内 容（伪元素），并基于复选框的状态来为其设置样式。然后，就可以把真正 的复选框隐藏起来（但不能把它从 tab 键切换焦点的队列中完全删除），再把生成性内容美化一番，用来顶替原来的复选框！
&emsp;&emsp;让我们来亲手试一试。先从下面这段简单的结构代码开始：
##### 创建替换元素
```
    <input type="checkbox" id="awesome" /> 
    <label for="awesome">Awesome!</label>
```
&emsp;&emsp;接下来需要生成一个伪元素，作为美化版的复选框。我们先给这个伪元素加上一些基本的样式：
```
    input[type="checkbox"] + label::before {
        content: '\a0'; /* 不换行空格 */
        display: inline-block;
        vertical-align: .2em;
        width: .8em;
        height: .8em;
        margin-right: .2em;
        border-radius: .2em;
        background: silver;
        text-indent: .15em;
        line-height: .65;
    }
```
&emsp;&emsp;看下效果
[![微信截图_20190424223820.png](https://i.loli.net/2019/04/24/5cc0750bcca1b.png)](https://i.loli.net/2019/04/24/5cc0750bcca1b.png)
##### 添加样式
&emsp;&emsp;原来的复选框仍然是可见的，待会儿我们会将其隐藏。现在需要给复选框的勾选状态加上不同 的样式。样式可以很简单，比如换种颜色，再加上勾选标记：
```
    input[type="checkbox"]:checked + label::before {
        content: '\2713';     
        background: yellowgreen; 
    }
```
[![微信截图_20190424224415.png](https://i.loli.net/2019/04/24/5cc0765223e1e.png)](https://i.loli.net/2019/04/24/5cc0765223e1e.png)
&emsp;&emsp;在上图中可以看到，这个伪元素已经俨然是一个经过简单美化的复选框了，而且功能完备。现在，我们需要把原来的复选框以一种不损失可访问性的方式隐藏起来。这意味着不能使用 display: none，因为那样会把它从键盘 tab 键切换焦点的队列中完全删除。我们改用另一种方法来达到目的：
```
    input[type="checkbox"] {
        position: absolute;
        clip: rect(0,0,0,0);
    }
```
&emsp;&emsp;这就完成了，我们得到了一个简单定制化的复选框！我们还可以进一步优化，比如在它聚焦或禁用时改变它的样式（效果如下图所示）：
```
    input[type="checkbox"]:focus + label::before {
        box-shadow: 0 0 .1em .1em #58a; 
    } 
 
    input[type="checkbox"]:disabled + label::before {
        background: gray;
        box-shadow: none;
        color: #555; 
    }
```
[![微信截图_20190424224856.png](https://i.loli.net/2019/04/24/5cc077701a9dd.png)](https://i.loli.net/2019/04/24/5cc077701a9dd.png)
>三个复选框分别的状态是：
>上图：自定义复选框的聚焦状态； 中图：自定义复选框被禁用的状态；下图：自定义复选框被勾选的状态

>本文转自《CSS揭秘》，还有更多CSS奇技淫巧欢迎在评论区留言！



