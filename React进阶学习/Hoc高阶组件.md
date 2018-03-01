# 高阶组件
官方文档对于高阶组件的定义
a higher-order component is a function that takes a component and returns a new component.
可以把它理解为一个函数，这个函数接受一个组件作为参数然后返回一个新的组件。
```JavaScript
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```
在React中 component组件是将props 转变? 成UI.高阶组件是将组件转换成另一个组件。
像一些第三方库如Redux 中的 connect就是一个高阶组件。
文档中举的是一个评论的例子,
CommentList组件通过订阅一个外部数据源来渲染评论列表
```JavaScript
class CommentList extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      // "DataSource" is some global data source
      comments: DataSource.getComments()
    };
  }

  componentDidMount() {
    // Subscribe to changes
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    // Clean up listener
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    // Update component state whenever the data source changes
    this.setState({
      comments: DataSource.getComments()
    });
  }

  render() {
    return (
      <div>
        {this.state.comments.map((comment) => (
          <Comment comment={comment} key={comment.id} />
        ))}
      </div>
    );
  }
}
```
紧接着又有一个BlogPost组件遵循相同的模式
```JavaScript
class BlogPost extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      blogPost: DataSource.getBlogPost(props.id)
    };
  }

  componentDidMount() {
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    this.setState({
      blogPost: DataSource.getBlogPost(this.props.id)
    });
  }

  render() {
    return <TextBlock text={this.state.blogPost} />;
  }
}
```
示例中的CommentList 和 BlogPost并不是相同的，它们在DataSource上调用不同的方法，产生不同的输出
，但是实现的过程是相似的。
1. 在挂载时，在DataSource添加一个change 监听器listener.
2. 在监听器内部，无论合适data source发生改变旧调用setState()
3. 卸载时，删除监听器

在一个大型应用中，上面这种订阅DataSource，调用setState()将会出现很多次。
我们就需要抽象化，允许我们在一个单独的地方去定义这个逻辑，以便在不同的组件之间可以共享。
这就引入了高阶组件。

我们可以写一个函数，创造一个组件，比如 CommentList 并且订阅DataSource。
这个函数其中一个参数就是子组件，然后订阅的data作为prop。
将这个函数定义为 withSubscription
```JavaScript
const CommentListWithSubscription = withSubscription(
  CommentList,
  (DataSource) => DataSource.getComments()
);

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
);
```
可以看到第一个参数是被包裹的组件，第二个参数就检索我们感兴趣的数据，DataSource, props。
当CommentListWithSubscription，BlogPostWithSubscription被渲染时，CommentList 和BlogPost
将会被传入从DataSource检索的当前最新数据作为data prop。
```JavaScript
// This function takes a component...
function withSubscription(WrappedComponent, selectData) {
  // ...and returns another component...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      // ... that takes care of the subscription...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      // ... and renders the wrapped component with the fresh data!
      // Notice that we pass through any additional props
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```
注意到HOC不修改输入的组件(WrappedComponent)，也不用继承复制它的表现。
反之，HOC是通过将它包裹在容器组件的方式组合这个原来的组件。
一个HOC是一个没有负作用的纯函数。

这就是HOC的介绍，这个包裹的组件接收所有容器(container)的props,还有用来渲染它的输出的
一个新的prop,称为data。
HOC不关心这个data如何或者怎样使用的，被包裹的组件也不关心这个data从何而来。

回到上面的withSubscription，这是一个普通函数，意味着你可以尽可能的添加你想添加的参数。
例如想让data属性的名字是配置的，一遍更好的使HOC与被包裹的组件隔离。
类似于组件，withSubscription和被包裹的组件之间的关于是完全基于props的。
这使得把HOC从一个组件交换到另一个很简单。他们对于被包裹的组件提供的都是相同的props。
这对于当你改变获取数据的库时可能很有帮助。

## 参考资料
[react.js](https://reactjs.org/docs/higher-order-components.html)
[来自知乎](https://zhuanlan.zhihu.com/p/24776678)
[Understanding Higher Order Components](https://medium.freecodecamp.org/understanding-higher-order-components-6ce359d761b)
[理解高阶组件](https://zhuanlan.zhihu.com/p/29250138)
[React Higher Order Components in depth](https://medium.com/@franleplant/react-higher-order-components-in-depth-cf9032ee6c3e)
[Higher-Order Components (HOCs) for Beginners](https://hackernoon.com/higher-order-components-hocs-for-beginners-25cdcf1f1713)
