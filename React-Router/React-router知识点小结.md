# 在Burger应用中添加React Router v4

主要问题是两个containers组件 BurgerBuilder与Checkout组件之间通过url参数传递数据。
应用的逻辑是在主界面，当选择了各种原料(ingredient)，点击continue按钮会弹出Modal.
在这个Modal里有刚刚选择的原料的信息和价格totalPrice.
下面是点击继续按钮后触发的方法。
```JavaScript
purchaseContinueHandler = () => {

  const queryParams=[];

  for (let i in this.state.ingredients) {
    queryParams.push(encodeURIComponent(i) + '=' + encodeURIComponent(this.state.ingredients[i]));
  }
  queryParams.push('price=' + this.state.totalPrice);
  const queryString = queryParams.join('&');
  this.props.history.push({
    pathname:'/checkout',
    search:'?' + queryString
  });
}
```
在这里面用到了history的push方法，最终会跳转到/checkout 路径下。
传递的第二个参数search就是传递数据的关键。
里面用到了encodeURIComponent()。
这里面先将ingredients对象转变成[salad=1,bacon=2]这样数组形式，然后用数组的join方法将数组转变成字符串。

MDN 上的定义
encodeURIComponent()是对统一资源标识符（URI）的组成部分进行编码的方法。它使用一到四个转义序列来表示字符串中的每个字符的UTF-8编码
为了避免服务器收到不可预知的请求，对任何用户输入的作为URI部分的内容你都需要用encodeURIComponent进行转义。
通过这个方法转义后浏览器跳转的地址类似/checkout?bacon=1&cheese=1&meat=0&salad=1&price=5.60这样


回到checkout组件
使用URLSearchParams处理URL的查询字符串
```JavaScript
componentWillMount() {
  const query = new URLSearchParams(this.props.location.search);
  const ingredients=  {};
  let price = 0;
  for (let param of query.entries()) {
    if(param[0] === 'price') {
      price = param[1];

    } else {
      ingredients[param[0]] = +param[1];
    }

  };
  console.log(price)
  this.setState({ingredients:ingredients,totalPrice:price});
}
```
待处理的数据都在URL的查询字符串中，所以用到URLSearchParams方法。目前是个实验性质的API。
URLSearchParams.entries()返回一个iterator可以遍历所有键/值对的对象。
这里的操作相反，将字符串转变成对象。
