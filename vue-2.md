---
title: Vue学习笔记（二）
date: 2017-04-24 08:51:11
tags: Vue
categories: 前端
---
## 生命周期
### created
实例已经创建的时候执行，即 `new Vue` 成功后执行

```
new Vue({
	el:"#box",
	data:{
		msg:"hi vue"
	},
	created:function(){
		console.log('实例已创建');
	}
})
```

### beforeCompile
从 `data` 中的 `msg` 到真正的 `html` 显示需要经过编译，编译之前(beforeCompile)

>注意：Vue2.0 使用 created 钩子函数替代。
**详解：**实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。


### compiled
编译之后

>注意：Vue2.0 使用 mounted 钩子函数替代。
**详解：**el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内。

### ready
插入到文档中

>注意：Vue2.0 使用新的 mounted 钩子函数替代。使用 mounted 并不能保证钩子函数中的 this.$el 在 document 中。

为此还应该引入 Vue.nextTick/vm.$nextTick。例如：

```
mounted: function () {
	this.$nextTick(function () {
		// 代码保证 this.$el 在 document 中
	})
}
```
### beforeDestroy
实例销毁之前调用。在这一步，实例仍然完全可用。
**该钩子在服务器端渲染期间不被调用。**

```
//
document.onclick = function(){
	vm.$destroy();
}
```

### destroyed
Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
**该钩子在服务器端渲染期间不被调用。**

