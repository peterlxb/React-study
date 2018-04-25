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

> Vue  creates a template based on out HTML code store internally and basic uses this template to
>
> create a real HTML code which then is rendered as DOM
>
> The HTML code we write is not the one running in the browser in the end.
>
> There is the layer in the middle and this layer is the vue instance which takes our HTML code creates a templates 
>
> and then render this templates like {{ .title }} ,then output the final HTML code which gets rendered



