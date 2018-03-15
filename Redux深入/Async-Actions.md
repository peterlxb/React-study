# Async Actions 异步操作
官方文档中的说明

当调用一个异步的API，有两个关键的时间点。开始调用的时刻，以及收到回答的时刻(或是timeout).

这两个时刻一般都需要应用的状态随之改变，为了实现它，我们需要派发常规的actions，来由reducers同步处理。
通常情况下对于任意一个API请求，你会派发三种不同的actions。


1. 一个action用来通知reducers请求开始了。
处理这个action的reducers也许就只是切换下state中的isFetching标识。通过这种方式来让UI知道是时候来显示一个spinner了。

2. 一个用来通知reducers请求已经成功完成。
处理这个action的reducers也许通过合并新的数据到它管理的state中然后重置isFetching。UI将会隐藏这个spinner.然后显示获取到的数据。

3. 一个action用来通知reducers请求失败了。
处理这个action的reducers也许会重置isFetching.另外，某些reducers也许会想存储这个error信息来显示在UI。

以burger应用中的异步请求为例。

```JavaScript
export const purchaseBurgerSuccess = (id,orderData) => {
  return {
    type:PURCHASE_BURGER_SUCCESS,
    orderId:id,
    orderData:orderData
  }
}

export const purchaseBurgerFail = (error) => {
  return {
    type: PURCHASE_BURGER_FAIL,
    error
  }
}

export const purchaseBurgerStart = () => {
  return {
    type:PURCHASE_BURGER_START
  }
}

export const purchaseBurger = (orderData) => {
  return dispatch => {
    dispatch(purchaseBurgerStart());
    axios.post('/orders.json',orderData)
      .then( response => {
        console.log(response.data);
        dispatch(purchaseBurgerSuccess(response.data, orderData));
      })
      .catch(error => {
        dispatch(purchaseBurgerFail(error));
      })
  }
}
```

首先定义了三个normal action,这三个action对应API 请求的start，succedd,fail三个过程。
处理异步请求的操作放在purchaseBurger中，这里用到了redux-thunk中间件。
当调用这个async action时，第一步就是dispatch(start-action)。然后根据调用请求返回的数据来dispatch不同的action。
