# Data Flow in Redux
Redux遵循严格的unidirectional data flow单向数据流。
这意味着应用中的数据都遵循相同的生命周期模式，这使得你的应用的逻辑更可预测也更容易理解。

任何一个Redux应用都遵循下面四个步骤
1. call store.dispatch(action)
  对于action的定义An action is a plain object describing what happened.
  例如下面的例子
  ```JavaScript
  { type: 'LIKE_ARTICLE', articleId: 42 }
  { type: 'FETCH_USER_SUCCESS', response: { id: 3, name: 'Mary' } }
  { type: 'ADD_TODO', text: 'Read the Redux docs.' }
  ```
  上面的action其实可以简单的理解成新闻的片段，比如"Mary喜欢文章42"或者"Read the Redux docs."
  被添加到了todo list中。
  可以在应用的任何一个地方调用store.dispatch(action)，包括在组件中或xhr回调中。
2. The Redux store calls the reducer function you gave it.
  Redux store会调用你传递给它的reducer。
  store会传递两个参数给reducer，当前的state tree和action。
  以todo app为例root reducer也许会接收如下所示的数据。
  ```JavaScript
  // The current application state (list of todos and chosen filter)
  let previousState = {
    visibleTodoFilter: 'SHOW_ALL',
    todos: [
      {
        text: 'Read the docs.',
        complete: false
      }
    ]
  }
   
  // The action being performed (adding a todo)
  let action = {
    type: 'ADD_TODO',
    text: 'Understand the flow.'
  }
   
  // Your reducer returns the next application state
  let nextState = todoApp(previousState, action)
    ```
  要时刻记住reducer是pure function。它只负责计算下一个state。它应该是完全可预测的。
  多次输入一样的值产生的输出都是一样的。它不应该用于一些会产生副作用的操作，如API调用，路由过渡(route transition)。这些操作应该在dispatch action之前发生。
3. The root reducer may combine the output of multiple reducers into a single state tree.
  主要讲的是combineReducers()方法
  可以自由决定如何构建你自己的root reducer，也就是主reducer。
  Redux提供的combineRefucers()帮助函数用于将root reducer 分解成分裂的函数，每个管理state tree中一个分之。
  还是以todo app 为例，有两个reducers，其中一个是todo 列表的，另一个负责当前煊泽过滤设置。
  ```JavaScript
  function todos(state = [], action) {
 // Somehow calculate it...
    return nextState
  }
 
  function visibleTodoFilter(state = 'SHOW_ALL', action) {
   // Somehow calculate it...
   return nextState
  }
   
  let todoApp = combineReducers({
   todos,
   visibleTodoFilter
  })
  ```
  当触发一个action，combineReducers返回的todoApp会调用两个reducer
  ```JavaScript
  let nextTodos = todos(state.todos, action)
  let nextVisibleTodoFilter = visibleTodoFilter(state.visibleTodoFilter, action)
  ```
  然后会将两个结果组合到一个单一的state tree中。
  ```JavaScript
  return {
    todos: nextTodos,
    visibleTodoFilter: nextVisibleTodoFilter
  }

  ```
4. The Redux store saves the complete state tree returned by the root reducer.
  Redux store保存着由root reducer 返回的完整的state tree。
  这个新的tree 就是应用的下一个state。
  每一个由store.subscribe(listener)注册的listener都会被调用。
  这个时候相应的UI状态就可以根据新的state更新。如果是使用react-redux，
  这个点就是component.setState(newState)调用的时机。
  
