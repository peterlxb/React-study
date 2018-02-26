# Actions
定义一个action很简单
Actions are payloads of information that send data from your application to your store. They are the only source of information for the store. You send them to the store using store.dispatch().
上面这段话来自Redux官网,Actions是store的信息的来源。通过调用store.dispatch()来把它们传递到store。
下面是一个添加新todo条目的示例代码
```JavaScript
  const ADD_TODO = 'ADD_TODO'

  {
    type: ADD_TODO,
    text: 'Build my first Redux app'
  }
```
Actions必须定义type属性，type属性用来表明执行何种action。而且type应该被定义为字符串常量。
官网的示例是一个Todo应用,所以要添加更多的不同type的action。比如通过索引index引用一个特殊的todo。
```JavaScript
  {
    type: TOGGLE_TODO,
    index: 5
  }
```
尽可能的每个action只传递少量值是个很好的方式。
示例中最后添加了一个action type来改变可显示的todo。
```JavaScript
  {
  type: SET_VISIBILITY_FILTER,
  filter: SHOW_COMPLETED
  }
```

## Action creators
在Redux中action creators只是返回一个action的函数。
```JavaScript
  function addTodo(text) {
    return {
      type: ADD_TODO,
      text
    }
  }
```
在Redux实际操作中，可直接把action creator传递到store.dispatch()中。
```JavaScript
  dispatch(addTodo(text))
  dispatch(completeTodo(index))
```
或者也可以创建bound action creator 来自动dispatch action
```JavaScript
  const boundAddTodo = text => dispatch(addTodo(text))
  const boundCompleteTodo = index => dispatch(completeTodo(index))
```
然后就可以直接调用这些函数。
```JavaScript
  boundAddTodo(text)
  boundCompleteTodo(index)
```

完整的actions.js源码
```JavaScript
/*
 * action types
 */
 
export const ADD_TODO = 'ADD_TODO'
export const TOGGLE_TODO = 'TOGGLE_TODO'
export const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER'
 
/*
 * other constants
 */
 
export const VisibilityFilters = {
  SHOW_ALL: 'SHOW_ALL',
  SHOW_COMPLETED: 'SHOW_COMPLETED',
  SHOW_ACTIVE: 'SHOW_ACTIVE'
}
 
/*
 * action creators
 */
 
export function addTodo(text) {
  return { type: ADD_TODO, text }
}
 
export function toggleTodo(index) {
  return { type: TOGGLE_TODO, index }
}
 
export function setVisibilityFilter(filter) {
  return { type: SET_VISIBILITY_FILTER, filter }
}
```
