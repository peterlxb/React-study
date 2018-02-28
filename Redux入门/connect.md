# connect
[原文地址](https://github.com/reactjs/react-redux/blob/master/docs/api.md#connectmapstatetoprops-mapdispatchtoprops-mergeprops-options)
## connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
这个方法本身并不会修改传入的 class component。相反它返回一个新的相关联的component给你使用。
参数
1. [mapStateToProps(state, [ownProps]): stateProps] (Function):
  指定了这个参数，意味着这个新组件会点阅Redux store的更新。这意味为任何时候store发生了改变都会调用mapStateToProps。
  mapStateToProps的结果必须是一个对象字面量。而且这个结果会被合并到新组件的props属性中。如果不想订阅store的更新，用null或undefined
  替代mapStateToProps.

2. [mapDispatchToProps(dispatch, [ownProps]): dispatchProps] (Object or Function):
