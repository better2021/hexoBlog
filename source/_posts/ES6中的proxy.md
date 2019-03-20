---
title: "ES6中的proxy"
date: 2019-02-21 15:52:49
tags:
---

**ES6 原生提供 Proxy 构造函数，用来生成 Proxy 实例**

```js
let proxy = new Proxy(target, handler);
```

> Proxy 对象的所有用法，都是上面这种形式，不同的只是 handler 参数的写法。
> 其中，new Proxy() 表示生成一个 Proxy 实例，target 参数表示所要拦截的目标对象，
> handler 参数也是一个对象，用来定制拦截行为

```js
let proxy = new Proxy(
  {},
  {
    get: function(target, property) {
      return 35;
    }
  }
);
proxy.time; // 35
proxy.name; //35
proxy.title; //35
```

上面代码中，作为构造函数，Proxy 接受两个参数。第一个参数是所要代理的目标对象（上例是一个空对象），即如果没有 Proxy 的介入，操作原来要访问的就是这个对象；第二个参数是一个配置对象，对于每一个被代理的操作，需要提供一个对应的处理函数，该函数将拦截对应的操作。比如，上面代码中，配置对象有一个 get 方法，用来拦截对目标对象属性的访问请求。get 方法的两个参数分别是目标对象和所要访问的属性。可以看到，由于拦截函数总是返回 35，所以访问任何属性都得到 35。

> Proxy 用于修改某些操作的默认行为，等同于在语言层面做出的修改，
> 所以属于一种"元编程", 即对编程语言进行编程

> Proxy 可以理解为,在目标对象之前架设一层"拦截"，外界对该对象的访问，
> 都是必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。
> Proxy 这个词的原意是代理，用在这里表示由它来‘代理’某些操作。

```js
let obj = new Proxy(
  {},
  {
    get: function(target, key, receiver) {
      console.log(`getting ${key}`);
      return Reflect.get(target, key, receiver);
    },
    set: function(target, key, value, receiver) {
      console.log(`setting ${key}`);
      return Reflect.set(target, key, value, receiver);
    }
  }
);

obj.count = 1;
// setting count
```

**如果 handler 没有设置任何拦截，那就等同于直接通向原对象。**

```js
let target = {};
let handler = {};
let proxy = new Proxy(target, handler);
proxy.a = 'b';
target.a; // 'b'
```

> 上面代码中，`handler`是一个空对象，没有任何拦截效果，访问`proxy`
> 就是等同访问`target`

一个技巧是将 Proxy 对象，设置到 object.proxy 属性，
就可以再 object 对象上调用

```js
const object = { proxy: new Proxy(target, handler) };
```

> Proxy 实例也可以作为其他对象的原型对象，obj 对象本身并么有 time 属性，
> 所以根据原型链，会在 proxy 对象上读取该属性，导致被拦截

`get(target,key,receiver)` => 拦截对象属性的读取，比如 proxy.foo 和 proxy['foo']
`set(target,key,receiver)` => 拦截对象属性的设置，比如 proxy.foo = v 或 proxy['foo'] = v 返回一个布尔值
`has(target,key,receiver)` => 拦截`key in proxy`的操作，返回一个布尔值

> get 方法用于拦截某个属性的读取操作，可以接受三个参数，依次为目标对象、属性名和 proxy 实例本身，
> 其中最后一个参数可选

```js
var person = {
  name: '张三'
};

var proxy = new Proxy(person, {
  get: function(target, property) {
    if (property in target) {
      return target[property];
    } else {
      throw new ReferenceError('Property "' + property + '" does not exist.');
    }
  }
});

proxy.name; // "张三"
proxy.age; // 抛出一个错误
```

**ES6 中的 export 和 export default**

> `export {a,b,c}`可以连续导出多个模块
> `export default a`只能导出一个默认模块

```js
// export 导出模块
let obj = {
  getName() {
    console.log(this.name);
  },
  name: 'test export'
};

export { obj };

import { obj as app } from '../../libs/test.js'; // 引入模块
```

```js
// 导出默认模块
export default {
  getName() {
    console.log(this.name);
  },
  name: 'test export'
};

// 或者
// let obj = {
//   getName() {
//     console.log(this.name);
//   },
//   name: 'test export'
// };
// export default obj;

import app from '../../libs/test.js'; // 引入默认模块（默认导入模块时的导入名称，例如app，可以自己随意取名）
```
