# *实例*

```html
<div id="app">
  {{ message }}
</div>
```

当这些数据改变时，视图会进行重渲染。值得注意的是只有当实例被创建时 `data` 中存在的属性才是**响应式**的

# 模板语法

1. 通过使用 [v-once 指令](https://cn.vuejs.org/v2/api/#v-once)，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定：

```
<span v-once>这个将不会改变: {{ msg }}</span>
```

2. 双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 `v-html` 指令：

```
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

3. 计算属性：

```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```

```js
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

#samples with getter and setter
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```

4. 侦听属性

虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。这就是为什么 Vue 通过 `watch` 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

```html
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
```

```js
<!-- 因为 AJAX 库和通用工具的生态已经相当丰富，Vue 核心代码没有重复 -->
<!-- 提供这些功能以保持精简。这也可以让你自由选择自己更熟悉的工具。 -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
    // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
    // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
    // 请参考：https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)'
        return
      }
      this.answer = 'Thinking...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error
        })
    }
  }
})
</script>
```

# v-bind for class and style

```html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

上面的语法表示 `active` 这个 class 存在与否将取决于数据属性 `isActive` 的 [truthiness](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy)。

```html
<div v-bind:class="[activeClass, errorClass]"></div>
```

```js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

**style**

```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

```js
data: {
  activeColor: 'red',
  fontSize: 30
}
```

```html
<div v-bind:style="styleObject"></div>
```

```html
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

# v-if, v-else, v-else-if

1. 因为 `v-if` 是一个指令，所以必须将它添加到一个元素上。但是如果想切换多个元素呢？此时可以把一个 `<template>` 元素当做不可见的包裹元素，并在上面使用 `v-if`。最终的渲染结果将不包含 `<template>` 元素。

2. `v-else` 元素必须紧跟在带 `v-if` 或者 `v-else-if` 的元素的后面，否则它将不会被识别。

3.  Vue 为你提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 `key` 属性即可：

   ```html
   <template v-if="loginType === 'username'">
     <label>Username</label>
     <input placeholder="Enter your username" key="username-input">
   </template>
   <template v-else>
     <label>Email</label>
     <input placeholder="Enter your email address" key="email-input">
   </template>
   ```

4. 当 `v-if` 与 `v-for` 一起使用时，`v-for` 具有比 `v-if` 更高的优先级。

# v-for

```html
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```

```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

1. 在 `v-for` 块中，我们拥有对父作用域属性的完全访问权限
2. `v-for` 还支持一个可选的第二个参数为当前项的索引。

```html
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```



3. 提供第二个的参数为键名：

```html
<div v-for="(value, key) in object">
  {{ key }}: {{ value }}
</div>
```

4. 第三个参数为索引：

```js
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }}: {{ value }}
</div>
```

5. Vue 包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下：
   - `push()`
   - `pop()`
   - `shift()`
   - `unshift()`
   - `splice()`
   - `sort()`
   - `reverse()`
6. 非变异 (non-mutating method) 方法，例如：`filter()`, `concat()` 和 `slice()`
7. 由于 JavaScript 的限制，Vue 不能检测以下变动的数组：
   1. 当你利用索引直接设置一个项时，例如：`vm.items[indexOfItem] = newValue`
   2. 当你修改数组的长度时，例如：`vm.items.length = newLength`

   ```js
Vue.set(vm.items, indexOfItem, newValue);
vm.items.splice(newLength)
   ```

8. `v-for` 也可以取整数。在这种情况下，它将重复多次模板。

```html
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```

9. 类似于 `v-if`，你也可以利用带有 `v-for` 的 `<template>` 渲染多个元素

# v-on

许多事件处理逻辑会更为复杂，所以直接把 JavaScript 代码写在 `v-on` 指令中是不可行的。因此 `v-on` 还可以接收一个需要调用的方法名称

```js
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
```

1. 除了直接绑定到一个方法，也可以在内联 JavaScript 语句中调用方法

2. 需要在内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 `$event` 把它传入方法

3. Vue.js 为 `v-on` 提供了**事件修饰符**。之前提过，修饰符是由点开头的指令后缀来表示的。

   - `.stop`
   - `.prevent`
   - `.capture`
   - `.self`
   - `.once`
   - `.passive`

