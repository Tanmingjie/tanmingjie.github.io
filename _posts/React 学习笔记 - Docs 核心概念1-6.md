---
layout: post
title: React学习笔记 - 官方文档核心概念1-6
subtitle: My Notes of React
author: oliver
header-img: img/react.jpg
catalog: true
tags:
    - React
---

[TOC]
# Hello World
最简易的React示例如下：
```
ReactDom.render(
    <h1>Hello, world!</h1>,
    document.getElementById('root')
)
```

# JSX 简介
```
const element = <h1>Hello, world!</h1>

const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;
```

在 JSX 语法中，你可以在大括号内放置任何有效的**JavaScript 表达式**。例如，2 + 2，user.firstName 或 formatName(user) 都是有效的 JavaScript 表达式。

## JSX 特定属性
你可以通过使用引号，来将属性值指定为字符串字面量：

```
const element = <div tabIndex="0"></div>;
```
也可以使用大括号，来在属性值中插入一个 JavaScript 表达式：
```
const element = <img src={user.avatarUrl}></img>;
```
在属性中嵌入 JavaScript 表达式时，不要在大括号外面加上引号。你应该**仅使用引号（对于字符串值）或大括号（对于表达式）中的一个**，对于同一属性不能同时使用这两种符号。

## JSX 防止注入攻击
你可以安全地在 JSX 当中插入用户输入内容：

```
const title = response.potentiallyMaliciousInput;
// 直接使用是安全的：
const element = <h1>{title}</h1>;
```

# 元素渲染

将一个元素渲染为Dom
```
//想要将一个 React 元素渲染到根 DOM 节点中，只需把它们一起传入 ReactDOM.render()：
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

## React 只更新它需要更新的部分
React DOM 会将元素和它的子元素与它们之前的状态进行比较，并只会进行必要的更新来使 DOM 达到预期的状态。

# 组件 & Props
**通过JavaScript定义组件**：
```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
该函数是一个有效的 React 组件，因为它
1. 接收唯一带有数据的 “props”（代表属性）对象
2. 返回一个 React 元素。

这类组件被称为“函数组件”，因为**它本质上就是 JavaScript 函数。**

**你同时还可以使用 ES6 的 class 来定义组件**：
```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

## Props的只读性

组件无论是使用函数声明还是通过 class 声明，都决不能修改自身的 props。来看下这个 sum 函数：
```
function sum(a, b) {
  return a + b;
}
```
这样的函数被称为“**纯函数**”，因为该函数不会尝试更改入参，且多次调用下相同的入参始终返回相同的结果。

相反，下面这个函数则不是纯函数，因为它更改了自己的入参：
```
function withdraw(account, amount) {
  account.total -= amount;
}
```
React 非常灵活，但它也有一个严格的**规则**：

**所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。**

当然，应用程序的 UI 是动态的，并会伴随着时间的推移而变化。在下一章节中，我们将介绍一种新的概念，称之为 “state”。在不违反上述规则的情况下，**state 允许 React 组件随用户操作、网络响应或者其他变化而动态更改输出内容。**

# State & 生命周期

## 将生命周期方法添加到 Class 中
在具有许多组件的应用程序中，当组件被销毁时释放所占用的资源是非常重要的。

当 Clock 组件第一次被渲染到 DOM 中的时候，就为其设置一个计时器。这在 React 中被称为“**挂载（mount）**”=> **componentDidMount**。 **componentDidMount() 方法会在组件已经被渲染到 DOM 中后运行。**

同时，当 DOM 中 Clock 组件被删除的时候，应该清除计时器。这在 React 中被称为“**卸载（unmount）**” => **componentWillUnmount**。

我们可以为 class 组件声明一些特殊的方法，当组件挂载或卸载时就会去执行这些方法：
```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  }

  componentWillUnmount() {

  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
## 正确地使用State
关于setState(), 需要注意三点：
### 不要直接修改State

例如，此代码不会重新渲染组件
```
// Wrong
this.state.comment = 'hello';
```
而是应该使用**setState()**:
```
// Correct
this.setState({comment: 'Hello'});
```
**构造函数是唯一可以给this.state赋值的地方**

### State的更新可能是异步的
出于性能考虑，React 可能会把多个 setState() 调用合并成一个调用。

**因为 this.props 和 this.state 可能会异步更新，所以你不要依赖他们的值来更新下一个状态。**

### State的更新会被合并
当你调用 setState() 的时候，React 会把你提供的对象合并到当前的 state。

例如，你的 state 包含几个独立的变量：
```javascript
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```
然后你可以分别调用 setState() 来单独地更新它们

## 数据是向下流动的
组件可以选择把它的 state 作为 props 向下传递到它的子组件中

# 事件处理
React 元素的事件处理和 DOM 元素的很相似，但是有一点语法上的不同:

- React 事件的命名采用**小驼峰式（camelCase）**，而不是纯小写。
- 使用 JSX 语法时你需要传入**一个函数作为事件处理函数**，而不是一个字符串。


在 React 中另一个不同点是你不能通过返回 false 的方式阻止默认行为。你必须显式的使用 preventDefault 。例如，传统的 HTML 中阻止链接默认打开一个新页面，你可以这样写：
```
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```
在 React 中，可能是这样的：
```
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

**你必须谨慎对待 JSX 回调函数中的 this**，在 JavaScript 中，class 的方法默认不会绑定 this。如果你忘记绑定 this.handleClick 并把它传入了 onClick，当你调用这个函数的时候 this 的值为 undefined。
```
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 为了在回调中使用 `this`，这个绑定是必不可少的
    this.handleClick = this.handleClick.bind(this);
  }
}
//或者下面语法也确保'handleClick'内的'this'已被绑定。
    return (
        <button onClick={(e) => this.handleClick(e)}>
        Click me
        </button>
    );
```
## 向事件处理程序传递参数
在循环中，通常我们会为事件处理函数传递额外的参数。例如，若 id 是你要删除那一行的 ID，以下两种方式都可以向事件处理函数传递参数：
```
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```
上述两种方式是等价的，分别通过**箭头函数**和 **Function.prototype.bind** 来实现。