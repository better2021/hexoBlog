---
title: typeScript的变量、接口及类
date: 2019-05-28 16:10
tags:
---

> `let` 和 `const` 是 `JavaScript` 里相对较新的变量声明方式。 像我们之前提到过的， `let` 在很多方面与 `var` 是相似的，但是可以帮助大家避免在 `JavaScript` 里常见一些问题。
> `const` 是对 `let` 的一个增强，它能阻止对一个变量再次赋值。

**_当用 let 声明一个变量，它使用的是词法作用域或块作用域。 不同于使用 var 声明的变量那样可以在包含它们的函数外访问，块作用域变量在包含它们的块或 for 循环之外是不能访问的_**

> 默认值可以让你在属性为 undefined 时使用缺省值

```js
function keepObject(param: { a: string, b?: number }) {
  // b为undefined时的缺省值，有b或没有b都不会报错
  let { a, b = 100 } = param
}
```

> 解构有嗯呢该用于函数声明

```js
type C = { a: string, b?: number }
function f({ a, b }: C): void {
  // ...
}
```

> TypeScript 的核心原则之一是对值所具有的结构进行类型检查。 它有时被称做“鸭式辨型法”或“结构性子类型化”。
> 在 TypeScript 里，接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约

```js
function printLabel(labelObj: { lable: string }) {
  console.log(labelObj.label)
}
// 类型检查器会查看printLabel的调用。 printLabel有一个参数，并要求这个对象参数有一个名为label类型为string的属性。
// 需要注意的是，我们传入的对象参数实际上会包含很多属性，但是编译器只会检查那些必需的属性是否存在，并且其类型是否匹配
let myObj = { size: 10, label: "size 10 object" }
printLabel(myObj)

interface SquareConfig {
  color?: string;
  width?: number;
}

function createSquare(config: SquareConfig): { color: string, area: number } {
  let newSquare = { color: "white", area: 100 }
  if (config.color) {
    newSquare.color = config.color
  }
  if (config.width) {
    newSquare.area = config.width * config.width
  }
  return newSquare
}

let mySquare = createSquare({ color: "black" })
```

**带有可选属性的接口与普通的接口定义差不多，只是在可选属性名字定义的后面加一个?符号。**

### 只读属性

> 一些对象属性只能在对象刚刚创建的时候修改其值，你可以再属性名前用`readonly`来指定只读属性

```js
interface Point{
  readonly x:number;
  readonly y:number;
}

let p1:Point ={x:10,y:20}
console.log(o1.x) // 10
p1.x =5 // error
```

### 函数类型

> 为了使用接口表示函数类型，我们需要给接口定义一个调用签名。 它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。

```js
interface SearchFunc {
  (source: string, substring: string): boolean;
}
// 这样定义后，我们可以像使用其它接口一样使用这个函数类型的接口。 下例展示了如何创建一个函数类型的变量，并将一个同类型的函数赋值给这个变量。
let mySearch: SearchFunc
mySearch = function(source: string, substring: string) {
  let result = source.search(substring)
  return resiltr > -1
}
```

### 公共，私有与受保护的修饰符

> 默认为`public`

```js
class Animal{
  public name:string
  public constructor(theName:string){
    this.name = theName
  }
  piblic move(distanceInMeters:number){
    console.log(`${this.name} moved`)
  }
}
```

### 理解 private

> 当成员被标记`private`时，它就不能再声明它的类的外部访问

```js
class Animal{
  private nane:string
  constructor(theName:string){
    this.name = theName
  }
}

new Animal('Cat').name // 错误: 'name' 是私有的.
```
