### 第一个模版template

#### 插入普通文本

```
<span>Message: {{ msg }}</span>
```

####  特性 v-bind打头

```
<div v-bind:id="dynamicId"></div>
<button v-bind:disabled="isButtonDisabled">Button</button>
```

如果`isButtonDisabled`的值是`null`、`undefined`或`false`，则`disabled`特性甚至不会被包含在渲染出来的`<button>`元素中。

#### [指令](https://cn.vuejs.org/v2/guide/syntax.html#%E6%8C%87%E4%BB%A4) v-开头

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



### [绑定 HTML Class](https://cn.vuejs.org/v2/guide/class-and-style.html#%E7%BB%91%E5%AE%9A-HTML-Class)

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













