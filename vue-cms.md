---
title: 一个基于 vue + elementUI 的后台管理系统
date: 2020-03-30 11:59:07
tags: [Vue]
categories: [前端, Vue]
---

## 项目概述

> 电商后台管理系统整体采用前后端分离的开发模式，其中前端项目是基于 Vue 技术栈的 SPA 项目。



技术选型：

+ Vue
+ Vue-router
+ Element-UI 
+ Axios
+ Echarts



## 登录/登出功能

### 登录功能

#### 登录实现

axios请求登录API

```javascript
const { data: res } = await this.$http.post('login', this.loginForm) 
if (res.meta.status !== 200) return this.$message.error('登录失败!') 
// 提示登录成功
this.$message.success('登录成功!')
// 把登录成功的token保存到sessionStorage 
window.sessionStorage.setItem('token', res.data.token) 
// 使用编程式导航，跳转到后台主页 
this.$router.push('/home')
```

#### 路由导航守卫控制访问权限

如果用户没有登录，但是直接通过 URL 访问特定页面，需要重新导航到登录页面。

[导航守卫官方文档]([https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E5%85%A8%E5%B1%80%E5%AE%88%E5%8D%AB](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#全局守卫))

```javascript
// 为路由对象，添加 beforeEach 导航守卫 
router.beforeEach((to, from, next) => {
	// 如果用户访问的登录页，直接放行
	if (to.path === '/login') return next()
	// 从 sessionStorage 中获取到 保存的 token 值
	const tokenStr = window.sessionStorage.getItem('token') 
	// 没有token，强制跳转到登录页
	if (!tokenStr) return next('/login')
	next()
})
```

#### 表单验证

```javascript
// 进行表单验证 this.$refs.loginFormRef.validate(async valid => {
// 如果验证失败，直接退出后续代码的执行
if (!valid) return
// 验证通过后这里完成登录成功后的相关操作(保存token、跳转到主页)
})
```

### 退出功能

基于 token 的方式实现退出比较简单，只需要销毁本地的 token 即可。这样，后续的请求就不会携带 token ， 必须重新登录生成一个新的 token 之后才可以访问页面。

```javascript
// 清空token 
window.sessionStorage.clear() 
// 跳转到登录页 
this.$router.push('/login')
```



## 主页

### 主页布局

先上下，后左右

```vue
<el-container>
	<!-- 头部区域 --> 
	<el-header></el-header> 
	<el-container>
		<!-- 侧边栏区域 --> 
		<el-aside></el-aside> 
		<!-- 右侧主体区域 --> 
		<el-main></el-main>
	</el-container> 
</el-container>
```

### 通过接口获取菜单数据

通过 axios 请求拦截器添加 token，保证拥有获取数据的权限。

```javascript
// axios请求拦截 
axios.interceptors.request.use(config => {
	// 为请求头对象，添加 Token 验证的 Authorization 字段 
	config.headers.Authorization = window.sessionStorage.getItem('token') 
	return config
})
```

### 动态渲染菜单数据并进行路由控制

+ 通过 v-for 双层循环分别进行一级菜单和二级菜单的渲染 
+ 通过路由相关属性(router)启用菜单的路由功能



## 用户管理

> 通过后台管理用户的账号信息，具体包括用户信息的展示、添加、修改、删除、角色分配、账号启用/注销等功能。

### 布局

+ 面包屑导航el-breadcrumb
+ Element-UI栅格系统基本使用el-row
+ 表格布局el-table、el-pagination

#### 用户状态列

作用域插槽

```vue
<template slot-scope="scope">
	<!-- 开关 -->
	<el-switch
		v-model="scope.row.mg_state"
		@change="stateChanged(scope.row.id, scope.row.mg_state)"
	></el-switch>
</template>
```

#### 数据分页

+ 当前页码:pagenum 
+ 每页条数:pagesize 
+ 记录总数:total
+ 页码变化事件
+ 每页条数变化事件
+ 分页条菜单控制

```vue
<el-pagination
	@size-change="handleSizeChange" 
	@current-change="handleCurrentChange" 
	:current-page="queryInfo.pagenum" 
	:page-sizes="[2, 3, 5, 10]" 
	:page-size="queryInfo.pagesize"
	layout="total, sizes, prev, pager, next, jumper" 
	:total="total"
>
</el-pagination>
```

#### 自定义表单验证规则（手机号）

```javascript
const checkMobile = (rule, value, cb) => {
	let reg = /^(0|86|17951)?(13[0-9]|15[012356789]|17[678]|18[0-9]|14[57])[0-9]{8}$/ 
  if (reg.test(value)) {
		cb()
	} else {
		cb(new Error('手机号码格式不正确')) }
	}
mobile: [
	{ required: true, message: '请输入手机号', trigger: 'blur' }, 
  { validator: checkMobile, trigger: 'blur' }
]
```



## 权限管理

> 通过权限管理模块控制不同的用户可以进行哪些操作，具体可以通过角色的方式进行控制，即每个用户分配 一个特定的角色，角色包括不同的功能权限。

### 角色权限分配

#### 展开行

通过 el-table-column 组件的 type =“expand” 方式实现表格行展开效果



## 分类管理

### 树形表格

[vue-table-with-tree-grid](https://github.com/MisterTaki/vue-table-with-tree-grid)

```bash
npm i vue-table-with-tree-grid -S
```



```javascript
import Vue from 'vue'
import ZkTable from 'vue-table-with-tree-grid' 
Vue.use(ZkTable)
```



## 商品管理

### 添加商品

#### 商品详情

富文本编辑器：

```bash
// 安装vue-quill-editor
npm install vue-quill-editor -S
```

```javascript
import VueQuillEditor from 'vue-quill-editor'
Vue.use(VueQuillEditor)
```

```vue
<quill-editor v-model="addForm.goods_introduce"></quill-editor>
```



## 数据统计

### 安装echarts

```bash
// 安装echarts库
npm install echarts -S
```

```javascript
// 导入echarts接口
import echarts from 'echarts'
```

### 绘制

```javascript
// 基于准备好的dom，初始化echarts实例
var myChart = echarts.init(this.$refs.main)
const { data: res } = await this.$http.get('reports/type/1')
if (res.meta.status !== 200) return this.$message.error('初始化折线图失败!') 
const data = _.merge(res.data, this.options)
// 绘制图表
myChart.setOption(data)
```

