原文地址:[https://blog.pusher.com/getting-started-with-react-router-v4/](https://blog.pusher.com/getting-started-with-react-router-v4/)

官方文档:https://reacttraining.com/react-router/web/guides/philosophy

## 1.[&lt;BrowserRouter&gt;](https://reacttraining.com/web/api/BrowserRouter)

一般常见用法直接包裹应用的App组件

```
import { BrowserRouter } from 'react-router-dom'

<BrowserRouter
  basename={optionalString}
  forceRefresh={optionalBool}
  getUserConfirmation={optionalFunc}
  keyLength={optionalNumber}
>
  <App/>
</BrowserRouter>
```

里面的参数都是可选，basename这个参数可以确定应用的base url，用法如下

```
<BrowserRouter basename="/calendar"/>
<Link to="/today"/> // renders <a href="/calendar/today">
```

加了basename属性/calendar,在Link中指定的路径会跳转到/calendar/today

## 2.[&lt;Route&gt;](https://reacttraining.com/web/api/Route)

这个route组件应该是react-router中最重要的，需要理解和掌握。

> Its most basic responsibility is to render some UI when a [location](https://reacttraining.com/web/api/location) matches the route’s`path`

官方文档的解释这个组件最基本的用法就是当匹配一个路径时显示特定的内容。

基本用法

```
import {Route } from 'react-router-dom';

  <div>
    <Route exact path="/" component={Home}/>
    <Route path="/news" component={NewsFeed}/>
  </div>
```

很简单，如果应用的位置在/,渲染Home组件，像下面这样

```
<div>
  <Home/>
  <!-- react-empty: 2 -->
</div>
```

如果应用的位置/news,则UI有点类似

```
<div>
  <!-- react-empty: 1 -->
  <NewsFeed/>
</div>
```

还有另外一种写法

```
<Route path="/" render={() => <h1>Home</h1>}/>
```

**在被&lt;Route&gt;包裹的组件内打印this.props,或自动获取与当前route有关的属性**

**history,match,location**

## 3.[&lt;Link&gt;](https://reacttraining.com/web/api/Link) and [&lt;NavLink&gt;](https://reacttraining.com/web/api/NavLink)

常用的组件，代替&lt;a&gt;标签，非常直观。

用&lt;a&gt;标签在应用的不同链接间跳转时可能会发生重新加载的情况，用&lt;Link&gt;代替&lt;a&gt;可以提高性能

```
import { Link } from 'react-router-dom'

<Link to="/about">About</Link>
```

点击后跳到/about路径，这个/about路径渲染的UI由&lt;route&gt;指定。

另一种更复杂的写法

```
<Link to={{
  pathname: '/courses',
  search: '?sort=name',
  hash: '#the-hash',
  state: { fromDashboard: true }
}}/>
```

### [&lt;NavLink&gt;](https://reacttraining.com/web/api/NavLink)

可以理解成&lt;Link&gt;的加强版，添加了特定的样式

```
import { NavLink } from 'react-router-dom'

<NavLink to="/about">About</NavLink>
```

添加样式类activeClassName

```
<NavLink
  to="/faq"
  activeClassName="selected"
>FAQs</NavLink>
```

直接添加样式属性

```
<NavLink
  to="/faq"
  activeStyle={{
    fontWeight: 'bold',
    color: 'red'
   }}
>FAQs</NavLink>
```

## 4.[withRouter](https://reacttraining.com/web/api/withRouter)

通过这个高阶组件，被&lt;Route&gt;包裹的子组件就能获取到history属性。

如udemy中的Post组件没有引用withRouter时this.props仅仅包裹传递的title,author,key值。

通过withRouter\(Post\)处理后this.props就包括history,match,location属性。

例子中要向下层组件传递route相关属性，也可以通过下面的方式

```
match = {this.props.match}
```

就像传递其他数据一样。

## 5.[&lt;Redirect&gt;](https://reacttraining.com/web/api/Redirect)

指向一个新的位置，一般需要加判断。

> The new location will override the current location in the history stack。

基本用法

```
import { Route, Redirect } from 'react-router'

<Route exact path="/" render={() => (
  loggedIn ? (
    <Redirect to="/dashboard"/>
  ) : (
    <PublicHomePage/>
  )
)}/>
```

跳转到另一个页面也可以用history.replace\(\)方法。

用了history.replace\(\)点击后退按钮不会回到之前的location

官方文档有个简单的关于auth重定向的示例。

## 6.Parameters

文档中示例有关url参数的。

主要用到match.params.id属性对应route

## 7.[&lt;Switch&gt;](https://reacttraining.com/web/api/Switch)

渲染第一个匹配的子&lt;Route&gt;或&lt;Redirect&gt;。

示例

```
import { Switch, Route } from 'react-router'

<Switch>
  <Route exact path="/" component={Home}/>
  <Route path="/about" component={About}/>
  <Route path="/:user" component={User}/>
  <Route component={NoMatch}/>
</Switch>
```

在&lt;Switch&gt;中&lt;Route&gt;的顺序很重要。匹配到第一个就不会往下匹配了

