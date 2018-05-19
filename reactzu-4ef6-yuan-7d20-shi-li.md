原文地址

[https://reactjs.org/blog/2015/12/18/react-components-elements-and-instances.html\#fn-1](https://reactjs.org/blog/2015/12/18/react-components-elements-and-instances.html#fn-1)

## Elements Describe the Tree {#elements-describe-the-tree}

**An element is a plain object **_**describing **_**a component instance or DOM node and its desired properties.**

An element is not an actual instance. Rather, it is a way to tell React what you \_want \_to see on the screen. It’s just an immutable description object with two fields:

type: \(string \| ReactClass\) and props: Object

### DOM Elements {#dom-elements}

```
{
  type: 'button',
  props: {
    className: 'button button-blue',
    children: {
      type: 'b',
      props: {
        children: 'OK!'
      }
    }
  }
}
```

This element is just a way to represent the following HTML as a plain object:

```
<button class='button button-blue'>
  <b>
    OK!
  </b>
</button>
```

### Component Elements {#component-elements}

**An element describing a component is also an element, just like an element describing the DOM node. They can be nested and mixed with each other.**

However, the`type `of an element can also be a function or a class corresponding to a React component:

```
{
  type: Button,
  props: {
    color: 'blue',
    children: 'OK!'
  }
}
```

This is the core idea of React.

**An element describing a component is also an element, just like an element describing the DOM node. They can be nested and mixed with each other.**

## Summary {#summary}

> **An **_**element **_**is a plain object describing what you want to appear on the screen in terms of the DOM nodes or other components. Elements can contain other elements in their props. Creating a React element is cheap. Once an element is created, it is never mutated.**

一个元素就是用来描述想要显示在屏幕上的DOM节点或其它组件的纯对象。



> **A **_**component **_**can be declared in several different ways. It can be a class with a `render()`method. Alternatively, in simple cases, it can be defined as a function. In either case, it takes props as an input, and returns an element tree as the output.**

组件有好几种定义方式，以props为输入，输出元素数



> **When a component receives some props as an input, it is because a particular parent component returned an element with its `type `and these props. This is why people say that the props flows one way in React: from parents to children.**

props总是由父组件传递到子组件



> **An **_**instance **_**is what you refer to as `this `in the component class you write. It is useful for **[**storing local state and reacting to the lifecycle events**](https://reactjs.org/docs/component-api.html)**.**

在class组件中的this就是实例，用来存储本地状态和对生命周期事件做出响应



> **Functional components don’t have instances at all. Class components have instances, but you never need to create a component instance directly—React takes care of this.**

用函数定义的组件没有实例。



> **Finally, to create elements, use**[**`React.createElement()`**](https://reactjs.org/docs/top-level-api.html#react.createelement)**,**[**JSX**](https://reactjs.org/docs/jsx-in-depth.html)**, or an **[**element factory helper**](https://reactjs.org/docs/top-level-api.html#react.createfactory)**. Don’t write elements as plain objects in the real code—just know that they are plain objects under the hood.**

创建元素的方法很多，不要直接在代码中用对象写元素。只需要知道背后的逻辑是这样的。





