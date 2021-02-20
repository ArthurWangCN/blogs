---
title: Vue学习笔记（六） Vue2.0过渡效果
date: 2017-04-29 09:47:33
tags: Vue
categories: 前端
---
>过渡效果（动画）在 2.0 里还叫 `transition`，只不过 1.0 里是通过属性来实现，到了 2.0 里由组件的形式来实现。

**格式：**

```
<transition>
	运动东西（元素、属性、路由...）
</transition>
```


**class定义：**

`.v-enter{}` 初始状态
`.v-enter-active{}` 变成什么样 -> 当元素出来（显示）
`.v-leave{}` 
`.v-leave-active{}` 变成什么样 -> 当元素离开（消失）


示例：

```
<div id="box">
	<input type="button" value="toggle" @click="show=!show">
	<transition name="toggle">
		<div class="box" v-show="show"></div>
	</transition>
</div>
```

```
.box{
	width: 200px;
	height: 200px;
	background-color: aqua;
}		
.toggle-enter-active,.toggle-leave-active{
	transition: 1s all ease;
}
.toggle-enter-active{
	width: 200px;
	height: 200px;
	opacity: 1;
}
.toggle-enter,.toggle-leave-active{
	width: 100px;
	height: 100px;
	opacity: 0;
}
```

```
new Vue({
	el:'#box',
	data:{
		show:false
	},
});
```

**可以在属性中声明 JavaScript 钩子:**

```
<transition
  v-on:before-enter="beforeEnter"		//动画enter之前
  v-on:enter="enter"					//动画enter
  v-on:after-enter="afterEnter"			//动画enter之后
  v-on:enter-cancelled="enterCancelled"
  v-on:before-leave="beforeLeave"		//动画leave之前
  v-on:leave="leave"					//动画leave
  v-on:after-leave="afterLeave"			//动画leave之后
  v-on:leave-cancelled="leaveCancelled"
>
</transition>
```


**如何配合 animate.css 使用？**
animate.css [下载地址](https://raw.githubusercontent.com/daneden/animate.css/master/animate.css)

示例：

```
<transition 
    enter-active-class="bounceInLeft" 
    leave-active-class="bounceOutRight"
>
  <div class="box animated" v-show="show"></div>
</transition>
```

>如果想要运动多个元素，需要使用 `<transition-group>`，并且给每一子元素加上 `key` 值

示例:

```
<transition-group
	enter-active-class="bounceInLeft" 
    leave-active-class="bounceOutRight"
>
  <p :key="1"></p>
  <p :key="2"></p>
</transition-group>
```


**详情参考: **

+ [Vue学习笔记（三） 过渡效果](http://legendaryarthur.cn/2017/04/24/vue-3-transition/)
+ [Vue.js中文网--过渡效果](http://cn.vuejs.org/v2/guide/transitions.html) 

