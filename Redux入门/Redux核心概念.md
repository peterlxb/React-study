# Core Concepts 来自Redux官网

想象应用的state(状态)是一个plain object(纯对象字面量)，一个todo应用应该类似

```JavaScript
{
  todos: [{
    text: 'Eat food',
    completed: true
  }, {
    text: 'Exercise',
    completed: false
  }],
  visibilityFilter: 'SHOW_COMPLETED'
}
```
要想改变state中的某个数据，需要派发一个action(dispatch an action).
action本身只是一个Javascript对象，如下所示

```JavaScript
  { type: 'ADD_TODO', text: 'Go to swimming pool' }
  { type: 'TOGGLE_TODO', index: 1 }
  { type: 'SET_VISIBILITY_FILTER', filter: 'SHOW_ALL' }
```
强制要求每次更改都被描述成一个action让我们清楚的知道app里面发生了什么。
如果某个数据发生了改变，我们就能知道为何发生了改变。

最后要将state和action绑在一起。就需要一个叫reducer的函数。
没有多余的魔法，它就是一个函数，接收state和action作为参数，然后返回
下一个state。

```JavaScript
function visibilityFilter(state = 'SHOW_ALL', action) {
  if (action.type === 'SET_VISIBILITY_FILTER') {
    return action.filter
  } else {
    return state
  }
}
 
function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([{ text: action.text, completed: false }])
    case 'TOGGLE_TODO':
      return state.map(
        (todo, index) =>
          action.index === index
            ? { text: todo.text, completed: !todo.completed }
            : todo
      )
    default:
      return state
  }
}
```
然后添加另外一个reducer来管理我们整个app的完整的state
```JavaScript
function todoApp(state = {}, action) {
  return {
    todos: todos(state.todos, action),
    visibilityFilter: visibilityFilter(state.visibilityFilter, action)
  }
}
```
上面基本上是整个Redux的核心概念。还没有使用任何Redux API。
可以概述为如何描述state是根据action来改变的。

# Three Principles
Redux有三大基本原则
## Single source of truth
  The state of your whole application is stored in an object tree within a single store.
  整个应用的state(状态)都保存在一个对象树中。
  这种方式使得创建全局应用更方便。单一的状态树夜使得调试更简单。
  ```JavaScript
  console.log(store.getState())
 
  /* Prints
  {
  visibilityFilter: 'SHOW_ALL',
  todos: [
    {
      text: 'Consider using Redux',
      completed: true,
    },
    {
      text: 'Keep all state in a single tree',
      completed: false
    }
  ]
  }
  */
  ```
## State is read-only
The only way to change the state is to emit an action, an object describing what happened.
唯一可以改变state的方式是触发一个action。
所有的更改都集中在一起按照严格的顺序一个接一个。
```JavaScript
  store.dispatch({
  type: 'COMPLETE_TODO',
  index: 1
  })
   
  store.dispatch({
  type: 'SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED'
  })
```
## Changes are made with pure functions
To specify how the state tree is transformed by actions, you write pure reducers.
为了明确state树是如何被action改变，需要编写纯reducer函数。
Reducers都是纯函数，接收之前(previous)的state和action作为参数，然后返回写一个state。
要记住是返回新的state对象，而不是直接修改之前(previous)的state。
```JavaScript
  function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      return action.filter
    default:
      return state
  }
  }
   
  function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case 'COMPLETE_TODO':
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: true
          })
        }
        return todo
      })
    default:
      return state
  }
  }
   
  import { combineReducers, createStore } from 'redux'
  const reducer = combineReducers({ visibilityFilter, todos })
  const store = createStore(reducer)
```
