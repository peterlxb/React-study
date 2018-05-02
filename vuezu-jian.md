一个vue组件简单示例

```
// Define a new component called button-counter
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

可以在全局vue实例中把这个组件当成一个自定义元素使用

```
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

```
new Vue({ el: '#components-demo' })
```

因为组件本身就是vue实例，所以也能设置`data`,`computed`, `watch`,`methods 等属性，以及`lifecycle hooks。

如下面的示例

```
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

有三个按钮，每次点击一个，只有被点击的组件状态发生改变。原因就是

> **That’s because each time you use a component, a new instance of it is created**.



