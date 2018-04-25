### 第一个模版template

#### 插入普通文本

```
<span>Message: {{ msg }}</span>
```

#### 特性 v-bind打头

```
<div v-bind:id="dynamicId"></div>
<button v-bind:disabled="isButtonDisabled">Button</button>
```

如果`isButtonDisabled`的值是`null`、`undefined`或`false`，则`disabled`特性甚至不会被包含在渲染出来的`<button>`元素中。

#### [指令](https://cn.vuejs.org/v2/guide/syntax.html#指令) v-开头

```
<p v-if="seen">现在你看到我了</p>
<a v-on:click="doSomething">...</a>
```

#### 修饰符

```
<form v-on:submit.prevent="onSubmit">...</form>
```

# 计算属性和侦听器

```
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```

    var vm = new Vue({
      el: '#example',
      data: {
        message: 'Hello'
      },
      computed: {
        // 计算属性的 getter
        reversedMessage: function () {
          // `this` 指向 vm 实例
          return this.message.split('').reverse().join('')
        }
      }
    })

**计算属性是基于它们的依赖进行缓存的**。计算属性只有在它的相关依赖发生改变时才会重新求值。这就意味着只要`message`

还没有发生改变，多次访问`reversedMessage`计算属性会立即返回之前的计算结果，而不必再次执行函数。

### [绑定 HTML Class](https://cn.vuejs.org/v2/guide/class-and-style.html#绑定-HTML-Class)

```
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

绑定的数据对象不必内联定义在模板里：

```
<div v-bind:class="classObject"></div>
```

```
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

也可以在这里绑定一个返回对象的[计算属性](https://cn.vuejs.org/v2/guide/computed.html)

```
<div v-bind:class="classObject"></div>
```

```
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

可以把一个数组传给`v-bind:class`，以应用一个 class 列表：

```
<div v-bind:class="[activeClass, errorClass]"></div>
```

```
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

### `v-bind:style`的对象语法

```
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

```
data: {
  activeColor: 'red',
  fontSize: 30
}
```

### 事件处理

用`v-on`指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。

    <div id="example-2">
      <!-- `greet` 是在下面定义的方法名 -->
      <button v-on:click="greet">Greet</button>
    </div>

    var example2 = new Vue({
      el: '#example-2',
      data: {
        name: 'Vue.js'
      },
      // 在 `methods` 对象中定义方法
      methods: {
        greet: function (event) {
          // `this` 在方法里指向当前 Vue 实例
          alert('Hello ' + this.name + '!')
          // `event` 是原生 DOM 事件
          if (event) {
            alert(event.target.tagName)
          }
        }
      }
    })

    // 也可以用 JavaScript 直接调用方法
    example2.greet() // => 'Hello Vue.js!'

#### [事件修饰符](https://cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6)

修饰符是由点开头的指令后缀来表示的。

* `.stop`
* `.prevent`
* `.capture`
* `.self`
* `.once`

```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```

### 条件渲染[`v-if`](https://cn.vuejs.org/v2/guide/conditional.html#v-if)

```
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```

#### [`<template>`元素上使用`v-if`条件渲染分组](https://cn.vuejs.org/v2/guide/conditional.html#%E5%9C%A8-lt-template-gt-%E5%85%83%E7%B4%A0%E4%B8%8A%E4%BD%BF%E7%94%A8-v-if-%E6%9D%A1%E4%BB%B6%E6%B8%B2%E6%9F%93%E5%88%86%E7%BB%84)

```
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

`v-else-if`，顾名思义，充当`v-if`的“else-if 块”，可以连续使用：

```
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

#### [`key`管理可复用的元素](https://cn.vuejs.org/v2/guide/conditional.html#%E7%94%A8-key-%E7%AE%A1%E7%90%86%E5%8F%AF%E5%A4%8D%E7%94%A8%E7%9A%84%E5%85%83%E7%B4%A0)

```
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```

### [`v-if`vs`v-show`](https://cn.vuejs.org/v2/guide/conditional.html#v-if-vs-v-show)

`v-if`是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if`也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show`就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。





