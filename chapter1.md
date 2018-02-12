# Getting start

React的三个特点

```
 1.Composition model 组件模型

 2.Declarative nature 声明式特点

 3.Unidirectional Data Flow 单向数据
```

## 一. Composition model

```
 官方文档中的说明，
```

> Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.

```
组件在react中其实就是函数，它们就收**props** 然后返回**React elements**
```

Udacity react couse

> Instead of composing functions to get some value,
>
> React can composing functions to get some UI

一段示例代码  
`<Page>`

`<Article />`

`<SideBar />`

`</Page>`

## 二: Declarative Code

命令式代码\(Imperative code\)

示例：

![](/assets/Screenshot.png)

在命令式代码中，我们是在命令JavaScript执行每一步骤

同样的代码用声明式

![](/assets/Screenshot %281%29.png)

在命令式代码中我们其实在告诉JavaScript它该如何执行每一步

使用Declarative code，我们告诉JavaScript我们想要达成的效果，而让JavaScript自己区处理步骤。

## 三.Unidirectional Data Flow 单向数据流

> In React , the data flows from the parent component to a child component

效果图如下

![](/assets/Screenshot %282%29.png)

Data flows down from parent component to child component ,Data updates are sent to the parent component

where the parent perform the actual change.

Data lives in the parent component and is passed down to the child component.Both parent and child component can use

the data

If the child component needs to make a change to the data, then it would send the updated data to the parent component where the change will actually be made.

























