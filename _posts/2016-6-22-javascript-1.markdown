---
layout:     post
title:      "Javascript爬坑记(一)——预解释"
subtitle:   "预解释、预编译、变量提升"
date:       2016-6-22
author:     "小久哥"
header-img: "http://7xp21g.com1.z0.glb.clouddn.com/img/article/tpbg/javascript.jpg"
tags:
    - javascript
    - js
---

> Javascript基础知识整理~第一篇。

***

### 全局作用域
当浏览器加载HTML页面的时候，首先会提供一个全局JS代码执行的环境->全局作用域（global/window）。

### js中变量和内存
* 栈内存：用来提供一个供js代码执行的环境 ->作用域（全局作用域/私有作用域）
* 堆内存：用来存储引用数据类型  ->对象存储的是属性名和属性值，函数存储的是代码字符串

### 变量提升
在当前的作用域中，JS代码执行之前，浏览器会首先默认的把所有带有`var`和`function`的进行提前的‘声明’和‘定义’。但是`var`和`function`两个有差别，`var`在这个阶段只会提前‘声明’，而`function`在这个阶段会‘声明’和‘定义’。变量提升只会发生在，当前的作用域下，例如：开始只对window下的进行变量提升，只有函数执行的时候才会对函数中的进行变量提升。

### 注意事项
不加var的，不会进行变量提升。

### 实例代码

```
console.log(a);   // undefined
var a = 1; 
console.log(a);   // 10
add();  // 3
function add(){
	var a = 2; 
	console.log(this.a + a); 
}
add();  // 3

console.log(b);  // 报错
b = 4 ;
```

```
// num 变量提升，但是没定义，所以为undefined，所以判断为true
if(!("num" in window)){
	var num = 100;
}
console.log(num);    //undefined
```