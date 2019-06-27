---
title: js获取字符串中指定位置的字符
date: 2019-04-21 22:15:48
tags: 
    - js
    - 字符串
categories: 
    - web前端
---

```
    var str = "aaabbbcccdddeeefff";
```

### js截取两个字符串之间的内容：

```
    str = str.match(/aaa(\S*)fff/)[1];
    alert(str);
    //bbbcccdddeee
```
### js截取某个字符串前面的内容：
```
    str = str.match(/(\S*)fff/)[1];
    alert(str);
    //aaabbbcccddd
```
### js截取某个字符串后面的内容：
```
    str = str.match(/aaa(\S*)/)[1];
    alert(str);
    //bbbcccdddeeefff
```
