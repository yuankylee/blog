---
title: "js笔记"
date: "2016-03-08"
description: "js笔记"
tags: ["js", "note"]
---

### 在作为判断条件时，下面这些值将被计算出 false :

* **false**
* **undefined**
* **null**
* **0**
* **NaN**
* **空字符串 ("")**

*当传递给条件语句时，所有其他值，包括所有对象会被计算为 **true** 。*


### 六种是 原型 的数据类型:

* **Boolean**.  布尔值，true 和 false.
* **null**. 一个表明 null 值的特殊关键字。 JavaScript 是大小写敏感的，因此 null 与 Null， NULL或其他变量完全不同。
* **undefined**.  变量未定义时的属性。
* **Number**.  表示数字，例如： 42 或者 3.14159。
* **String**.  表示字符串，例如："Howdy"
* **Symbol** ( 在 ECMAScript 6 中新添加的类型).。一种数据类型，它的实例是唯一且不可改变的。

### function 的几种常用定义方式

```javascript
function demo() {
  //code...
}
```

```javascript
var demo = function() {
  //code...
};
```

```javascript
(function(){
  //code...
})();
```
```javascript
function demo(){
  //code...
}
demo.prototype={
  //code...
};
var item = new demo();
```


### 函数的作用域

在函数内定义的变量不能从函数之外的任何地方取得，因为变量仅仅在该函数的域的内部有定义。相对应的，一个函数可以取得在它的域中定义的任何变量和子函数。换言之，定义在全局域中的函数可以取得所有定义在全局域中的变量。而定义在一个函数内部的子函数可以取得定义在其父函数内的，或已经由其父函数取得的任何变量。

```javascript
// 下面的变量定义在全局作用域中
var a = 2,
    b = 5,
    text = "My age is";

// 本函数定义在全局作用域
function demo1() {
  return a * b;
}

demo1(); // Returns 10

// 嵌套函数的例子
function demo2 () {
  var a = 2,
      b = 5;
  
  function add() {
    return text +  (a + b);
  };
  
  return add();
}

demo2(); // Returns "My age is 7"
```