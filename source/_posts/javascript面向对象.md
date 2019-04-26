---
title: javascript面向对象
date: 2019-02-21 22:28:11
tags: 面向对象
type: categories
---

JavaScript 的核心是支持面向对象的，同时它也提供了强大灵活的 OOP 语言能力,
面向对象编程是用抽象方式创建基于现实世界模型的一种编程模式。它使用先前建立的范例，包括模块化，多态和封装几种技术。今天，许多流行的编程语言（如 Java，JavaScript，C＃，C+ +，Python，PHP，Ruby 和 Objective-C）都支持面向对象编程（OOP）。

```js
//  arguments 是一个对应于传递给函数的参数的类数组对象
function sendMsg(msg, obj) {
  console.log(arguments[0], arguments[2]);
  console.log(arguments.length);
}
sendMsg('a'); // a undefined , 1

function send(mes, str) {
  if (arguments.length === 2) {
    //当调用函数有两个参数时执行
    console.log('参数是两个');
  } else {
    console.log(arguments.length);
  }
}

send('ad'); // 1

function displayError(msg) {
  // 检查确保msg不是undefinedif
  if (typeof msg == 'undefined') {
    msg = '缺少参数';
  }
  console.log(msg);
}
displayError('a'); // a
displayError(); //缺少参数

// 检查数字是否是一个字符串
let num = '12a';
if (typeof num == 'string') {
  num = parseInt(num);
}
console.log(num); // 12

// 用于延迟显示消息的通用函数
function delayAlert(msg, time) {
  setTimeout(() => {
    alert(msg);
  }, time);
}

delayAlert('hello', 2000);

// 一个设置其上下文的颜色风格函数
function chanegColor(color) {
  this.style.color = color;
}

let main = document.getElementById('main');
// 用call 方法改变它的颜色
changeColor.call(main, 'red'); // 将main对象的颜色改为红色

// 创建一个新的user构造函数
function user(name, age) {
  this.name = name;
  this.age = age;
}

//  在user的原型上添加getName()方法
user.prototype.getName = function() {
  return this.name;
};

// 在user的原型上添加getAge()方法
user.prototype.getAge = function() {
  return this.age;
};

// 实例化一个新的user对象
let person = new user('feiyu', 25);
person.getName(); // feiyu
person.getAge(); // 25
```

###何为原型链###

> 现在我们知道，实例对象的 `__proto__` 属性指向其对应的原型对象。
> 而在原型对象 `prototype` 上又有 `constructor` 和 `__proto__` 属性，
> 此时的 `__proto__` 又指向上级对应的原型对象，最终指向 `Object.prototype`，
> 而 `Object.prototype.__proto__ === null`。这就构成了原型链，而原型链最终都是指向 `null`。

###数组排序###

```js
let arr = [2, 5, 3, 6, 1];
arr.sort((a, b) => b - a); // [6, 5, 3, 2, 1];
arr.sort((a, b) => {
  // 箭头函数中有 {} 括号就必须要用 return 返回
  return a - b;
}); // [1, 2, 3, 5, 6];
```
