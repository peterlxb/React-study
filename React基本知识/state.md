# State

**Local state : a feature available only to classes.**

官方文档是一个关于tick 的示例

![](/assets/Screenshot %288%29.png)

上图示例只是显示一次时间，要想时间动态显示，示例中添加了Lifecycle hooks

## Using State Correctly {#using-state-correctly}

如何正确更新local state

There are three things you should know about`setState()`.

* ### Do Not Modify State Directly {#do-not-modify-state-directly}

  For example, this will not re-render a component:

![](/assets/Screenshot %289%29.png)

Instead, use `setState()`:

![](/assets/Screenshot %2810%29.png)

文档中的note

> **The only place where you can assign**`this.state`**is the constructor.**

* ### State Updates May Be Asynchronous {#state-updates-may-be-asynchronous}

  Because`this.props`and`this.state`may be updated asynchronously, you should not rely on their values for calculating the next state.

For example, this code may fail to update the counter:

![](/assets/Screenshot %2811%29.png)

To fix it, use a second form of

`setState()`

that accepts a function rather than an object. That function will receive the previous state as the first argument, and the props at the time the update is applied as the second argument:

![](/assets/Screenshot %2812%29.png)

* ### State Updates are Merged {#state-updates-are-merged}

来自官方文档的示例

![](/assets/Screenshot %2814%29.png)

![](/assets/Screenshot %2813%29.png)

The merging is shallow, so `this.setState({comments})`leaves `this.state.posts`intact, but completely replaces`this.state.comments`

# The Data Flows Down.

来自文档

> Neither parent nor child components can know if a certain component is stateful or stateless, and they shouldn’t care whether it is defined as a function or a class.
>
> This is why state is often called local or encapsulated. It is not accessible to any component other than the one that owns and sets it.

A component may choose to pass its state down as props to its child components:

> ![](/assets/Screenshot %2815%29.png)  
> **This is commonly called a “top-down” or “unidirectional” data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components “below” them in the tree.**



