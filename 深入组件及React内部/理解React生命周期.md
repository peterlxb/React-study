## 理解React lifecycle

生命周期主要描述组件在运行过程中不同阶段会执行的方法。

有各种各样的原因会导致一个组件重新渲染。在每种情况下不同的函数会被调用来允许开发者更新组件中特定的部分。

### 第一种情况，组件创建时:

![](/assets/lifecycle-1.png)

constructor\(\)方法在每次创建一个新对象时会被执行一遍。在这个方法里面一般保存初始state.不应该在这里面执行网络请求。

在第一行应该加上super\(props\); 有一段定义

> Calling this special function will call the constructor of our parent class and allow it to initialize itself. This is why we have access to `this.props`only after we’ve initially called `super`.

constructor\(\)之后是componentWillMount\(\).

这个方法一般不使用。同样不应该在这个方法内执行http请求之类的。在里面调用setState\(\)不会有副作用。

然后就是执行render\(\)方法。

执行render\(\)并不意味着会马上接触到真正的DOM，这里只是告诉React要渲染什么内容。

调用完render\(\)就执行组件下的子组件。

然后就调用componentDidMount\(\)方法。

这个方法只运行一次。是处理http请求\(ajax\)最合适的地方。

在componentDidMount\(\)中调用会setState\(\)导致重新渲染。

### 第二种父组件更新导致的重新渲染:

![](/assets/lifecycle-2.png)

首先调用componentWillReceiveProps\(nextProps\)方法。这个方法内能做的操作是同步state 和props。

调用完这个方法执行shouldComponentUpdate\(nextProps, nextState\),能做的操作是同步state 和props。

返回true或false。默认返回true,就是更新继续。如果返回false更新暂停。

shouldComponentUpdate\(\)返回true则继续执行componentWillUpdate\(nextProps, nextState\)。

componentWillUpdate\(\)这里是最适合同步state和props的地方，因为已经确定会更新。

然后就是调用render\(\)方法。然后更新子组件。

子组件更新完，然后调用 componentDidUpdate\(prevProps,  prevState\).

在componentDidUpdate里面同样可以处理http请求\(ajax\)。

调用setState\(\)会导致重新渲染。

### 第三 在组件中调用setState会导致re-rendering:

![](/assets/lifecycle-3.png)

这个是由内部change，setState\(\)触发的re-rendering。

在shouldComponentUpdate\(nextProps,nextState\)中可以进行检查判断是不是进行更新。





几个参考网站  
[Understanding React — Component life-cycle](https://medium.com/@baphemot/understanding-reactjs-component-life-cycle-823a640b3e8d)  
[ReactJs component lifecycle methods — A deep dive](https://hackernoon.com/reactjs-component-lifecycle-methods-a-deep-dive-38275d9d13c0)

