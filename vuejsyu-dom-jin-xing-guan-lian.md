![](/assets/屏幕快照 2018-04-17 下午2.19.31.png)



## 绑定元素特性：

```
<div id="app-2">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>
```

JS

```
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '页面加载于 ' + new Date().toLocaleString()
  }
})
```

![](/assets/屏幕快照 2018-04-17 下午2.50.29.png)

看到的v-bind特性被称为**指令**。指令带有前缀`v-`，以表示它们是 Vue 提供的特殊特性。

> **它们会在渲染的 DOM 上应用特殊的响应式行为。**

在这里，该指令的意思是：“将这个元素节点的title特性和 Vue 实例的`message`属性保持一致”。不用{{}}

类似的还有绑定链接

```
<p><a v-bind:href="link" target="_blank">Google</a></p>
```

将链接的href属性与Vue 实例的link属性保持一致。

