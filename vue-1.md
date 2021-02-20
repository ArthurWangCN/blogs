---
title: Vue学习笔记（一）
date: 2017-02-21 17:12:54
tags: Vue
categories: 前端
---
讲师：[Strive](https://ke.qq.com/teacher/1023135381)

### Vue是什么
是一个 `mvvm` 框架（库），和 `Angular` 类似；
比较容易上手、小巧。

[Vue官网](http://cn.vuejs.org/)
[API](http://cn.vuejs.org/v2/api/)

#### vue 和 angular 的区别：

| vue        | angular           | 
|:-------------:|:-------------:|
| 简单、易学      | 上手难 |
| 指令以 `v-xxx 开头`      | 指令以 `ng-xxx` 开头      |
| 一片 html 代码配合上 json，再 new 出来 vue 实例 | 所有属性和方法都挂到 $scope 身上|
| 个人维护项目 |由 google 维护|
| 适合移动端项目，小巧 | 适合PC端项目 |

#### vue和angular相同点：
不兼容低版本IE

*****

### vue基本雏形
`angular` 展示一条基本数据(伪代码)：
```
var app = angular.module('app',[]);
app.controller('xxx',function($scope){
	$scope.msg = 'welcome'
})

html:
div ng-controller = "xxx"
{{msg}}
```

`vue` 展示一条基本数据(伪代码)：
```
<div id="box">{{msg}}</div>	//一块html代码

//new出一个对象实例
new Vue（{
	el:'#box',		//选择器
	data:{
		msg:'welcome vue'
	}
}）
```

### 常用指令
指令：扩展html标签功能、属性

+ v-model:一般表单元素(input)，双向数据绑定
+ v-for:循环

注意：`vue2.0` 相对与之前版本丢弃 `$index` 和 `$key`，新数组语法：

```
<tr v-for="list in lists">{{list}}</tr>
或者
<tr v-for="（list,index）in lists">{{index}}</tr>
```
详情：[[译] Vue 2.0 的变化（二）之其他重大更改](https://segmentfault.com/a/1190000007018605)

+ v-on:click: 点击事件
>扩展：事件绑定用 `v-on:`

```
//方法中的this
var c = new Vue({
	el:"#box",
	data:{
		str:'almighty vue'
	}
	methods:{
		add:function(){
			alert(this.str);	//这里的this就是new出来的vue实例c
		}
	}
})
```

+ v-show="true/false": 显示隐藏

***

bootstrap+vue简易留言板(todolist):
>`bootstrap` : css框架，和 jqueryMobile一样，只需要给标签赋class和role，依赖jquery。

效果：

![简易留言板](http://blogpic.at15cm.com/todolist1.png)

![简易留言板](http://blogpic.at15cm.com/todolist2.png)


******

### 事件深入
v-on:click 简写: `@click=""`

**事件对象**
`@click="show($event)"`

**事件冒泡**
阻止冒泡: ① `ev.cancelBubble=true;` ② `@click.stop`

**默认事件**
阻止默认行为: ① `ev.preventDefault();` ② `@contextmenu.prevent`

**键盘类事件**
@keydown $event keyCode
@keyup

**常用键：**
回车 `@keydown.13` 或 `@keydown.enter`
上键 `@keydown.up`
下键 `@keydown.down`
...

### 属性
`v-bind:src=""` 简写: `:src=""`

>课程冲突:`<img src="{{url}}">` 报错，不会显示出图片，需用标准写法
![属性需使用标准写法](http://blogpic.at15cm.com/bug-src.png)

**特殊：**class和style
`class用法:`
① :class="[red]"  ->red是数据
② :class="{red:true}"	->red是类名
③ :class="json" 	->对应data中的json

`style用法：`
:style="[c]" 	->c对应json
*注意：*复合样式采用驼峰样式

*****

### 模板
![](http://blogpic.at15cm.com/vue-bind1.png)

>注意：单次绑定已经被新的 `v-once` 取代;
HTML的计算插值已经移除，取代的是 `v-html` 指令.

### 过滤器
过滤模板数据，系统提供一些过滤器
![](http://blogpic.at15cm.com/vue-filter1.png )

**注意：**
内置文本过滤器移除：
`text[0].toUpperCase() + text.slice(1)` 替换 `capitalize` 过滤器;
`text.toUpperCase()` 替换 `uppercase` 过滤器
`text.toLowerCase()` 替换 `lowercase` 过滤器



>课程冲突：不允许把Vue挂载到 `<html>` 或 `<body>` 上，应挂载到普通元素上
![不允许把Vue挂载到html或body上](http://blogpic.at15cm.com/bug-mount.png)


### 交互
vue想做交互要引入 [`vue-resource`](https://github.com/pagekit/vue-resource)
方式：

**get:**

**1.获取一个普通文本数据：**
```
this.$http.get('a.txt').then(
	function(res) {
		console.log(res.data);
	},function(res){
		console.log('res.status')
	});
```
>注意：这里浏览器可能会报如下图所示的跨域错误
![跨域错误](http://blogpic.at15cm.com/%E8%B7%A8%E5%9F%9F%E9%97%AE%E9%A2%98.png)

解决：[在Mac OS下开发html5+JS Chrome 浏览器 跨域 和 安全访问问题](http://blog.csdn.net/justinjing0612/article/details/9532953)，我直接把文件传到了七牛上，通过http协议传输的当然不会出现跨域问题啦。

**2.给服务器发送数据：**
```
this.$http.get('get.php',{
	a:1,
	b:2
}).then(
	function(res) {
		alert(res.data);
},function(res){
		alert(res.status);
});
```

**post:**
```
this.$http.post('post.php',{
	a:1,
	b:2
},{
	emulateJSON:true
}).then(
	function(res) {
		alert(res.data);
},function(res){
		alert(res.status);
});
```

**jsonp:**
`好搜`接口：
	https://sug.so.360.cn/suggest?callback=suggest_so&word=a

```
this.$http.jsonp('https://sug.so.360.cn/suggest',{
	word:'a'
}).then(
	function(res) {
		alert(res.data.s);
},function(res){
		alert(res.status);
});
```

`百度`接口：
	https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd=a&cb=show
	
```
get:function(){
	this.$http.jsonp('https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su',{
		wd:'a'
	},{
		jsonp:'cb'
	}).then(
		function(res) {
			console.log(res.data.s);
		},function(res){
			alert(res.status)
	});
}
```
