---
title: Vue学习笔记（五） 从 Vue1.x 到 Vue2.x
date: 2017-04-28 17:12:00
tags: Vue
categories: 前端
---
**1.组件模板不再支持片段代码**
组件中模板：

+ 之前：

	```
	<template>
		<h2>this is a component</h2><strong>strong</strong>
	</template>
	```
	
+ 现在：
	必须要有一个根元素包裹住所有代码
	
	```
	<template>
		<div>
			<h2>this is a component</h2><strong>strong</strong>
		</div>
	</template>
	```
	

**2.关于组件的定义**

+ 之前：
	`Vue.extend` 这种方式在2.0里面有，但是有所改动，即便是能用也不推荐使用

	在2.0能继续使用：
	
	```
	Vue.component(组件名称，{
		data(){}
		methods:{}
		template:
	})
	```

	
+ 现在：
	2.0退出一个组件，简洁定义方式：
	
	```
	var tem = {
		template:组件模板
	}
	Vue.component('aaa',tem);	
	```
	
	
**3.生命周期**
[生命周期](http://legendaryarthur.cn/2017/04/24/vue-2/)

+ 之前：

	```
	init
	created
	beforeCompile
	compiled
	ready
	beforeDestroy
	destroyed
	```
	
+ 现在：
	
	```
	beforeCreate	组件实例刚刚被创建，属性都没有
	created		    实例已经创建完成，属性已经绑定，但是DOM还没有生成
	beforeMount		模板编译之前
	mounted			模板编译完成，之前的 ready 变成了现在的 mounted
	beforeUpdate	组件更新之前
	updated			组件更新完毕
	beforeDestroy	组件销毁之前
	destroyed		组件销毁后
	```
	
![生命周期](http://blogpic.at15cm.com/vue-lifecycle.png)


**4.循环——重复添加数据**

+ 之前：
	多次点击按钮添加值会报错：

	```
	vue.js:1114 [Vue warn]: Duplicate value found in v-for="val in list": "d". Use track-by="$index" if you are expecting duplicate values.
	```


	```
	<ul>
		<input type="button" value="add" @click="add">
		<li v-for="val in list">{{val}}</li>
	</ul>
	<script>
		new Vue({
			el:'#box',
			data:{
				list:['a','b','c']
			}, 
			methods:{
				add:function () {
					this.list.push('d');
				}
			}
		});
	</script>
	```
	
	 在 `li` 标签上加上 `track-by="$index"` 可解决	 
+ 现在：
	2.0里面默认就可以添加重复数据
	

**5.循环——index**
	
+ 之前
	里面包含很多隐式变量如 `$index`,`$key`
	
	```
	<li v-for="val in list">{{$index}}+{{val}}</li>
	```
	
+ 现在：
	
	```
	<li v-for="(val,index) in list">{{index}}+{{val}}</li>
	```
	
>注意：1.0也可以使用类似2.0的方法，但是格式是 `(index,value)`，2.0使用 `(value,index)` 是因为作者更想贴近于原生JS -> `arr.forEach(function(item,index){})`

`key` 同理。


**6.循环——提升循环性能**

+ 之前：
	通过在 `li` 标签上加上 `track-by="$index"` 提高循环的性能
 
+ 现在：

	```
	<li v-for="(val,index) in arr" :key="index">
	```
	

**7.自定义键盘指令**

+ 之前：
	
	```
	Vue.directive('on').keyCodes.f1 = 112;
	```
	
+ 现在：
	
	```
	Vue.config.keyCodes.f1 = 112;
	```


**8.过滤器**

+ 之前：
	系统自带很多过滤器
	
	```
	{{msg|currency}}
	{{msg|json}}
	```

+ 现在：
	2.0内置过滤器全部删除了，让自己去实现
	作者推荐 `lodash` 工具库
	
	
**9.自定义过滤器**

+ 之前：
	
	```
	Vue.filter('toDouble',function(n){
		return n<9?'0'+n:n;
	});
	```
	
	```
	{{msg|toDouble}}
	```
	
	传参(空格)：
	
	```
	{{msg|toDouble '12' '5'}}
	```
	
+ 现在：
	
	传参：
					
	```
	{{msg|toDouble('12','5')}}
	```
	

参考：[从 Vue 1.x 迁移](http://cn.vuejs.org/v2/guide/migration.html)