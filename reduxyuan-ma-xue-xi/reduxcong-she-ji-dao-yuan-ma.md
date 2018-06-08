# Redux从设计到源码

主要讲述三方面内容：

1. Redux 背后的设计思想

2. 源码分析以及自定义中间件

3. 开发中的最佳实践

# Redux背后的设计思想 {#redux-}

在讲设计思想前，先简单讲下Redux是什么？我们为什么要用Redux？

## Redux是什么？ {#redux-}

Redux是JavaScript状态容器，能提供可预测化的状态管理。

它认为：

* Web应用是一个状态机，视图与状态是一一对应的。

* 所有的状态，保存在一个对象里面。

我们先来看看“状态容器”、“视图与状态一一对应”以及“一个对象”这三个概念的具体体现。

![](/assets/状态容器.png)如上图，Store是Redux中的状态容器，它里面存储着所有的状态数据，每个状态都跟一个视图一一对应。

Redux也规定，一个State对应一个View。只要State相同，View就相同，知道了State，就知道View是什么样，反之亦然。

比如，当前页面分三种状态：loading（加载中）、success（加载成功）或者error（加载失败），那么这三个就分别唯一对应着一种视图。

现在我们对“状态容器”以及“视图与状态一一对应”有所了解了，那么Redux是怎么实现可预测化的呢？我们再来看下Redux的工作流程。

![](/assets/Redux工作流程.png)

首先，我们看下几个核心概念：

* Store：保存数据的地方，你可以把它看成一个容器，整个应用只能有一个Store。

* State：Store对象包含所有数据，如果想得到某个时点的数据，就要对Store生成快照，这种时点的数据集合，就叫做State。

* Action：State的变化，会导致View的变化。但是，用户接触不到State，只能接触到View。所以，State的变化必须是View导致的。Action就是View发出的通知，表示State应该要发生变化了。

* Action Creator：View要发送多少种消息，就会有多少种Action。如果都手写，会很麻烦，所以我们定义一个函数来生成Action，这个函数就叫Action Creator。

* Reducer：Store收到Action以后，必须给出一个新的State，这样View才会发生变化。这种State的计算过程就叫做Reducer。Reducer是一个函数，它接受Action和当前State作为参数，返回一个新的State。

* dispatch：是View发出Action的唯一方法。

然后我们过下整个工作流程：

1. 首先，用户（通过View）发出Action，发出方式就用到了dispatch方法。

2. 然后，Store自动调用Reducer，并且传入两个参数：当前State和收到的Action，Reducer会返回新的State

3. State一旦有变化，Store就会调用监听函数，来更新View。

到这儿为止，一次用户交互流程结束。可以看到，在整个流程中数据都是单向流动的，这种方式保证了流程的清晰。

## 为什么要用Redux？ {#-redux-}

前端复杂性的根本原因是大量无规律的交互和异步操作。

变化和异步操作的相同作用都是改变了当前View的状态，但是它们的无规律性导致了前端的复杂，而且随着代码量越来越大，我们要维护的状态也越来越多。

我们很容易就对这些状态何时发生、为什么发生以及怎么发生的失去控制。那么怎样才能让这些状态变化能被我们预先掌握，可以复制追踪呢？

这就是Redux设计的动机所在。

Redux试图让每个State变化都是可预测的，将应用中所有的动作与状态都统一管理，让一切有据可循。

![](/assets/redux之前.png)

如上图所示，如果我们的页面比较复杂，又没有用任何数据层框架的话，就是图片上这个样子：交互上存在父子、子父、兄弟组件间通信，数据也存在跨层、反向的数据流。

这样的话，我们维护起来就会特别困难，那么我们理想的应用状态是什么样呢？看下图：

![](/assets/redux之后.png)

架构层面上讲，我们希望UI跟数据和逻辑分离，UI只负责渲染，业务和逻辑交由其它部分处理，从数据流向方面来说, 单向数据流确保了整个流程清晰。

我们之前的操作可以复制、追踪出来，这也是Redux的主要设计思想。

综上，Redux可以做到：

* 每个State变化可预测。
* 动作与状态统一管理。

#### Redux与Flux {#redux-flux}

Redux是Flux思想的一种实现，同时又在其基础上做了改进。Redux还是秉承了Flux单向数据流、Store是唯一的数据源的思想。

![](/assets/Redux与Flux.png)最大的区别：

1. Redux只有一个Store。

