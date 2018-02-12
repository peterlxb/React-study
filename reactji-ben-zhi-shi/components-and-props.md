# Components and Props

React are all about creating components.在react 中,component 类似于function

最简单定义组件的方式就是定义一个函数

![](/assets/Screenshot %285%29.png)

> **This function is a valid React component because it accepts a single “props” \(which stands for properties\) object argument with data and returns a React element. We call such components “functional” because they are literally JavaScript functions.**
>
> You can also use an[ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) to define a component:

![](/assets/Screenshot %286%29.png)

## Props are Read-Only {#props-are-read-only}

来自官方文档

> **Whether you declare a component **[**as a function or a class**](https://reactjs.org/docs/components-and-props.html#functional-and-class-components)** , it must never modify its own props.**

纯函数

![](/assets/Screenshot %287%29.png)

Such functions are called [“pure”](https://en.wikipedia.org/wiki/Pure_function) because they do not attempt to change their inputs, and always return the same result for the same inputs.

来自官方文档中的描述

React is pretty flexible but it has a single strict rule:

> **All React components must act like pure functions with respect to their props.**

Of course, application UIs are dynamic and change over time. A new concept of “state”. State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.

