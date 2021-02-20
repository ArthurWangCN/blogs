---
title:  Vue学习笔记（三） 过渡效果
date: 2017-04-24 19:22:38
tags: Vue
categories: 前端
---
## Bower
Bower是一个客户端技术的软件包管理器，它可用于搜索、安装和卸载如JavaScript、HTML、CSS之类的网络资源。

常用命令：

```
bower install <包名>
bower uninstall <包名>s
bower info <包名>
```

示例：

```
bower info vue
```

参考：[bower简明入门教程](https://segmentfault.com/a/1190000002971135)
[Mac上安装Bower](http://legendaryarthur.cn/2017/04/24/bower-install/)



## 过渡效果
>本质是 `CSS3` 的 `transition` 和 `animation`

Vue 在插入、更新或者移除 DOM 时，提供多种不同方式的应用过渡效果。
包括以下工具：

+ 在 CSS 过渡和动画中自动应用 class
+ 可以配合使用第三方 CSS 动画库，如 Animate.css
+ 在过渡钩子函数中使用 JavaScript 直接操作 DOM
+ 可以配合使用第三方 JavaScript 动画库，如 Velocity.js

### 单元素/组件的过渡
Vue 提供了 `transition` 的封装组件，在下列情形中，可以给任何元素和组件添加 `entering/leaving` 过渡

+ 条件渲染 （使用 v-if）
+ 条件展示 （使用 v-show）
+ 动态组件
+ 组件根节点

示例：

```
<div id="demo">
	<button v-on:click="show = !show">
		Toggle
	</button>
	<transition name="fade">
		<p v-if="show">hello</p>
	</transition>
</div>
```

```
.fade-enter-active, .fade-leave-active {
	transition: opacity .5s
}
.fade-enter, .fade-leave-active {
	opacity: 0
}
```

```
new Vue({
	el: '#demo',
	data: {
		show: true
    }
})
```

当插入或删除包含在 `transition` 组件中的元素时，Vue 将会做以下处理：

+ 自动嗅探目标元素是否应用了 CSS 过渡或动画，如果是，在恰当的时机添加/删除 CSS 类名。
+ 如果过渡组件提供了 JavaScript 钩子函数，这些钩子函数将在恰当的时机被调用。
+ 如果没有找到 JavaScript 钩子并且也没有检测到 CSS 过渡/动画，DOM 操作（插入/删除）在下一帧中立即执行。(注意：此指浏览器逐帧动画机制，与 Vue，和Vue的 nextTick 概念不同)

#### 过渡的-CSS-类名
会有 4 个(CSS)类名在 enter/leave 的过渡中切换

+ `v-enter:` 定义进入过渡的开始状态。在元素被插入时生效，在下一个帧移除。
+ `v-enter-active:` 定义进入过渡的结束状态。在元素被插入时生效，在 transition/animation 完成之后移除。
+ `v-leave:` 定义离开过渡的开始状态。在离开过渡被触发时生效，在下一个帧移除。
+ `v-leave-active:` 定义离开过渡的结束状态。在离开过渡被触发时生效，在 transition/animation 完成之后移除。

![transition](http://blogpic.at15cm.com/transition-1.png)

对于这些在 enter/leave 过渡中切换的类名，`v-` 是这些类名的前缀。使用 `<transition name="my-transition">` 可以重置前缀，比如 `v-enter` 替换为 `my-transition-enter`。

`v-enter-active` 和 `v-leave-active` 可以控制 `进入/离开` 过渡的不同阶段。

#### CSS过渡
常用的过渡都是使用 CSS 过渡。
示例：

```
<div id="example-1">
	<button @click="show = !show">
		Toggle render
	</button>
	<transition name="slide-fade">
		<p v-if="show">hello</p>
	</transition>
</div>
```

```
/* 可以设置不同的进入和离开动画 */
/* 设置持续时间和动画函数 */
.slide-fade-enter-active {
	transition: all .3s ease;
}
.slide-fade-leave-active {
	transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-active {
	transform: translateX(10px);
	opacity: 0;
}
```

```
new Vue({
	el: '#example-1',
	data: {
		show: true
	}
})
```

#### CSS 动画
CSS 动画用法同 CSS 过渡，区别是在动画中 v-enter 类名在节点插入 DOM 后不会立即删除，而是在 animationend 事件触发时删除。

示例： (省略了兼容性前缀)

```
<div id="example-2">
	<button @click="show = !show">Toggle show</button>
	<transition name="bounce">
		<p v-if="show">Look at me!</p>
	</transition>
</div>
```

```
.bounce-enter-active {
	animation: bounce-in .5s;
}
.bounce-leave-active {
	animation: bounce-out .5s;
}
@keyframes bounce-in {
	0% {
		transform: scale(0);
	}
	50% {
		transform: scale(1.5);
	}
	100% {
		transform: scale(1);
	}
}
@keyframes bounce-out {
	0% {
		transform: scale(1);
	}
	50% {
		transform: scale(1.5);
	}
	100% {
		transform: scale(0);
	}
}
```

```
new Vue({
	el: '#example-2',
	data: {
		show: true
	}
})
```

#### 自定义过渡类名
我们可以通过以下特性来自定义过渡类名：

+ enter-class
+ enter-active-class
+ leave-class
+ leave-active-class

他们的优先级高于普通的类名，这对于 Vue 的过渡系统和其他第三方 CSS 动画库，如 Animate.css 结合使用十分有用。

示例：

```
<link href="https://unpkg.com/animate.css@3.5.1/animate.min.css" rel="stylesheet" type="text/css">
<div id="example-3">
	<button @click="show = !show">
		Toggle render
	</button>
	<transition
		name="custom-classes-transition"
		enter-active-class="animated tada"
		leave-active-class="animated bounceOutRight"
	>
		<p v-if="show">hello</p>
	</transition>
</div>
```

```
new Vue({
	el: '#example-3',
	data: {
		show: true
	}
})
```

#### 同时使用-Transitions-和-Animations
Vue 为了知道过渡的完成，必须设置相应的事件监听器。它可以是 `transitionend` 或 `animationend` ，这取决于给元素应用的 CSS 规则。如果你使用其中任何一种，Vue 能自动识别类型并设置监听。

但是，在一些场景中，你需要给同一个元素同时设置两种过渡动效，比如 `animation` 很快的被触发并完成了，而 `transition` 效果还没结束。在这种情况中，你就需要使用 `type` 特性并设置 animation 或 transition 来明确声明你需要 Vue 监听的类型。


更多内容见 [Vue中文网--过渡效果](http://cn.vuejs.org/v2/guide/transitions.html)。