Flux中允许有多个Store，但是Redux中只允许有一个，相较于Flux，一个Store更加清晰，容易管理。Flux里面会有多个Store存储应用数据，并在Store里面执行更新逻辑，当Store变化的时候再通知controller-view更新自己的数据；Redux将各个Store整合成一个完整的Store，并且可以根据这个Store推导出应用完整的State。

同时Redux中更新的逻辑也不在Store中执行而是放在Reducer中。单一Store带来的好处是，所有数据结果集中化，操作时的便利，只要把它传给最外层组件，那么内层组件就不需要维持State，全部经父级由props往下传即可。子组件变得异常简单。

1. Redux中没有Dispatcher的概念。

Redux去除了这个Dispatcher，使用Store的Store.dispatch\(\)方法来把action传给Store，由于所有的action处理都会经过这个Store.dispatch\(\)方法，Redux聪明地利用这一点，实现了与Koa、RubyRack类似的Middleware机制。Middleware可以让你在dispatch action后，到达Store前这一段拦截并插入代码，可以任意操作action和Store。很容易实现灵活的日志打印、错误收集、API请求、路由等操作。

# 源码分析 {#-}

查看源码的话先从GitHub把这个地址上拷下来，切换到src目录，如下图：

![](/assets/reduxSource.png)  


看下整体结构：

其中utils下面的Warning.js主要负责控制台错误日志的输出，我们直接忽略index.js是入口文件，createStore.js是主流程文件，其余4个文件都是辅助性的API。

我们先结合下流程分析下对应的源码。

![](/assets/reduxUse.png)

首先，我们从Redux中引入createStore方法，然后调用createStore方法，并将Reducer作为参数传入，用来生成Store。为了接收到对应的State更新，我们先执行Store的subscribe方法，将render作为监听函数传入。然后我们就可以dispatchaction了，对应更新view的State。

那么我们按照顺序看下对应的源码：

## 入口文件index.js {#-index-js}

![](/assets/index.png)

入口文件，上面一堆检测代码忽略，看红框标出部分，它的主要作用相当于提供了一些方法，这些方法也是Redux支持的所有方法。

然后我们看下主流程文件：createStore.js。

## 主流程文件：createStore.js {#-createstore-js}

![](/assets/createStore.png)createStore主要用于Store的生成，我们先整理看下createStore具体做了哪些事儿。

首先，一大堆类型判断先忽略，可以看到声明了一系列函数，然后执行了dispatch方法，最后暴露了dispatch、subscribe……几个方法。这里dispatch了一个init Action是为了生成初始的State树。

我们先挑两个简单的函数看下，getState和replaceReducer，其中getState只是返回了当前的状态。replaceReducer是替换了当前的Reducer并重新初始化了State树。这两个方法比较简单，下面我们在看下其它方法。

  
![](/assets/otherMethod.png)

订阅函数的主要作用是注册监听事件，然后返回取消订阅的函数，它把所有的订阅函数统一放一个数组里，只维护这个数组。

为了实现实时性，所以这里用了两个数组来分别处理dispatch事件和接收subscribe事件。

store.subscribe\(\)方法总结：

* 入参函数放入监听队列

* 返回取消订阅函数

再来看下store.dispatch\(\)--&gt;分发action，修改State的唯一方式。

![](/assets/dispatch.png)

store.dispatch\(\)方法总结：

* 调用Reducer，传参（currentState，action）。

* 按顺序执行listener。

* 返回action。

到这儿的话，主流程我们就讲完了，下面我们讲下几个辅助的源码文件。

## bindActionCreators.js {#bindactioncreators-js}

bindActionCreators把action creators转成拥有同名keys的对象，使用dispatch把每个action creator包装起来，这样可以直接调用它们。

![](/assets/bindActionCreators.png)

实际情况用到的并不多，惟一的应用场景是当你需要把action creator往下传到一个组件上，却不想让这个组件觉察到Redux的存在，而且不希望把Redux Store或dispatch传给它。

## combineReducers.js--&gt;用于合并Reducer {#combinereducers-js-reducer}

![](/assets/combineReducers.png)这个方法的主要功能是用来合并Reducer，因为当我们应用比较大的时候Reducer按照模块拆分看上去会比较清晰，但是传入Store的Reducer必须是一个函数，所以用这个方法来作合并。代码不复杂，就不细讲了。它的用法和最后的效果可以看下上面左侧图。

