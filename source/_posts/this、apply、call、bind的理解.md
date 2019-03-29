---
title: this、apply、call、bind的理解
date: 2019-03-29 10:40
tags:
---

### this的指向
在ES5中，this的指向，始终坚持一个原则：***this永远指向最后调用它的那个对象***

```js
 var name = "windowsName";

    function fn() {
        var name = 'Cherry';
        innerFunction();
        function innerFunction() {
            console.log(this.name);      // windowsName
        }
    }

    fn()
```
> `fn()` 最后仍然是被 `window` 调用的。所以 `this` 指向的也就是 `window`

### 箭头函数
在ES6中箭头函数是可以避免ES5中this的坑，***箭头函数的this始终指向函数定义的this，而非执行***

```js
let name = 'windowName'
let a = {
  name:'cherry',
  fun1:function(){
    console.log(this.name)
  },
  fun2:function(){
    setTimeout(()=>{
      this.fun1()
    },1000)
  }
}

a.fun2()  // cherry
```

### 使用 apply

```js
let name = 'windowName'
let a = {
  name:'cherry',
  fun1:function(){
    console.log(this.name)
  },
  fun2:function(){
    setTimeout(function(){
      this.fun1()
    }.apply(a),1000) // 本来setTimeout中的this应该是指向window，但使用apply后可以让this指向对象a
  }
}

a.fun2() // cherry
```

### 使用call

```js
let name = 'windowName'
let a = {
  name:'cherry',
  fun1:function(){
    console.log(this.name)
  },
  fun2:function(){
    setTimeout(function(){
      this.fun1()
    }.call(a),1000) // 本来setTimeout中的this应该是指向window，但使用call后也可以让this指向对象a
  }
}

a.fun2() // cherry
```

> `apply`与`call`的区别，只是传入参数的不同
> fun.call(obj,param1,param2,param3)
> fun.apply(obj,[param1,param2,param3])

```js
var a = {
  name:'hello',
  fn:function(a,b){
    console.log(a+b)
  }
}

var b = a.fn
b.apply(a,[1,2]) // 3
``` 

```js
var a = {
  name:'hello',
  fn:function(a,b){
    console.log(a+b)
  }
}

var b = a.fn
b.call(a,1,2) // 3
```

> bind()方法创建一个新的函数, 当被调用时，将其this关键字设置为提供的值，在调用新函数时，在任何提供之前提供一个给定的参数序列。

```js
// 所以我们可以看出，bind 是创建一个新的函数，我们必须要手动去调用
var a = {
  name:'hello',
  fn:function(a,b){
    console.log(a+b)
  }
}

var b = a.fn
b.bind(a,1,2)() // 3
```

>  匿名函数的`this`永远指向`window`


```js
let obj = { name : 'xiaoming' }
 
function fun(age,sex){
  console.log(this.name,age,sex)
} 
fun.call(obj,15,'女生') // xiaoming 15 女生
fun.apply(obj,[18,'女生']) // xiaoming 18 女生

```