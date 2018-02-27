# Reducers
Reducers specify how the application's state changes in response to actions sent to the store. Remember that actions only describe the fact that something happened, but don't describe how the application's state changes.
Reducers根据传入store的action明确(specify)应用的state如何改变。actions只是用来描述将要发生什么，并不是藐视应用的state改变。
## 第一步:设计state的样子(shape)
比如文档中示例的Todo应用，在Redux中所有应用的状态都保存在一个单一的对象中，在开始开发前应该先思考这个state对象的结构。需要思考最小化的呈现时应用state对象结构是怎么样的。
对于todo应用，只需要保存两件事
1. 目前选中的visibility filter
2. 实际todos应用列表

在实际开发过程中会发现你需要在state tree中存储一些data(数据)，也包括一些UI state(状态)。
但是需要尽量将data和UI state分离开.
示例 state tree
```JavaScript
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
```
## 处理actions
文档定义
The reducer is a pure function that takes the previous state and an action, and returns the next state.
reducer就是一个纯函数，接受旧的state和action作为参数，返回新的state。
```JavaScript
(previousState, action) => newState
```
之所以称为reducer是因为Array.prototype.reduce函数。
一个reducer不能做的操作:
1. Mutate its arguments;
2. Perform side effects like API calls and routing transitions;
3. Call non-pure functions, e.g. Date.now() or Math.random().

reducer 必须是纯函数，关于纯函数的定义
Given the same arguments, it should calculate the next state and return it. No surprises. No side effects. No API calls. No mutations. Just a calculation.

开始的时候当Redux第一次调用reducer时传入的state是undefined.
这是我们返回应用的初始状态的最佳时机。
示例代码
```JavaScript
import { VisibilityFilters } from './actions'
 
const initialState = {
  visibilityFilter: VisibilityFilters.SHOW_ALL,
  todos: []
}
 
function todoApp(state, action) {
  if (typeof state === 'undefined') {
    return initialState
  }
 
  // For now, don't handle any actions
  // and just return the state given to us.
  return state
}
```
可以使用ES6 default arguments syntax默认参数语法写成更完整的reducer
```JavaScript
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    default:
      return state
  }
}
```
上面代码中我们不是直接修改state，而是利用Object.assign()创建一个state的复制。
也可以使用 object spread operator proposal对象展开运算符实现同样效果{ ...state, ...newState }
最后在default选项返回最初的state，对于任何未知的action返回prev state非常关键。
在任何时候都要确保reducer返回一个state(新的state或者之前的state)。

在React course中的示例
state 的结构
```JavaScript
  const initialCalendarState = {
    sunday:{
      breakfast:null,
      lunch:null,
      dinner:null,
    },
    monday:{
      breakfast:null,
      lunch:null,
      dinner:null,
    },
    tuesday:{
      breakfast:null,
      lunch:null,
      dinner:null,
    },
    ....
    saturday:{
      breakfast:null,
      lunch:null,
      dinner:null,
    }
  }
```
示例reducer
```JavaScript
  function calendar(state=initialCalendarState, action) {
    switch (action.type) {
      case ADD_RECIPE:
        return {
          ...state,
          [day]: { //使用ES6 computed property name
            ...state[day], //表示除了选定的那一天之外的所有天
            [meal]:recipe.label,
          }
        }
      case REMOVE_RECIPE:
        return {
          ...state,
          [day]:{
            ...state[day],
            [meal]:null,
          }
        }
    }
  }
```
在console中测试
```JavaScript
  > test1 = {
    ...initialCalendarState,
    "monday": {
      breakfast:"test1",
      lunch:null,
      dinner:null,
    }
  }
```
经过测试不加[],不影响最后的结果，返回的test1对象除了monday的breakfast的值改变，其他都未变。
在reducer中，直接用Object spread来返回新的对象。
同样在console中测试
```JavaScript
  test2 = {
    ...initialCalendarState, //第一个...用来过滤其他day
    ["monday"]:{
      ...initialCalendarState["monday"],//第二个...用来过滤breakfast和dinner
      ["lunch"]:"good food"
    }
  }
```
上面代码同样是创造一个新对象，只有Monday 的lunch选项改变。
