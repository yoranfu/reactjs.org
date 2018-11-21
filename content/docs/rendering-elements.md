---
id: rendering-elements
title: Rendering Elements
permalink: docs/rendering-elements.html
redirect_from:
  - "docs/displaying-data.html"
prev: introducing-jsx.html
next: components-and-props.html
---

Elements are the smallest building blocks of React apps.

An element describes what you want to see on the screen:元素描述了你在屏幕上所看到的内容：

```js
const element = <h1>Hello, world</h1>;
```

Unlike browser DOM elements, React elements are plain objects, and are cheap to create. 与浏览器DOM元素不同，React元素是普通的对象，易于创建。React DOM takes care of updating the DOM to match the React elements.React DOM接管更新DOM匹配React元素。

>**Note:**
>
>One might confuse elements with a more widely known concept of "components".可能会混淆组件和元素的概念。 We will introduce components in the [next section](/docs/components-and-props.html). Elements are what components are "made of", and we encourage you to read this section before jumping ahead.组件是由元素组成

## Rendering an Element into the DOM

Let's say there is a `<div>` somewhere in your HTML file:例如在HTML某处有个`<div>`元素

```html
<div id="root"></div>
```

We call this a "root" DOM node because everything inside it will be managed by React DOM.我们称之为"root"DOM节点，在它内部的一切都将被React DOM管理。

Applications built with just React usually have a single root DOM node. 仅使用React构建的应用通常都有一个root DOM节点。If you are integrating React into an existing app, you may have as many isolated root DOM nodes as you like.如果你将React集成到已有的应用中，你可以有任意多个独立的root DOM节点。

To render a React element into a root DOM node, pass both to `ReactDOM.render()`:要渲染一个React元素到root DOM节点内，将它们传入`ReactDOM.render()`

`embed:rendering-elements/render-an-element.js`

[](codepen://rendering-elements/render-an-element)

It displays "Hello, world" on the page.

## Updating the Rendered Element

React elements are [immutable](https://en.wikipedia.org/wiki/Immutable_object). Once you create an element, you can't change its children or attributes. An element is like a single frame in a movie: it represents the UI at a certain point in time.React元素是不可变的。一旦创建，无法改变子元素或属性。元素就像电影里的一帧：它表示某个特定时间节点的UI。

With our knowledge so far, the only way to update the UI is to create a new element, and pass it to `ReactDOM.render()`.就我们目前所知，更新UI的唯一方法就是创建一个新的元素，然后传入`ReactDOM.render()`。

Consider this ticking clock example:

`embed:rendering-elements/update-rendered-element.js`

[](codepen://rendering-elements/update-rendered-element)

It calls `ReactDOM.render()` every second from a [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) callback.

>**Note:**
>
>In practice, most React apps only call `ReactDOM.render()` once. In the next sections we will learn how such code gets encapsulated into [stateful components](/docs/state-and-lifecycle.html).实际中，大多数的React应用只调用一次`ReactDOM.render()`。
>
>We recommend that you don't skip topics because they build on each other.

## React Only Updates What's Necessary React只更新必要的部分

React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.React DOM比较前后的元素及其子元素，只涉及必要的DOM更新达到预期的状态。

You can verify by inspecting the [last example](codepen://rendering-elements/update-rendered-element) with the browser tools:

![DOM inspector showing granular updates](../images/docs/granular-dom-updates.gif)

Even though we create an element describing the whole UI tree on every tick, only the text node whose contents has changed gets updated by React DOM.虽然我们每秒都创建了描述整个UI树的元素，但是React DOM只更新了发生改变的文本节点的内容。

In our experience, thinking about how the UI should look at any given moment rather than how to change it over time eliminates a whole class of bugs.依我们的经验，思考任意时刻UI应该怎样而不是怎么改变它可以消除很大一类的错误。
