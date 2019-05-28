---
title: TypeScript学习
date: 2019-05-27 20:10
tags:
---

> TypeScript 是 javaScript 的超集，给 javaScript 添加了静态类型，增强了语言的健壮性

### 类型注释

```js
fuction greeter(person:string){
  return 'Hello' + person
}

let user = 'Jane User'
document.body.innerHTML = greeter(user)
```

### 接口

> 在 TypeScript 里，只在两个类型内部的结构兼容那么这两个类型就是兼容的。 这就允许我们在实现接口时候只要保证包含了接口要求的结构就可以，而不必明确地使用 implements 语句。

```js
interface Person {
  firstName: string;
  lastName: string;
}

function greeter(person: Person) {
  return "Hello," + person.firstName + person.lastName
}

let user = { firstName: "Jane", lastName: "User" }
document.body.innerHTML = greeter(user)
```

### 类

> 让我们创建一个 Student 类，它带有一个构造函数和一些公共字段。 注意类和接口可以一起共作，程序员可以自行决定抽象的级别。
> 还要注意的是，在构造函数的参数上使用 public 等同于创建了同名的成员变量。

```js
class Student{
  fullName:string;
  constructor(public firstName,public middleInitial,public lastName){
    this.fullName = firstName + ' ' + middleINitial + lastName
  }
}

interface Person{
  firstName:string;
  lastName:string
}

function greeter(person:Person){
  return 'Hello ' + person.fristName + ' ' + person.lastName
}

let user = new Student('Jane','M','User')
document.body.inderHTML = greeter(user)
```

## 基础数据类型

### 布尔值

> 最基本的数据类型就是简单的 true/false 值，在 javaScript 和 TypeScript 里叫做 boolean

```js
let isDone: boolean = false
```

### 数字

> 和 JavaScript 一样，TypeScript 里的所有数字都是浮点数

```js
let decLiteral: number = 6
let hexLiteral: number = 0xf00
```

### 字符串

> 像其它语言里一样，我们使用 string 表示文本数据类型。 和 JavaScript 一样，可以使用双引号（ "）或单引号（'）表示字符串。

```js
let name: string = "Jone"
name = "nike"
let age: number = 20`hello,my name is ${name} , i am ${age}`
```

### 元组

> 元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同

```js
let x: [string, number]
x = ["hello", 10] // ok
x = [10, "hello"] // error
```

### 枚举

> `enum`类型是对 JavaScript 标准数据类型的一个补充。 像 C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

```js
enum Color {Red,Green,Blue}
let c:Color = Color.Green
// 默认情况下，从0开始为元素编号。 你也可以手动的指定成员的数值。 例如，我们将上面的例子改成从 1开始编号：
// 枚举类型提供的一个便利是你可以由枚举的值得到它的名字。 例如，我们知道数值为2，但是不确定它映射到Color里的哪个名字，我们可以查找相应的名字
enum Color {Red=1,Green,Blue}
let colorName:string = Color[2]
console.log(colorName)  // // 显示'Green'因为上面代码里它的值是2
```

### Any

> 有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。
> 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 any 类型来标记这些变量

```js
let nitSure: any = 4
notSure = "maybe a string"
notSure = false
// 当你只知道一部分数据的类型时，any类型也是有用的。 比如，你有一个数组，它包含了不同的类型的数据：
let list: any[] = [1, true, "hello"]
list[1] = 10
```

### void

> 某种程度上来说，void 类型像是与 any 相反，它表示没有任何类型。当一个函数没有返回值时，你通常会见到返回值类型是 void

```js
function warnUser(): void {
  console.log("This is warning message")
}
// 声明一个void类型的变量没有什么大用，因为你只能为它赋予undefined和null：
let unusable: void = undefined
```

### Null 和 Undefined

> Typescript 里，`undefined` 和 `null` 两者各自有自己的类型分别叫做`undefined` 和 `null`

```js
let u: undefined = undefined
let n: null = null
// 也许在某处你想传入一个 string或null或undefined，你可以使用联合类型string | null | undefined。 再次说明，稍后我们会介绍联合类型。
let adc: string | null | undefined = "yaha"
```

### Never

> never 类型表示的是那些永不存在的值的类型。
> 例如， never 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型；
> 变量也可能是 never 类型，当它们被永不为真的类型保护所约束时。

```js
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
  throw new Error(message)
}

// 推断的返回值类型为never
function fail() {
  return error("Something failed")
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
  while (true) {}
}
```

### Object

> `object` 表示非原始类型，也就是除`number`,`string`,`boolean`,`symbol`,`null`或`undefined` 之外的类型
> 使用`object`类型，就可以更好的表示像`Object.create`这样的 API

```js
declare function create(o: object | null): void

create({ prop: 0 }) // OK
create(null) // OK

create(42) // Error
create("string") // Error
create(false) // Error
create(undefined) // Error
```

### 类型断言

> 有时候你会遇到这样的情况，你会比 TypeScript 更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。
> 通过类型断言这种方式可以告诉编译器，“相信我，我知道自己在干什么”。
> 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript 会假设你，程序员，已经进行了必须的检查。

```js
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```
