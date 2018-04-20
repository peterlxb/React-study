第一个概念：[声明式渲染](https://cn.vuejs.org/v2/guide/index.html#声明式渲染)

> **Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统：**

```HTML
<div id="app">
  {{ message }}
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

输出内容

![](/assets/屏幕快照 2018-04-20 下午2.42.27.png)

![](/assets/屏幕快照 2018-04-19 下午2.32.04.png)