## compose.js--&gt;用于组合传入的函数 {#compose-js-}

![](/assets/compose.png)compose这个方法，主要用来组合传入的一系列函数，在中间件时会用到。可以看到，执行的最终结果是把各个函数串联起来。

## applyMiddleware.js--&gt;用于Store增强 {#applymiddleware-js-store-}

中间件是Redux源码中比较绕的一部分，我们结合用法重点看下。

首先看下用法：

```
const store = createStore(reducer,applyMiddleware(…middlewares))
or

const store = createStore(reducer,{},applyMiddleware(…middlewares))

```

可以看到，是将中间件作为createStore的第二个或者第三个参数传入，然后我们看下传入之后实际发生了什么。

![](/assets/applyMiddleware.png)

从代码的最后一行可以看到，最后的执行代码相当于applyMiddleware\(…middlewares\)\(createStore\)\(reducer,preloadedState\)然后我们去applyMiddleware里看它的执行过程。

![](/assets/ing.png)  
可以看到执行方法有三层，那么对应我们源码看的话最终会执行最后一层。最后一层的执行结果是返回了一个正常的Store和一个被变更过的dispatch方法，实现了对Store的增强。

这里假设我们传入的数组chain是［f,g,h］，那么我们的dispatch相当于把原有dispatch方法进行f,g,h层层过滤，变成了新的dispatch。

由此的话我们可以推出中间件的写法：因为中间件是要多个首尾相连的，需要一层层的“加工”，所以要有个next方法来独立一层确保串联执行，另外dispatch增强后也是个dispatch方法，也要接收action参数，所以最后一层肯定是action。

再者，中间件内部需要用到Store的方法，所以Store我们放到顶层，最后的结果就是：

![](/assets/middleware.png)

看下一个比较常用的中间件redux－thunk源码，关键代码只有不到10行。

![](/assets/thunk.png)作用的话可以看到，这里有个判断：如果当前action是个函数的话，return一个action执行，参数有dispatch和getState，否则返回给下个中间件。

这种写法就拓展了中间件的用法，让action可以支持函数传递。

我们来总结下这里面的几个疑点。

#### Q1：为什么要嵌套函数？为何不在一层函数中传递三个参数，而要在一层函数中传递一个参数，一共传递三层？ {#q1-}

因为中间件是要多个首尾相连的，对next进行一层层的“加工”，所以next必须独立一层。那么Store和action呢？Store的话，我们要在中间件顶层放上Store，因为我们要用Store的dispatch和getState两个方法。action的话，是因为我们封装了这么多层，其实就是为了作出更高级的dispatch方法，是dispatch，就得接受action这个参数。

#### Q2：middlewareAPI中的dispatch为什么要用匿名函数包裹呢？ {#q2-middlewareapi-dispatch-}

我们用applyMiddleware是为了改造dispatch的，所以applyMiddleware执行完后，dispatch是变化了的，而middlewareAPI是applyMiddleware执行中分发到各个middleware，所以必须用匿名函数包裹dispatch，这样只要dispatch更新了，middlewareAPI中的dispatch应用也会发生变化。

#### Q3: 在middleware里调用dispatch跟调用next一样吗？ {#q3-middleware-dispatch-next-}

因为我们的dispatch是用匿名函数包裹，所以在中间件里执行dispatch跟其它地方没有任何差别，而执行next相当于调用下个中间件。

# 最佳实践 {#-}

[官网](http://cn.redux.js.org/index.html)中对最佳实践总结的很到位，我们重点总结下以下几个:

* 用对象展开符增加代码可读性。

* 区分smart component（know the State）和dump component（完全不需要关心State）。

* component里不要出现任何async calls，交给action creator来做。

* Reducer尽量简单，复杂的交给action creator。

* Reducer里return state的时候，不要改动之前State，请返回新的。

* immutable.js配合效果很好（但同时也会带来强侵入性，可以结合实际项目考虑）。

* action creator里，用promise/async/await以及Redux-thunk（redux-saga）来帮助你完成想要的功能。

* action creators和Reducer请用pure函数。

* 请慎重选择组件树的哪一层使用connected component\(连接到Store\)，通常是比较高层的组件用来和Store沟通，最低层组件使用这防止太长的prop chain。

* 请慎用自定义的Redux-middleware，错误的配置可能会影响到其他middleware.

* 有些时候有些项目你并不需要Redux（毕竟引入Redux会增加一些额外的工作量）












