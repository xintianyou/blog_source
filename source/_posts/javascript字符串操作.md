---
title: javascript字符串操作
date: 2019-04-22 22:15:48
tags: 
    - js
    - 字符串
categories: 
    - web前端
---

### js截取两个字符串之间的内容：

```
    var str = "aaabbbcccdddeeefff";
    str = str.match(/aaa(\S*)fff/)[1];
    alert(str);//结果bbbcccdddeee
```
### js截取某个字符串前面的内容：
```
    var str = "aaabbbcccdddeeefff";
    str = str.match(/(\S*)fff/)[1];
    alert(str);//结果aaabbbcccddd
```
### js截取某个字符串后面的内容：
```
    var str = "aaabbbcccdddeeefff";
    str = str.match(/aaa(\S*)/)[1];
    alert(str);//结果bbbcccdddeeefff
```
