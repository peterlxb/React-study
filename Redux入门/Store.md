# Store
在redux应用中只有一个store。当需要拆分数据处理逻辑时，应该使用reducer composition而不是
创建多个store。
创建一个store很简单
先引入创建的reducer，然后使用createStore()方法
```JavaScript
import { createStore } from 'redux'
import todoApp from './reducers'
let store = createStore(todoApp)
```
三个主要方法
1. getState()
 不接受任何参数，返回当前store的state
2. dispatch()
 接收一个action对象，然后会调用reducer函数，传入当前state和action。
 示例如下
 ```JavaScript
  let store = createStore(reducer);
  const receiveComment = comment => ({
    type:"RECEIVE_COMMENT",
    comment
  });

  store.dispatch(receiveComment('Redux application'));
  export default store
 ```
3. subscribe()
  store.subscribe()接收一个listener callback function，无论何时store的状态发生改变了
  都会调用回调函数。
