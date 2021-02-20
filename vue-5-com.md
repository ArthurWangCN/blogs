---
title: Vue学习笔记（五） 组件通信
date: 2017-04-28 19:23:06
tags: Vue
categories: 前端
---
>组件通信：`vm.$emit()` 和 `vm.$on()`

## 父组件和子组件

子组件想要拿到父组件数据（1.x）

```
<div id="box">
	father:{{a}}
	<child-com :msg.sync="a"></child-com>	//sync让父组件同步数据改变
</div>
<template id="child">
	<div>
		<h2>child component</h2>
		<input type="button" value="change" @click="change">
		<strong>{{msg}}</strong>
	</div>
</template>
```

```
new Vue({
	el:'#box',
	data:{
		a:'father data'
	},
	components:{
		'child-com':{
			props:['msg'],
			template:"#child",
			methods:{
				change:function(){
					this.msg="changed"
				}
			}
		}
	}
});
```


1.x 子组件可以更改父组件信息，可以是同步 `sync`, 2.x 不再允许直接给父级数据做赋值操作，解决：

+ 父组件每次传一个对象给子组件，对象之间引用

	```
	el:'#box',
	data:{
		dataMsg:{
			a:'father data2'
		}
	}
	```
	
	```
	<child-com :msg="dataMsg"></child-com>
	```
	
+ 只是为了不报错可以使用 `mounted` 中转

	```
	components:{
		'child-com':{
			data(){
				return {
					b:''
				}
			},
			props:['msg'],
			template:"#child",
			mounted(){
				this.b = this.msg;
			},
			methods:{
				change:function(){
					this.b="changed"
				}
			}
		}
	}
	```
	
	
## 单一事件管理组件通信
```
#大致流程:
var Event = new Vue();
Event.$emit(事件名称,数据) 
Event.$on(事件名称,function(){
	//data
}.bind(this))
```

示例：

```
<div id="box">
	<aaa></aaa>
	<bbb></bbb>
	<ccc></ccc>
</div>
```

```
<template id="ac">
	<div>
		<span>我是A组件->{{a}}</span>
		<input type="button" value="send" @click="send" />
	</div>
</template>
<template id="bc">
	<div>
		<span>我是B组件->{{a}}</span>
		<input type="button" value="send" @click="send" />
	</div>
</template>
<template id="cc">
	<div>
		<h2>我是C组件</h2>
		<div>A的数据：{{a}}</div>
		<div>B的数据：{{b}}</div>
	</div>
</template>
```

```
<script>
	//准备一个空的实例对象
	var Event = new Vue();
	var A = {
		template:'#ac',
		data(){
			return {
				a:'a data'
			}
		},
		methods:{
			send(){
				Event.$emit('a-msg',this.a)
			}
		}
	};
	var B = {
		template:'#bc',
		data(){
			return {
				a:'b data'
			}
		},
		methods:{
			send(){
				Event.$emit('b-msg',this.a)
			}
		}
	};
	var C = {
		template:"#cc",
		data(){
			return {
				a:'',
				b:''
			}
		},
		mounted(){
			Event.$on('a-msg',function(a){
				this.a = a
			}.bind(this));
			Event.$on('b-msg',function(a){
				this.b = a
			}.bind(this))
		}
	};
	new Vue({
		el:'#box',
		data:{			
		},
		components:{
			'aaa':A,
			'bbb':B,
			'ccc':C
		}
	});
</script>
```
