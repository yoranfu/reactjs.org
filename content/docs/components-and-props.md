---
id: components-and-props
title: Components and Props
permalink: docs/components-and-props.html
redirect_from:
  - "docs/reusable-components.html"
  - "docs/reusable-components-zh-CN.html"
  - "docs/transferring-props.html"
  - "docs/transferring-props-it-IT.html"
  - "docs/transferring-props-ja-JP.html"
  - "docs/transferring-props-ko-KR.html"
  - "docs/transferring-props-zh-CN.html"
  - "tips/props-in-getInitialState-as-anti-pattern.html"
  - "tips/communicate-between-components.html"
prev: rendering-elements.html
next: state-and-lifecycle.html
---

Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.组件可以让你将UI分割成独立的可复用的部分，孤立思考每个部分。 This page provides an introduction to the idea of components. You can find a [detailed component API reference here](/docs/react-component.html).

Conceptually, components are like JavaScript functions.组件概念上与JavaScript函数类似。 They accept arbitrary inputs (called "props") and return React elements describing what should appear on the screen.它们接受任意输入（称为“props”）并返回React组件，描述应该呈现在屏幕上的东西。

## Function and Class Components

The simplest way to define a component is to write a JavaScript function:最简单的定义组件的方式是写个函数

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

This function is a valid React component because it accepts a single "props" (which stands for properties) object argument with data and returns a React element. We call such components "function components" because they are literally JavaScript functions.此函数是一个合法的React组件，因为它接受一个props对象参数，返回一个React元素。我们称这种组件为“函数组件”，因为它们是字面上的JavaScript函数。

You can also use an [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) to define a component:你也可以使用es6的class来定义组件:

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

The above two components are equivalent from React's point of view.以上两个组件在React的看来是等价的。

Classes have some additional features that we will discuss in the [next sections](/docs/state-and-lifecycle.html). Until then, we will use function components for their conciseness.Class有一些额外的特性，我们将在下一章讨论。

## Rendering a Component

Previously, we only encountered React elements that represent DOM tags:前面我们只遇到表示DOM标签的React组件：

```js
const element = <div />;
```

However, elements can also represent user-defined components:然而，元素也可以表示用户自定义的组件：

```js
const element = <Welcome name="Sara" />;
```

When React sees an element representing a user-defined component, it passes JSX attributes to this component as a single object. We call this object "props".当React遇到代表用户自定义组件时，会将JSX属性作为一个对象传递给这个组件。我们称这个对象为“props”。

For example, this code renders "Hello, Sara" on the page:

```js{1,5}
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[](codepen://components-and-props/rendering-a-component)

Let's recap what happens in this example:我们概括一下这个示例的过程：

1. We call `ReactDOM.render()` with the `<Welcome name="Sara" />` element.
2. React calls the `Welcome` component with `{name: 'Sara'}` as the props.
3. Our `Welcome` component returns a `<h1>Hello, Sara</h1>` element as the result.
4. React DOM efficiently updates the DOM to match `<h1>Hello, Sara</h1>`.

>**Note:** Always start component names with a capital letter.组件名总是以大写字母开头
>
>React treats components starting with lowercase letters as DOM tags. For example, `<div />` represents an HTML div tag, but `<Welcome />` represents a component and requires `Welcome` to be in scope.React将小写字母开头的组件视为DOM标签。例如，`<div />` 表示一个HTML div标签，而`<Welcome />`表示一个组件，并且需要作用域中存在`Welcome`。
>
>You can read more about the reasoning behind this convention [here.](/docs/jsx-in-depth.html#user-defined-components-must-be-capitalized)

## Composing Components

Components can refer to other components in their output. This lets us use the same component abstraction for any level of detail. A button, a form, a dialog, a screen: in React apps, all those are commonly expressed as components.

For example, we can create an `App` component that renders `Welcome` many times:

```js{8-10}
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[](codepen://components-and-props/composing-components)

Typically, new React apps have a single `App` component at the very top. However, if you integrate React into an existing app, you might start bottom-up with a small component like `Button` and gradually work your way to the top of the view hierarchy.通常React应用在最顶部有一个`App`组件。然而，如果你将React嵌入已有的应用，你可能自底向上从类似`Button`这样的一个小组件开始，逐渐向上到顶层视图结构。

## Extracting Components

Don't be afraid to split components into smaller components.

For example, consider this `Comment` component:

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[](codepen://components-and-props/extracting-components)

It accepts `author` (an object), `text` (a string), and `date` (a date) as props, and describes a comment on a social media website.

This component can be tricky to change because of all the nesting, and it is also hard to reuse individual parts of it. Let's extract a few components from it.

First, we will extract `Avatar`:

```js{3-6}
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

The `Avatar` doesn't need to know that it is being rendered inside a `Comment`. This is why we have given its prop a more generic name: `user` rather than `author`.

We recommend naming props from the component's own point of view rather than the context in which it is being used.

We can now simplify `Comment` a tiny bit:

```js{5}
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

Next, we will extract a `UserInfo` component that renders an `Avatar` next to the user's name:

```js{3-8}
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

This lets us simplify `Comment` even further:

```js{4}
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[](codepen://components-and-props/extracting-components-continued)

Extracting components might seem like grunt work at first, but having a palette of reusable components pays off in larger apps. A good rule of thumb is that if a part of your UI is used several times (`Button`, `Panel`, `Avatar`), or is complex enough on its own (`App`, `FeedStory`, `Comment`), it is a good candidate to be a reusable component.有一个实用的经验方法，如果你的UI的某部分会多次用到，或者太过复杂，把它抽象成一个可复用的组件是一个比较好的选择。

## Props are Read-Only

Whether you declare a component [as a function or a class](#function-and-class-components), it must never modify its own props. 无论是使用函数还是类来声明组件，决不能修改自己的props。Consider this `sum` function:

```js
function sum(a, b) {
  return a + b;
}
```

Such functions are called ["pure"](https://en.wikipedia.org/wiki/Pure_function) because they do not attempt to change their inputs, and always return the same result for the same inputs.类似的函数称为纯函数，它们并没有改变自己的输入值，对于相同的输入总是返回相同的结果。

In contrast, this function is impure because it changes its own input:

```js
function withdraw(account, amount) {
  account.total -= amount;
}
```

React is pretty flexible but it has a single strict rule:React是非常灵活的，但它也有一个严格的规则：

**All React components must act like pure functions with respect to their props.**所有的React组件必须像纯函数那样对待它们的props

Of course, application UIs are dynamic and change over time. In the [next section](/docs/state-and-lifecycle.html), we will introduce a new concept of "state". State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.当然，应用界面是随时间动态变化的。我们将在下一章介绍一个称为state的新概念。state可以在不违法上述规则的情况下，根据用户操作、网络响应或者其它动态改变组件的输出。
