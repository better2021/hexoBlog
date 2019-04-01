---
title: TypeScript基础类型
date: 2019-03-29 20:10
tags:
---
 

### 基础类型

#### 布尔值
```js
let isDone:boolean = false
```

#### 数字
和JavaScript一样，TypeScript里的所有数字都是浮点数。 这些浮点数的类型是 number。 除了支持十进制和十六进制、八进制等字面量

```js
let dec:number = 6
let hex:number = 0xf00d
let binary:number = 0b1010
let oct:number = 0o744
```

#### 字符串
```js
let name:string = 'bob'
name = 'nike'
let senten:string = `Hello, my name is ${ name }`
```
#### 数组
TypeScript像JavaScript一样可以操作数组元素。 有两种方式可以定义数组。 第一种，可以在元素类型后面接上 []，表示由此类型元素组成的一个数组：

```js
let list:number[] = [1,2,3] // 第一种方式
let list:Array<number> = [1,2,3] // 第二种数组泛型，Array<元素类型>
```

#### 元组
元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为 string和number类型的元组

```js
let x:[string,number]
x = ['hello',10] // OK
x = [10,'hello'] // Error
```

#### 枚举
`enum`类型是对JavaScript标准数据类型的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

```js
enum Color {red = 1,green = 3, blue}
let c:Color = Color.green
```

#### Any
有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。
这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。  
这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。   
那么我们可以使用 any类型来标记这些变量：

```js
let notSure:any = 4
notSure = 'maybe is a string'
notSure = false
```

在对现有代码进行改写的时候，any类型是十分有用的，它允许你在编译时可选择地包含或移除类型检查。   
你可能认为 Object有相似的作用，就像它在其它语言中那样。   
但是 Object类型的变量只是允许你给它赋任意值 - 但是却不能够在它上面调用任意的方法，即便它真的有这些方法

```js
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
```

当你只知道一部分数据的类型时，any类型也是有用的。 比如，你有一个数组，它包含了不同的类型的数据：

```js
let list:any[] = [1,true,'str']
list[1] = 100
```

#### Void
某种程度上来说，void类型像是与any类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 void：

```js
function warnUser():void{
  console.log('this is my warning message')
}
let umable:viod = undefined
```

#### Null 和 Undefined
TypeScript里，undefined和null两者各自有自己的类型分别叫做undefined和null。 和 void相似，它们的本身的类型用处不是很大：

```js
let u:undefined = undefined
let n:null = null
```
> 默认情况下null和undefined是所有类型的子类型。 就是说你可以把 null和undefined赋值给number类型的变量。

#### Never
never类型表示的是那些永不存在的值的类型。 
例如， never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 never类型，当它们被永不为真的类型保护所约束时。

never类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外）。 即使 any也不可以赋值给never

```js
// 返回nerver的函数必须存在无法达到的终点
function error(message:string):nerver{
  throw new Error(message)
}

// 推断的返回值类型为never
function fail(){
  return error('something failed')
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(){
  while(true){...}
}
```

### Object
object表示非原始类型，也就是除number，string，boolean，symbol，null或undefined之外的类型。

使用object类型，就可以更好的表示像Object.create这样的API。例如：

```js
declare function create(o:object|null):void{
  ...
}
create(42) // Error
```

> 类型断言有两种形式。 其一是“尖括号”语法：

```js
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```