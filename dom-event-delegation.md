---
title: DOM事件委托
date: 2020-05-23 00:05:20
tags: 前端
categories: 前端
---

首先提两个疑问：
1. 儿子被点击，算不算点击老子？
2. 那么先调用老子的函数还是先调用儿子的函数？

首先第一个问题，儿子被点击，肯定算点击了老子。对于第二个问题，要考虑实际情况，这就涉及到了捕获和冒泡。


## 捕获与冒泡
>事件流描述的是从页面中接受事件的顺序，分为冒泡流和捕获流


### 区别
+ `事件冒泡` 是指事件从 `最具体的元素` 接收，然后逐级向上传播，直到不具体的节点（通常指文档节点）；
+ `事件捕获` 相反，它是从 `不具体的节点` 开始，逐步到最具体的节点；

因此我们可以说，**冒泡先调用儿子的监听函数，捕获先调用老子的监听函数**。


### 事件模型
#### 标准事件模型
+ 三个阶段，即**捕获阶段=>目标阶段=>冒泡阶段**；
+ 注册事件：`addEventListener()` ；移除事件：`removeEventListener()`；
+ `e` 对象被传递给所有的监听函数。事件结束后，e 对象就不存在了；
+ 阻止冒泡：`e.stopPropagation()`；
+ 阻止默认事件：`e.preventDefault()`；
+ 若注册同一函数，与之同名的函数都会被忽略；

#### IE事件模型
+ 两个阶段，即**目标阶段和冒泡阶段**
+ 注册事件：`attachEvent()` ；移除事件：`detachEvent()`；
+ 获得event对象：`e = window.event`（可见IE中的event对象是个全局属性)；
+ 阻止冒泡：`e.cancelBubble = true;`；
+ 阻止默认事件：`e.returnValue=false`；
+ 同一函数被注册多次，发生次数等于注册次数；

注意：attachEvent注册的函数作为**全局调用函数**，而不是文档元素的方法，因此 `this` 引用的是 `window` 对象


### target VS currentTarget
+ e.target —— 用户操作的元素
+ e.currentTarget —— 程序员监听的元素

this 指的是 e.currentTarget，不推荐使用。

示例：
div > span {文字}， 用户点击文字，这里 e.currentTarget 就是 div， e.target 就是 span。



### 一个特例
假设只有一个div被监听（不考虑父子同时被监听），fn分别在捕获阶段和冒泡阶段监听 click 事件，用户点击的元素就是开发者监听的：

```javascript
div.addEventListener('click', f1)
div.addEventListener('click', f2, true)
```

 这里我们会想当然的认为标准事件流先捕获再冒泡，也就是先f2再f1，但是这里是先f1再f2。如果调换两行代码的位置，才是先f2再f1。

记住：这种情况下，**谁先监听谁先执行**。


### 不可取消冒泡
**有些事件不可取消冒泡**
#### scroll 事件![在这里插入图片描述](https://img-blog.csdnimg.cn/20200523215415312.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FydGh1cldhbmc3Nw==,size_16,color_FFFFFF,t_70)
+ Bubbles：该事件是否冒泡；
+ Cancelable：开发者是否可以取消冒泡

#### 插曲：如何阻止滚动
首先明确为什么 scroll 事件不能通过阻止scroll默认动作来阻止滚动，因为**先有滚动才有滚动事件**。

要阻止滚动，可阻止 `wheel` 和 `touchstart` 的默认动作，但是注意需要找准滚动条所在的元素。同时也要留意滚动条还能用，可以使用 CSS 让滚动条不显示：

```javascript
// 1. 首先阻止鼠标的滚轮事件wheel
const box = document.querySelector('#box');
box.addEventListener('wheel', (e)=>{
  e.preventDefault();
})
// 2. CSS隐藏滚动条
::-webkit-scrollbar{
  width: 0 !important;
}
// 3. 移动端阻止触摸滚动事件touchstart
box.addEventListener('touchstart', (e)=>{
  e.preventDefault();
})
```

也可使用 CSS `overflow: hidden;` 直接取消滚动条，但此时 JS 仍然可以修改 scrollTop


### 自定义事件
浏览器自带的事件有100多种，那么开发者怎么在这些时间之外自定义事件？

我们可以通过创建 `Event` 对象和 `CustomEvent` 对象来创建自定义对象，这里演示 CustomEvent：

```javascript
button.addEventListener('click', ()=>{
	const event = new CustomEvent('abc', {
		detail: {name: 'aw', age: 18},
		bubbles: true
	})
	button.dispatchEvent(event)
})
button.addEventListener('abc', (e)=>{
	console.log(e.detail)
})
```

### 事件委托
场景一：
要给100个按钮添加点击事件 —— 监听这100个按钮的祖先，等冒泡的时候判断target是不是这100个按钮中的一个。

场景二：
要监听目前不存才的元素的点击事件 —— 监听祖先，等电机的时候看看是不是想要监听的元素。

从这两个场景中我们可以看出事件委托的优点：
+ 省监听数，也就是**省内存**
+ 可以**监听动态元素**

示例一：

```html
<div id="box">
	<button data-id="1">click 1</button>
	<button data-id="2">click 2</button>
	<button>click 3</button>
	<button>click 4</button>
	<button>click 5</button>
	<button>click 6</button>
</div>
```

```javascript
const box = document.getElementById('box');
// 事件是监听在父级div上的
box.addEventListener('click', (e) => {
  const t = e.target
  if (t.tagName.toLowerCase() === 'button') {	// 判断点击对象是否为button
    console.log('button 点击了')
    console.log('按钮内容是' + t.textContent)	// 按钮内容是click 1
    console.log('id内容是' + t.dataset.id)	// id内容是1
  }
})
```


示例二：

```javascript
// 通过定时器模拟动态添加元素
setTimeout(() => {
  const btn = document.createElement('button')
  btn.textContent = 'click 1'
  box.appendChild(btn)
}, 1000);
// 事件是监听在父级div上的
box.addEventListener('click', (e) => {
  const t = e.target
  if (t.tagName.toLowerCase() === 'button') {
    console.log('button 点击了')
  }
})
```

### 封装事件委托
封装事件委托，思路是判断 `target` 是否是预期的：

```javascript
function _on(eventType, element, selectorString, fn) {
	if (!(element instanceof Element)) {
		element = document.querySelector(element)
	}
	element.addEventListener(eventType, (e)=>{
		const t = e.target
		if (t.matches(selectorString)) {
			fn(e)
		}
	})
}

// 上面的例子可以改写为：
_on('click', box, 'button', ()=>{
	console.log('button 点击了')
})
```

正常来说这样是可以的，但实际上正确答案是需要递归判断 target / target的爸爸 / target的爷爷：

```javascript
function _on(eventType, element, selectorString, fn) {
	if (!(element instanceof Element)) {
		element = document.querySelector(element)
	}
	element.addEventListener(eventType, (e)=>{
		let t = e.target
		// 递归向上判断
		while(!t.matches(selectorString)){
			// 一直到被监听的元素
			if(t === element) {
				t = null
				break
			}
			t = t.parentNode
		}
		t && fn.call(t, e)
	})
}
```