### 生命周期图示
下图说明了实例的生命周期。你不需要立马弄明白所有的东西，不过以后它会有帮助。
![vue-lifecycle](http://blogpic.at15cm.com/vue-lifecycle.png)


## 解决 Mustache 闪烁
用户有可能会看到花括号闪烁，解决：

1.v-cloak
用法：这个指令保持在元素上直到关联实例结束编译。和 CSS 规则如 `[v-cloak] { display: none }` 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。

```
//html goes here...
<div v-cloak>
  {{ message }}
</div>
//CSS goes here...
[v-cloak] {
  display: none;
}
```

2.v-text/v-html

```
<span v-text="msg"></span>
<!-- 和下面的一样 -->
<span>{{msg}}</span>
```


## 计算属性
**computed:**
计算属性将被混入到 Vue 实例中。
所有 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例。

>注意，不应该使用箭头函数来定义计算属性函数 (例如 aDouble: () => this.a * 2)。理由是箭头函数绑定了父级作用域的上下文，所以 this 将不会按照期望指向 Vue 实例，this.a 将是 undefined。

计算属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。

```
var vm = new Vue({
	el:"#box",
	data:{
		a:1
	},
	computed:{
		b:function(){	//这里的b是属性，默认调用get，值取决于return的值
			//业务逻辑代码...
			return this.a + 1;
		}
	}
});
```

这里 computed 里的 b 的完整用法为：

```
b:{
	get:function(){
		//return this.a + 1;
	},
	set:function(val){
		//this.a = val;
	}
}
```

**`computed` 里面可以放置一些业务逻辑代码，一定记得 return**

示例：

```
var vm = new Vue({
  data: { a: 1 },
  computed: {
    // 仅读取，值只须为函数
    aDouble: function () {
      return this.a * 2
    },
    // 读取和设置
    aPlus: {
      get: function () {
        return this.a + 1
      },
      set: function (v) {
        this.a = v - 1
      }
    }
  }
})
vm.aPlus   // -> 2
vm.aPlus = 3
vm.a       // -> 2
vm.aDouble // -> 4
```


## 常用实例属性和方法
### vm.$el
+ 类型: `HTMLElement`
+ 只读
+ 详细:
Vue 实例使用的根 DOM 元素。

### vm.$data
+ 类型: `Object`
+ 详细:
Vue 实例观察的数据对象。Vue 实例代理了对其 data 对象属性的访问。

实例创建之后，可以通过 `vm.$data` 访问原始数据对象。Vue 实例也代理了 `data` 对象上所有的属性，因此访问 `vm.a` 等价于访问 `vm.$data.a`。

### vm.$options
+ 类型: Object
+ 只读
+ 详细:
用于当前 Vue 实例的初始化选项。需要在选项中包含自定义属性时会有用处：

```
new Vue({
  customOption: 'foo',
  created: function () {
    console.log(this.$options.customOption) // -> 'foo'
  }
})
```

### vm.$mount
+ 返回值： `vm` - 实例自身

+ 用法：
如果 Vue 实例在实例化时没有收到 el 选项，则它处于“未挂载”状态，没有关联的 DOM 元素。可以使用 vm.$mount() 手动地挂载一个未挂载的实例。
这个方法返回实例自身，因而可以链式调用其它实例方法。

```
var vm = new Vue({
	data:{
		msg:"hi vue"
	}
}).mount("#box");
```

## 过滤器
### debounce
延迟操作

```
<input v-on:keyup="doStuff | debounce 500">
```

>注意：Vue2.0 替换了 `debounce` 过滤器，使用 lodash’s debounce (也有可能是 throttle) 来直接控制高耗任务

```
//html goes here...
<input v-on:keyup="doStuff">
//js goes here...
methods: {
	doStuff: _.debounce(function () {
		// ...
	}, 500)
}
```

### limitBy
限制数据个数(取几个)
```
<p v-for="item in items | limitBy 10">{{ item }}</p>
```

>注意：Vue2.0替换了 `limitBy` 过滤器，在 computed 属性中使用js内置方法： `.slice`。

```
//html goes here...
<div id="box">
	<ul>
		<li v-for="item in items">{{item}}</li>
	</ul>
</div>
```

```
//js goes here...
var vm = new Vue({
  el:"#box",
  data:{
    arr:[1,2,3,4,5]
  },
  computed:{
    items:function() {
      return this.arr.slice(0,3);
    }
  }
});
```

### filterBy
过滤数据

```
<p v-for="user in users | filterBy searchQuery in 'name'">{{ user.name }}</p>
<ul>
	<li v-for="val in arr|filterBy a">{{val}}</li>
</ul>
```

>注意：Vue2.0替换了 `filterBy` 过滤器，在 computed 属性中使用js内置方法 `.filter`。

```
//html goes here...
<p v-for="user in filteredUsers">{{ user.name }}</p>
//js goes here...
computed: {
  filteredUsers: function () {
    var self = this
    return self.users.filter(function (user) {
      return user.name.indexOf(self.searchQuery) !== -1
    })
  }
}
```

实例：
```
var vm = new Vue({
  el:"#box",
  data:{
    arr:['arthur','clover','luke','penny'],
    letter:''
  },
  computed:{
    items:function() {
      return this.arr.filter(function(arr){
        return arr.indexOf('r')!==-1;
      });
    }
  }
});
```

### orderBy
排序

```
<p v-for="user in users | orderBy 'name'">{{ user.name }}</p>
```

>注意：Vue2.0替换了 `orderBy` 过滤器，而是在 computed 属性中使用 lodash’s orderBy (或者可能是 sortBy)。

```
<p v-for="user in orderedUsers">{{ user.name }}</p>
```

```
computed: {
	orderedUsers: function () {
		return _.orderBy(this.users, 'name')
	}
}
```

甚至可以字段排序：

```
_.orderBy(this.users, ['name', 'last_login'], ['asc', 'desc'])
```

### 自定义过滤器
**Vue.filter**
用法：
注册或获取全局过滤器。

```
// 注册
Vue.filter('my-filter', function (value) {
	// 返回处理后的值
})
// getter，返回已注册的过滤器
var myFilter = Vue.filter('my-filter')
```

实例：
```
Vue.filter('toDou',function(input){
	return input<10?'0'+input:input;
});
```


## 自定义指令
### Vue.directive
用法：
注册或获取全局指令。

```
// 注册
Vue.directive('my-directive', {
	bind: function () {},
	inserted: function () {},
	update: function () {},
	componentUpdated: function () {},
	unbind: function () {}
})
// 注册（传入一个简单的指令函数）
Vue.directive('my-directive', function () {
	// 这里将会被 `bind` 和 `update` 调用
})
// getter，返回已注册的指令
var myDirective = Vue.directive('my-directive')
```

示例：
```
//html goes here...
<span v-red>test</span>
```

```
//js goes here...
Vue.directive('red',function(){
	this.el.style.color='red';
})
```

>注意：指令以 `v-` 开头

实例：实现拖拽

```
//html goes here...
<div id="box">
	<div v-drag :style={width:'100px',height:'100px',background:'aqua',position:'absolute',left:0,top:0}></div>
</div>
```

```
//js goes here...
Vue.directive('drag',function() {
  // var oDiv = document.getElementById('box1');
  var oDiv = this.el;
  oDiv.onmousedown = function(ev){
    var disX = ev.clientX - oDiv.offsetLeft;
    var disY = ev.clientY - oDiv.offsetTop;
    document.onmousemove = function(ev){
      var l = ev.clientX - disX;
      var t = ev.clientY - disY;
      oDiv.style.left = l + 'px';
      oDiv.style.top = t + 'px';
    };
    document.onmouseup = function(){
      document.onmousemove = null;
      document.onmouseup = null;
    };
  };
});
var vm = new Vue({
  el:"#box",
  data:{}
});
```


### 自定义元素指令
```
Vue.elementDirective('zns-red',{
	bind:function(){
		this.el.style.background = 'red';
	}
})
```


## 自定义键盘信息
```
<input type="text" @keydown.ctrl="show">	//这里按 ctrl 键并不会执行 show
//ctrl 键码为 17
<input type="text" @keydown.17="show">		//这里就能执行
```

解决：

```
Vue.directive('on').keyCodes.ctrl = 17;
```

>注意：Vue2.0替换了 `Vue.directive('on').keyCodes`，新的简明配置 `keyCodes` 的方式是通过 `Vue.config.keyCodes`。

示例：

```
Vue.config.keyCodes.f1 = 112;
```

>这里不用 `ctrl` 做演示，因为在 Vue2.0 里完美支持，到这里我实在是无语了... Strive大神你出来咱们探讨一下人生...


## 监听数据变化
**vm.$watch(expOrFn,callback,[options])**

+ 返回值： {Function} unwatch
+ 用法：
观察 Vue 实例变化的一个表达式或计算属性函数。
回调函数得到的参数为新值和旧值。
表达式只接受监督的键路径。
对于更复杂的表达式，用一个函数取代。

>注意：在变异（不是替换）对象或数组时，旧值将与新值相同，因为它们的引用指向同一个对象/数组。Vue 不会保留变异之前值的副本。

+ 示例：

```
// 键路径
vm.$watch('a.b.c', function (newVal, oldVal) {
  // 做点什么
})
// 函数
vm.$watch(
  function () {
    return this.a + this.b
  },
  function (newVal, oldVal) {
    // 做点什么
  }
)
```

+ `vm.$watch` 返回一个取消观察函数，用来停止触发回调：

```
var unwatch = vm.$watch('a', cb)
// 之后取消观察
unwatch()
```

+ 选项：deep
为了发现对象内部值的变化，可以在选项参数中指定 deep: true 。
注意监听数组的变动不需要这么做。

```
vm.$watch('someObject', callback, {
  deep: true
})
vm.someObject.nestedValue = 123
// callback is fired
```

+ 选项：immediate
在选项参数中指定 `immediate: true` 将立即以表达式的当前值触发回调：

```
vm.$watch('a', callback, {
  immediate: true
})
// 立即以 a 的当前值触发回调
```

+ 示例：
```
vm.$watch('a',function(){
	alert('changed');
})