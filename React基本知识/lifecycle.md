## Adding Lifecycle Methods to a Class {#adding-lifecycle-methods-to-a-class}

[https://medium.com/@baphemot/understanding-reactjs-component-life-cycle-823a640b3e8d](https://medium.com/@baphemot/understanding-reactjs-component-life-cycle-823a640b3e8d "from medium")

官方文档中的解释

In applications with many components, it’s very important to free up resources taken by the components when they are destroyed.

We want to[set up a timer](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval)whenever the`Clock`is rendered to the DOM for the first time. This is called “mounting” in React.

We also want to[clear that timer](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/clearInterval)whenever the DOM produced by the`Clock`is removed. This is called “unmounting” in React.

We can declare special methods on the component class to run some code when a component mounts and unmounts:

![](/assets/Screenshot %2816%29.png)

1. ## componentDidMount

这个方法在整个生命周只调用一次

This function will be called only once in the whole life-cycle of a given component and it being called signalizes that the component — and all its sub-components — rendered properly.

Since this function is guaranteed to be called only once it is a perfect candidate for performing any side-effect causing operations such as AJAX requests

## 



