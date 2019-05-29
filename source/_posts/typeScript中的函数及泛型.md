---
title: typeScript的函数及泛型
date: 2019-05-29 10:10
tags:
---

> 函数是`JavaScript`应用程序的基础。 它帮助你实现抽象层，模拟类，信息隐藏和模块。 在`TypeScript`里，虽然已经支持类，命名空间和模块，但函数仍然是主要的定义 行为的地方。 `TypeScript`为`JavaScript`函数添加了额外的功能，让我们可以更容易地使用。

### typescript 为函数定义类型

> 我们可以给每个参数添加类型之后再为函数本身添加返回值类型，TypeScript 能够根据返回语句自动推断出返回值类型，因此我们通常省略它。

```js
function add(x: number, y: number): number {
  return x + y
}

let myAdd = function(x: number, y: number): number {
  return x + y
}
```
