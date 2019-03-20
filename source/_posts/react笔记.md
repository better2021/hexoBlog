---
title: react笔记
date: 2019-02-21 22:29:14
tags:
---

React 是一个为数据提供渲染为 HTML 视图的开源 JavaScript 库。React 视图通常采用包含以自定义 HTML 标记规定的其他组件的组件渲染。React 为程序员提供了一种子组件不能直接影响外层组件的模型，数据改变时对 HTML 文档的有效更新，和现代单页应用中组件之间干净的分离。

> 可以使用`jsx`中的点表示来引用`react`组件
> 例如：MyComponents.DatePiker 的组件，可以直接在 jsx 中使用它

```js
import React from 'react';
const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Image a {props.color} here</div>;
  }
};

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
```

**用户自定义组件的首字母必须大写**

```js
import React from 'react';

// 正确！组件名应该首字母大写:
function Hello(props) {
  // 正确！div 是有效的 HTML 标签:
  return <div>Hello {props.toWhat}</div>;
}

function HelloWorld() {
  // 正确！React 能够将大写开头的标签名认为是 React 组件。
  return <Hello toWhat="World" />;
}
```

> 运行时选择类型

```js
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // 正确！JSX 标签名可以为大写开头的变量。
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```

如果你已经有了个 props 对象，并且想在 JSX 中传递它，你可以使用 ... 作为“展开(spread)”操作符来传递整个属性对象。下面两个组件是等效的：

```js
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = { firstName: 'Ben', lastName: 'Hector' };
  return <Greeting {...props} />;
}
```

> 使用`PropTypes`进行类型检查

```js
import PropTypes from 'prop-types';
class Greeting extends React.Component {
  render() {
    return <h1>hello,{this.props.name}</h1>;
  }
}
Greeting.propTypes = {
  name: proptypes.string
};
```

_PropTypes 包含一整套验证器，可用于确保你接收的数据是有效的。在这个示例中，我们使用了 PropTypes.string。当你给属性传递了无效值时，JavsScript 控制台将会打印警告。出于性能原因，propTypes 只在开发模式下进行检查_

> 你可以通过配置 defaultProps 为 props 定义默认值：

```js
// 自执行函数
(function(x) {
  console.log(x); // 10
})(10);
```

```js
class Greeting extends React.Component {
  render() {
    return <h1>hello,{this.props.name}</h1>;
  }
}

// 为属性指定默认值
Greeting.defaultProps = {
  name: 'Stranger'
};

// 渲染 'hello,Stranger'
ReactDOM.render(<Greeting />, document.getElementById('example'));
```

> Refs 提供了一种方式，用于访问在 render 方法中创建的 DOM 节点或 React 元素

```js
class MyComponent extends React.Component {
  constructor(props) {
    surper(props);
    this.myRef = React.createRef();
  }
  rebder() {
    return <div ref={this.myRft} />;
  }
}
```

> redux 的使用

```js
// 定义reducer函数,名字可以随意起
function reducer(state,action) {
  /*初始化state和switch case*/
}

// 生成store
const store = createStore(reducer)

// 监听数据变化重新渲染页面
store.subscribe(()=>renderApp(store.getState())

// 获取state的值
store.getState()

// action只能用dispatch去触发更新
store.dispatch({type:'ADD'}) // 调用type为ADD的action

```
