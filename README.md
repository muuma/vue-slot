# vue-slot

### 安装依赖

`yarn`

### 运行项目

`yarn serve`

[文档地址](https://cn.vuejs.org/v2/guide/components-slots.html)

> Vue 实现了一套内容分发的 API，这套 API 的设计灵感源自 [Web Components 规范草案](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md)，将 `<slot>` 元素作为承载分发内容的出口。

# 例子（Vue 2.6.0 以上语法）

### 1. 插槽内容

子组件 `<navigation-link>`
```
<a v-bind:href="url" class="nav-link" >
  <slot></slot>
</a>
```
父组件
```
<navigation-link url="/profile">
  Your Profile
</navigation-link>  
```
> 当组件渲染的时候，`<slot></slot>` 将会被替换为“Your Profile”。

插槽内可以包含任何模板代码：
```
<navigation-link url="/profile">
  <!-- 添加一个 Font Awesome 图标 -->
  <span class="fa fa-user"></span>
  Your Profile
</navigation-link>
```
甚至其它的组件：
```
<navigation-link url="/profile">
  <!-- 添加一个图标的组件 -->
  <font-awesome-icon name="user"></font-awesome-icon>
  Your Profile
</navigation-link>
```
> 如果 `<navigation-link>` 没有包含一个 `<slot>` 元素，则该组件起始标签和结束标签之间的任何内容都会被抛弃。

### 2. 默认内容（后备内容）
子组件 `<submit-button>`
```
<button type="submit">
  <slot>Submit</slot>  <!-- Submit 就是后备内容 -->
</button>
```
父组件
 - 当在一个父级组件中使用 `<submit-button>` 并且不提供任何插槽内容时，后备内容“Submit”将会被渲染：
```
<submit-button></submit-button>
```
- 如果提供内容，则这个提供的内容将会被渲染从而取代后备内容：
```
<submit-button>Save</submit-button>
```
### 3. 具名插槽
> `<slot>` 元素有一个特殊的特性：`name`。这个特性可以用来定义额外的插槽：

子组件 `<base-layout>`
```
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
```
> 一个不带 `name` 的 `<slot>` 出口会带有隐含的名字“default”。

向具名插槽提供内容时，可以在一个 `<template>` 元素上使用 `v-slot` 指令，并以 `v-slot` 的参数的形式提供其名称：
```
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```
> 任何没有被包裹在带有 `v-slot` 的 `<template>` 中的内容都会被视为默认插槽的内容。

**注意：** `v-slot` 只能添加在一个 `<template>` 上，只有一种情况例外（详见“作用域插槽-独占默认插槽的缩写语法”）。

### 4. 编译作用域
当你想在一个插槽中使用数据时，例如第一个例子：
```
<navigation-link url="/profile">
  Logged in as {{ user.name }}
</navigation-link>
```
> 该插槽跟模板（父组件中）的其它地方一样可以访问相同的实例属性 (也就是相同的“作用域”)，而不能访问 `<navigation-link>` 的作用域。例如 `url` 是访问不到的：
```
<navigation-link url="/profile">
  Clicking here will send you to: {{ url }}
  <!-- 这里的 `url` 会是 undefined，
      因为"/profile"是传递给<navigation-link>的，只有在<navigation-link>内部可以访问 -->
</navigation-link>
```
> **父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。**

### 5. 作用域插槽（这里我测试跟文档有点出入，具体看代码）
有时让插槽内容能够访问子组件中才有的数据是很有用的。例如：
子组件 `<current-user>`
```
<span>
  <slot>{{ user.lastName }}</slot>
</span>

<script>  
data () {
  return {
    user: {
      firstName: 'current-user-first-name',
      lastName: 'current-user-last-name'
    }
  }
}
</script>  
```
父组件
```
<current-user>
  {{ user.firstName }}
</current-user>
``` 
> 然而上述代码不会正常工作，因为只有 `<current-user>` 组件可以访问到 `user` 而我们提供的内容是在父级渲染的。

> 为了让 `user` 在父级的插槽内容可用，我们可以将 `user` 作为一个 `<slot> `元素的特性绑定上去：
```
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```
> 绑定在 `<slot>` 元素上的特性被称为**插槽 prop**。现在在父级作用域中，我们可以给 `v-slot` 带一个值来定义我们提供的**插槽 prop** 的名字：

```
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.firstName }}
  </template>
</current-user>
```
- #### 独占默认插槽的缩写语法
> 在上述情况下，当被提供的内容只有默认插槽时，组件的标签才可以被当作插槽的模板来使用。这样我们就可以把 `v-slot` 直接用在组件上：
```
<current-user v-slot:default="slotProps">
  {{ slotProps.firstName }}
</current-user>
```
或
```
<current-user v-slot="slotProps">
  {{ slotProps.firstName }}
</current-user>
```
**注意：**
1. 默认插槽的缩写语法不能和具名插槽混用，因为它会导致作用域不明确：
```
<!-- 无效，会导致警告 -->
<current-user v-slot="slotProps">
  {{ slotProps.firstName }}
  <template v-slot:other="otherSlotProps">
    slotProps is NOT available here
  </template>
</current-user>
```
  2. 只要出现多个插槽，请始终为所有的插槽使用完整的基于 `<template>` 的语法：
```

<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.firstName }}
  </template>

  <template v-slot:other="otherSlotProps">
    ...
  </template>
</current-user>
```
- #### 解构插槽 Prop
> 作用域插槽的内部工作原理是将你的插槽内容包括在一个传入单个参数的函数里：
```
function (slotProps) {
  // 插槽内容
}
```
> 这意味着 `v-slot` 的值实际上可以是任何能够作为函数定义中的参数的 JavaScript 表达式。所以在支持的环境下 (单文件组件或现代浏览器)，你也可以使用 [ES2015 解构](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#%E8%A7%A3%E6%9E%84%E5%AF%B9%E8%B1%A1)来传入具体的插槽 prop，如下：
```
<current-user v-slot="{ user }">
  {{ user.firstName }}
</current-user>
```
> 这样可以使模板更简洁，尤其是在该插槽提供了多个 prop 的时候。它同样开启了 prop 重命名等其它可能，例如将 `user` 重命名为 `person`：
```
<current-user v-slot="{ user: person }">
  {{ person.firstName }}
</current-user>
```
> 你甚至可以定义后备内容，用于插槽 prop 是 `undefined` 的情形：
```
<current-user v-slot="{ user = { firstName: 'Guest' } }">>
  {{ user.firstName }}
</current-user>
```
### 6. 动态插槽名
> [动态指令参数](https://cn.vuejs.org/v2/guide/syntax.html#%E5%8A%A8%E6%80%81%E5%8F%82%E6%95%B0)也可以用在 `v-slot` 上，来定义动态的插槽名
```
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```
### 7. 具名插槽的缩写
`v-slot` 的缩写，即把参数之前的所有内容 (`v-slot:`) 替换为字符 `#`。例如 `v-slot:header` 可以被重写为 `#header`：
```
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```
**注意：**和其它指令一样，该缩写只在其有参数的时候才可用：
```
<!-- 这样会触发一个警告 -->
<current-user #="{ user }">
  {{ user.firstName }}
</current-user>
```
如果你希望使用缩写的话，你必须始终以明确插槽名取而代之：
```
<current-user #default="{ user }">
  {{ user.firstName }}
</current-user>
```
### 8. 其他示例
> **插槽 prop 允许我们将插槽转换为可复用的模板，这些模板可以基于输入的 prop 渲染出不同的内容。**这在设计封装数据逻辑同时允许父级组件自定义部分布局的可复用组件时是最有用的。

例如，子组件 `<todo-list>` ：
```
<ul>
  <li v-for="todo in filteredTodos" v-bind:key="todo.id">
    <!-- 我们为每个 todo 准备了一个插槽， 将 `todo` 对象作为一个插槽的 prop 传入。 -->
    <slot v-bind:todo="todo">
      <!-- 后备内容 -->
      {{ todo.text }}
    </slot>
  </li>
</ul>
```
父组件：
```
<todo-list v-bind:todos="todos">
  <template v-slot:todo="{ todo }">
    <span v-if="todo.isComplete">✓</span>
    {{ todo.text }}
  </template>
</todo-list>
```

参考：[理解vue中的scope的使用](https://www.cnblogs.com/tugenhua0707/p/7745735.html)