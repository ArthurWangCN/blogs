---
title:  Vue学习笔记（三） 组件
date: 2017-04-25 20:41:56
tags: Vue
categories: 前端
---
## 什么是组件？
组件（Component）是 Vue.js 最强大的功能之一。组件可以扩展 HTML 元素，封装可重用的代码。在较高层面上，组件是自定义元素， Vue.js 的编译器为它添加特殊功能。在有些情况下，组件也可以是原生 HTML 元素的形式，以 is 特性扩展。


## 使用组件
### 注册
之前说过，我们可以通过以下方式创建一个 Vue 实例：

```
new Vue({
  el: '#some-element',
  // 选项
})
```

要注册一个全局组件，你可以使用 `Vue.component(tagName, options)`。 例如：

```
Vue.component('my-component', {
  // 选项
})
```

>对于自定义标签名，Vue.js 不强制要求遵循 W3C规则 （小写，并且包含一个短杠），尽管遵循这个规则比较好。

组件在注册之后，便可以在父实例的模块中以自定义元素 `<my-component></my-component>` 的形式使用。要确保在初始化根实例 之前 注册了组件：

```
<div id="example">
  <my-component></my-component>
</div>
```

```
// 注册
Vue.component('my-component', {
  template: '<div>A custom component!</div>'
})
// 创建根实例
new Vue({
  el: '#example'
})
```

渲染为：

```
<div id="example">
  <div>A custom component!</div>
</div>
```

### 局部注册
不必在全局注册每个组件。通过使用组件实例选项注册，可以使组件仅在另一个实例/组件的作用域中可用：

```
var Child = {
  template: '<div>A custom component!</div>'
}
new Vue({
  // ...
  components: {
    // <my-component> 将只在父模板可用
    'my-component': Child
  }
})
```

这种封装也适用于其它可注册的 Vue 功能，如指令。


### DOM 模版解析说明
当使用 DOM 作为模版时（例如，将 `el` 选项挂载到一个已存在的元素上）, 你会受到 HTML 的一些限制，因为 Vue 只有在浏览器解析和标准化 HTML 后才能获取模版内容。尤其像这些元素 `ul` ，`ol`，`table` ，`select` 限制了能被它包裹的元素， 而一些像 `option` 这样的元素只能出现在某些其它元素内部。

在自定义组件中使用这些受限制的元素时会导致一些问题，例如：

```
<table>
  <my-row>...</my-row>
</table>
```

自定义组件 <my-row> 被认为是无效的内容，因此在渲染的时候会导致错误。变通的方案是使用特殊的 `is` 属性：

```
<table>
  <tr is="my-row"></tr>
</table>
```

应当注意，如果您使用来自以下来源之一的字符串模板，这些限制将不适用：

+ `<script type="text/x-template">`
+ JavaScript内联模版字符串
+ `.vue` 组件

因此，有必要的话请使用字符串模版。


### data 必须是函数
通过Vue构造器传入的各种选项大多数都可以在组件里用。 `data` 是一个例外，它必须是函数。 
实际上，如果你这么做：

```
Vue.component('my-component', {
  template: '<span>{{ message }}</span>',
  data: {
    message: 'hello'
  }
})
```

那么 Vue 会停止，并在控制台发出警告，告诉你在组件中 `data` 必须是一个函数。理解这种规则的存在意义很有帮助，让我们假设用如下方式来绕开Vue的警告：

```
<div id="example-2">
  <simple-counter></simple-counter>
  <simple-counter></simple-counter>
  <simple-counter></simple-counter>
</div>
```

```
var data = { counter: 0 }
Vue.component('simple-counter', {
  template: '<button v-on:click="counter += 1">{{ counter }}</button>',
  // 技术上 data 的确是一个函数了，因此 Vue 不会警告，
  // 但是我们返回给每个组件的实例的却引用了同一个data对象
  data: function () {
    return data
  }
})
new Vue({
  el: '#example-2'
})
```

