---
title: 函数作用域、闭包与this指向问题
date: 2020-6-6 15:30:26
tags: 
    - js
categories: 
    - web前端
    - js
---

## 作用域
>作用域是可访问变量的集合，在JavaScript中对象和函数同样是变量，作用域为可访问变量，对象，函数的集合。作用域可以分为全局作用域和局部作用域。
### 全局作用域
>变量在函数外定义，即为全局变量，全局变量有全局作用域，网页中所有脚本和函数都可以使用。如果变量在函数内没有声明，也是全局变量。

```
  var name = "hello World";
  // 此处可调用 name 变量
  function myFunction() {
      // 函数内可调用 name 变量
  }
```

```
  // 此处可调用 name 变量
  function myFunction() {
      name = "hello World";
      // 此处可调用 name 变量
  }
```

### 局部作用域
>变量在函数内声明，变量为局部作用域，只能在函数内部访问。

```
  // 此处不能调用 name 变量
  function myFunction() {
      var name = "hello World";
      // 函数内可调用 name 变量
  }
```

>局部变量只作用于函数内，所以不同的函数可以使用相同名称的变量。局部变量在函数执行时创建，函数执行完毕后局部变量就会自动销毁。
>JavaScript变量生命周期，局部变量函数执行完毕后销毁，全局变量在页面关闭后销毁。函数参数只在函数内起作用，属于局部变量。

## 闭包
### 闭包
>函数A内部有函数B，函数B可以访问函数A的变量，那么函数B就是闭包。本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

```
  function A(){
      var a = 123;
      function B(){
          console.log(a) //123
      }
      return B()
  }
  A()();
```

### 闭包有3大特性：
+ 函数嵌套函数
+ 函数内部可以引用函数外部的参数和变量
+ 参数和变量不会被**[垃圾回收机制](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Memory_Management)**回收

### 闭包优点：
+ 可读取函数内部的变量
+ 局部变量可以保存在内存中，实现数据共享
+ 执行过程所有变量都匿名在函数内部

### 闭包缺点
+ 使函数内部变量存在内存中，内存消耗大
+ 滥用闭包可能会导致**[内存泄漏](http://www.ruanyifeng.com/blog/2017/04/memory-leak.html)**
+ 闭包可以在父函数外部改变父函数内部的值，慎操作

### 使用场景
+ 模拟私有方法
+ setTimeout的循环
+ 匿名自执行函数
+ 结果要缓存场景
+ 实现类和继承

## this指向
>this是在函数运行时，在函数体内部自动生成的一个对象，只能在函数体内部使用。几种情况，在不同的环境下会有不同的值。

1.对于直接调用函数来说，不管foo函数被放在了什么地方，this的指向一定是window
```
  var a = 1
  function foo() {
      console.log(this.a)
  }
  foo()
```
2.对于obj.foo()来说，谁调用了函数那么谁就是this
```
  const obj = {
  a: 2,
  foo: foo
  }
  obj.foo()
```
3.对于new操作实例化来说，this就会绑定在实例化对象上面且不会被改变
```
  const c = new foo()
```

## 参考
[阮一峰 JavaScript 的 this 原理](http://www.ruanyifeng.com/blog/2018/06/javascript-this.html)
[阮一峰 Javascript 的 this 用法](http://www.ruanyifeng.com/blog/2010/04/using_this_keyword_in_javascript.html)