4. 在监听键盘事件时，我们经常需要检查常见的键值。Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符：

   ```
   <!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
   <input v-on:keyup.13="submit">
   ```

   记住所有的 `keyCode` 比较困难，所以 Vue 为最常用的按键提供了别名：

   ```
   <!-- 同上 -->
   <input v-on:keyup.enter="submit">
   
   <!-- 缩写语法 -->
   <input @keyup.enter="submit">
   ```

   全部的按键别名：

   - `.enter`
   - `.tab`
   - `.delete` (捕获“删除”和“退格”键)
   - `.esc`
   - `.space`
   - `.up`
   - `.down`
   - `.left`
   - `.right`

   可以通过全局 `config.keyCodes` 对象[自定义按键修饰符别名](https://cn.vuejs.org/v2/api/#keyCodes)：

   ```
   // 可以使用 `v-on:keyup.f1`
   Vue.config.keyCodes.f1 = 112
   ```

5. 可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

   - `.ctrl`
   - `.alt`
   - `.shift`
   - `.meta`

6. `.exact` 修饰符允许你控制由精确的系统修饰符组合触发的事件。

   ```html
   <!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
   <button @click.ctrl="onClick">A</button>
   
   <!-- 有且只有 Ctrl 被按下的时候才触发 -->
   <button @click.ctrl.exact="onCtrlClick">A</button>
   
   <!-- 没有任何系统修饰符被按下的时候才触发 -->
   <button @click.exact="onClick">A</button>
   ```

7. ### [鼠标按钮修饰符](https://cn.vuejs.org/v2/guide/events.html#%E9%BC%A0%E6%A0%87%E6%8C%89%E9%92%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6)

   - `.left`
   - `.right`
   - `.middle`

   这些修饰符会限制处理函数仅响应特定的鼠标按钮。

# v-model

可以用 `v-model` 指令在表单 `<input>` 及 `<textarea>` 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素`<input v-model="message" placeholder="edit me"><p>Message is: {{ message }}</p>`

```html

```

```html
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
```

# 组件

这里有一个 Vue 组件的示例：

```
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

组件是可复用的 Vue 实例，且带有一个名字：在这个例子中是 `<button-counter>`。我们可以在一个通过 `new Vue` 创建的 Vue 根实例中，把这个组件作为自定义元素来使用：

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

1.  **一个组件的 data 选项必须是一个函数**，因此每个实例可以维护一份被返回对象的独立的拷贝. 否则，所有的实例将共享一个值，这样所有的组件就将不再独立

## 局部注册

```js
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }

new Vue({
  el: '#app'
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

对于 `components` 对象中的每个属性来说，其属性名就是自定义元素的名字，其属性值就是这个组件的选项对象。

注意**局部注册的组件在其子组件中不可用**

## [通过 Prop 向子组件传递数据](https://cn.vuejs.org/v2/guide/components.html#%E9%80%9A%E8%BF%87-Prop-%E5%90%91%E5%AD%90%E7%BB%84%E4%BB%B6%E4%BC%A0%E9%80%92%E6%95%B0%E6%8D%AE)

```js
#定义并注册了子组件，并绑定了property 属性
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
#生成父组件，相关数据保存在父组件中
new Vue({
  el: '#blog-post-demo',
  data: {
    posts: [
      { id: 1, title: 'My journey with Vue' },
      { id: 2, title: 'Blogging with Vue' },
      { id: 3, title: 'Why Vue is so fun' },
    ]
  }
})
#通过v-bind的方式，将父组件中的值传递到了子组件上
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>
```

可以使用 `v-bind` 来动态传递 prop



### 单向数据流

所有的 prop 都使得其父子 prop 之间形成了一个**单向下行绑定**：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外改变父级组件的状态，从而导致你的应用的数据流向难以理解。

额外的，每次父级组件发生更新时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你**不**应该在一个子组件内部改变 prop。如果你这样做了，Vue 会在浏览器的控制台中发出警告。

## [在组件上使用 `v-model`](https://cn.vuejs.org/v2/guide/components.html#%E5%9C%A8%E7%BB%84%E4%BB%B6%E4%B8%8A%E4%BD%BF%E7%94%A8-v-model)

组件内的 `<input>` 必须：

- 将其 `value` 特性绑定到一个名叫 `value` 的 prop 上
- 在其 `input` 事件被触发时，将新的值通过自定义的 `input` 事件抛出

```js
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})

<custom-input v-model="searchText"></custom-input>
```

# 插槽（占位符）

```js
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})

<alert-box>
  Something bad happened.
</alert-box>
```

### 具名插槽

在一个从component中传入多个插槽

```html
#template中的定义
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
#实际的html
<base-layout>
  <template slot="header">
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template slot="footer">
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