由于这三个组件共享了同一个 `data` ， 因此增加一个 counter 会影响所有组件！这不对。我们可以通过为每个组件返回全新的 data 对象来解决这个问题：

```
data: function () {
  return {
    counter: 0
  }
}
```

现在每个 counter 都有它自己内部的状态了。


### 构成组件
组件意味着协同工作，通常父子组件会是这样的关系：组件 A 在它的模版中使用了组件 B 。它们之间必然需要相互通信：父组件要给子组件传递数据，子组件需要将它内部发生的事情告知给父组件。然而，在一个良好定义的接口中尽可能将父子组件解耦是很重要的。这保证了每个组件可以在相对隔离的环境中书写和理解，也大幅提高了组件的可维护性和可重用性。

在 Vue.js 中，父子组件的关系可以总结为 props down, events up 。父组件通过 props 向下传递数据给子组件，子组件通过 events 给父组件发送消息。看看它们是怎么工作的。

![props-events](http://blogpic.at15cm.com/props-events.png)


## Prop
###使用 Prop 传递数据
组件实例的作用域是孤立的。这意味着不能(也不应该)在子组件的模板内直接引用父组件的数据。要让子组件使用父组件的数据，我们需要通过子组件的 `props` 选项。

子组件要显式地用 `props` 选项声明它期待获得的数据：

```
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 就像 data 一样，prop 可以用在模板内
  // 同样也可以在 vm 实例中像 “this.message” 这样使用
  template: '<span>{{ message }}</span>'
})
```

然后我们可以这样向它传入一个普通字符串：

```
<child message="hello!"></child>
```


### camelCase vs. kebab-case
HTML 特性是不区分大小写的。所以，当使用的不是字符串模版，camelCased (驼峰式) 命名的 prop 需要转换为相对应的 kebab-case (短横线隔开式) 命名：

```
Vue.component('child', {
  // camelCase in JavaScript
  props: ['myMessage'],
  template: '<span>{{ myMessage }}</span>'
})
```

```
<!-- kebab-case in HTML -->
<child my-message="hello!"></child>
```

如果你使用字符串模版，则没有这些限制。

### 动态Prop
在模板中，要动态地绑定父组件的数据到子模板的props，与绑定到任何普通的HTML特性相类似，就是用 v-bind。每当父组件的数据变化时，该变化也会传导给子组件：

```
<div>
  <input v-model="parentMsg">
  <br>
  <child v-bind:my-message="parentMsg"></child>
</div>
```

使用 v-bind 的缩写语法通常更简单：

```
<child :my-message="parentMsg"></child>
```

### 字面量语法 vs 动态语法
初学者常犯的一个错误是使用字面量语法传递数值：

```
<!-- 传递了一个字符串 "1" -->
<comp some-prop="1"></comp>
```

因为它是一个字面 prop ，它的值是字符串 `"1"` 而不是number。如果想传递一个实际的number，需要使用 `v-bind` ，从而让它的值被当作 JavaScript 表达式计算：

```
<!-- 传递实际的 number -->
<comp v-bind:some-prop="1"></comp>
```

### 单向数据流
prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是不会反过来。这是为了防止子组件无意修改了父组件的状态——这会让应用的数据流难以理解。

另外，每次父组件更新时，子组件的所有 prop 都会更新为最新值。这意味着你不应该在子组件内部改变 prop 。如果你这么做了，Vue 会在控制台给出警告。

为什么我们会有修改prop中数据的冲动呢？通常是这两种原因：

1. prop 作为初始值传入后，子组件想把它当作局部数据来用；
2. prop 作为初始值传入，由子组件处理成其它数据输出。

对这两种原因，正确的应对方式是：

+ 定义一个局部变量，并用 prop 的值初始化它：

```
props: ['initialCounter'],
data: function () {
	return { counter: this.initialCounter }
}
```

+ 定义一个计算属性，处理 prop 的值并返回。
```
props: ['size'],
computed: {
	normalizedSize: function () {
		return this.size.trim().toLowerCase()
	}
}

>注意在 JavaScript 中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它会影响父组件的状态。


### Prop 验证
我们可以为组件的 props 指定验证规格。如果传入的数据不符合规格，Vue 会发出警告。当组件给其他人使用时，这很有用。

要指定验证规格，需要用对象的形式，而不能用字符串数组：

```
Vue.component('example', {
  props: {
    // 基础类型检测 （`null` 意思是任何类型都可以）
    propA: Number,
    // 多种类型
    propB: [String, Number],
    // 必传且是字符串
    propC: {
      type: String,
      required: true
    },
    // 数字，有默认值
    propD: {
      type: Number,
      default: 100
    },
    // 数组／对象的默认值应当由一个工厂函数返回
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
})
```

type 可以是下面原生构造器：

+ String
+ Number
+ Boolean
+ Function
+ Object
+ Array

type 也可以是一个自定义构造器函数，使用 instanceof 检测。

当 prop 验证失败，Vue会在抛出警告 (如果使用的是开发版本)。




## 使用 Slot 分发内容
在使用组件时，我们常常要像这样组合它们：

```
<app>
  <app-header></app-header>
  <app-footer></app-footer>
</app>
```

注意两点：

+ <app> 组件不知道它的挂载点会有什么内容。挂载点的内容是由<app>的父组件决定的。
+ <app> 组件很可能有它自己的模版。

为了让组件可以组合，我们需要一种方式来混合父组件的内容与子组件自己的模板。这个过程被称为 `内容分发` (或 “transclusion” 如果你熟悉 Angular)。Vue.js 实现了一个内容分发 API ，参照了当前 Web 组件规范草案，使用特殊的 <slot> 元素作为原始内容的插槽。

### 编译作用域
在深入内容分发 API 之前，我们先明确内容在哪个作用域里编译。假定模板为：

```
<child-component>
  {{ message }}
</child-component>

```

message 应该绑定到父组件的数据，还是绑定到子组件的数据？答案是父组件。组件作用域简单地说是：

父组件模板的内容在父组件作用域内编译；子组件模板的内容在子组件作用域内编译。

一个常见错误是试图在父组件模板内将一个指令绑定到子组件的属性/方法：

```
<!-- 无效 -->
<child-component v-show="someChildProperty"></child-component>
```

假定 `someChildProperty` 是子组件的属性，上例不会如预期那样工作。父组件模板不应该知道子组件的状态。

如果要绑定作用域内的指令到一个组件的根节点，你应当在组件自己的模板上做：

```
Vue.component('child-component', {
  // 有效，因为是在正确的作用域内
  template: '<div v-show="someChildProperty">Child</div>',
  data: function () {
    return {
      someChildProperty: true
    }
  }
})
```

类似地，分发内容是在父作用域内编译。


### 单个 Slot
除非子组件模板包含至少一个 `<slot>` 插口，否则父组件的内容将会被丢弃。当子组件模板只有一个没有属性的 slot 时，父组件整个内容片段将插入到 slot 所在的 DOM 位置，并替换掉 slot 标签本身。

最初在 `<slot>` 标签中的任何内容都被视为备用内容。备用内容在子组件的作用域内编译，并且只有在宿主元素为空，且没有要插入的内容时才显示备用内容。

假定 my-component 组件有下面模板：

```
<div>
  <h2>我是子组件的标题</h2>
  <slot>
    只有在没有要分发的内容时才会显示。
  </slot>
</div>
```

父组件模版：

```
<div>
  <h1>我是父组件的标题</h1>
  <my-component>
    <p>这是一些初始内容</p>
    <p>这是更多的初始内容</p>
  </my-component>
</div>
```

渲染结果：

```
<div>
  <h1>我是父组件的标题</h1>
  <div>
    <h2>我是子组件的标题</h2>
    <p>这是一些初始内容</p>
    <p>这是更多的初始内容</p>
  </div>
</div>
```

### 具名 Slot
`<slot>` 元素可以用一个特殊的属性 `name` 来配置如何分发内容。多个 slot 可以有不同的名字。具名 slot 将匹配内容片段中有对应 slot 特性的元素。

仍然可以有一个匿名 slot ，它是默认 slot ，作为找不到匹配的内容片段的备用插槽。如果没有默认的 slot ，这些找不到匹配的内容片段将被抛弃。

例如，假定我们有一个 app-layout 组件，它的模板为：

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

父组件模版：

```
<app-layout>
  <h1 slot="header">这里可能是一个页面标题</h1>
  <p>主要内容的一个段落。</p>
  <p>另一个主要段落。</p>
  <p slot="footer">这里有一些联系信息</p>
</app-layout>
```

渲染结果为：

```
<div class="container">
  <header>
    <h1>这里可能是一个页面标题</h1>
  </header>
  <main>
    <p>主要内容的一个段落。</p>
    <p>另一个主要段落。</p>
  </main>
  <footer>
    <p>这里有一些联系信息</p>
  </footer>
</div>
```

在组合组件时，内容分发 API 是非常有用的机制。


### 作用域插槽
>2.1.0 新增

作用域插槽是一种特殊类型的插槽，用作使用一个（能够传递数据到）可重用模板替换已渲染元素。

在子组件中，只需将数据传递到插槽，就像你将 prop 传递给组件一样：

```
<div class="child">
  <slot text="hello from child"></slot>
</div>
```

在父级中，具有特殊属性 scope 的 `<template>` 元素，表示它是作用域插槽的模板。scope 的值对应一个临时变量名，此变量接收从子组件中传递的 prop 对象：

```
<div class="parent">
  <child>
    <template scope="props">
      <span>hello from parent</span>
      <span>{{ props.text }}</span>
    </template>
  </child>
</div>
```

如果我们渲染以上结果，得到的输出会是：

```
<div class="parent">
  <div class="child">
    <span>hello from parent</span>
    <span>hello from child</span>
  </div>
</div>
```

作用域插槽更具代表性的用例是列表组件，允许组件自定义应该如何渲染列表每一项：

```
<my-awesome-list :items="items">
  <!-- 作用域插槽也可以是具名的 -->
  <template slot="item" scope="props">
    <li class="my-fancy-item">{{ props.text }}</li>
  </template>
</my-awesome-list>
```

列表组件的模板：

```
<ul>
  <slot name="item"
    v-for="item in items"
    :text="item.text">
    <!-- 这里写入备用内容 -->
  </slot>
</ul>
```

## 补充
**Strive讲父子组件**

```
new Vue({
	el:"#box",
	data:{		
	},
	components:{
		'aaa':{
			data:function(){
				return {
					msg:'I belong to aaa'
				}
			},
			template:"<h1>我是aaa---{{msg}}</h1><bbb></bbb>",
			components:{
				'bbb':{
					props:['msg'],
					template:'<h3>我是bbb---{{msg}}</h3>'
				}
			}
		}
	}
});
```

>注意：Vue2.0 以后组件模板的根节点只能有一个，因此控制台会有一个警告


vue默认情况下子组件不能访问父组件的数据，组件数据传递：`props`
1.子组件获取父组件数据
调用子组件：
```
<bbb m="数据"></bbb>
```
子组件之内：
```
props:['m','myMsg']
```

```
props:{
	'm':String,
	'myMsg':Number
}
```
2.父组件获取子组件数据
子组件把自己的数据发送到父级
vm.emit(事件名，数据);
v-on:


更多内容见 [Vue中文网--组件](http://cn.vuejs.org/v2/guide/components.html)