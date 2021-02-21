---
title: Vue Q&A
date: 2020-03-31 21:34:47
tags: Vue
categories: [前端, Vue]
---

**1、跟keep-alive有关的生命周期是哪些？描述下这些生命周期**

activated和deactivated两个生命周期函数

1.activated：当组件激活时，钩子触发的顺序是created->mounted->activated

2.deactivated: 组件停用时会触发deactivated，当再次前进或者后退的时候只触发activated

**2、vue怎么实现强制刷新组件？**

+ v-if：当v-if的值发生变化时，组件都会被重新渲染一遍。因此，利用v-if指令的特性，可以达到强制
+ `this.$forceUpdate()`

**3、vue如何优化首页的加载速度？**

① 第三方js库按CDN引入（一、cdn引入 二、去掉第三方库引入的import 三、把第三方库的js文件从打包文件里去掉）

② vue-router路由懒加载

③ 压缩图片资源

④ 静态文件本地缓存

⑤ 服务器端SSR渲染

**4、vue中data的属性可以和methods中的方法同名吗？为什么？**

不可以。因为，Vue会把methods和data的东西，全部代理到Vue生成的对象中，会产生覆盖所以最好不要同名。`[Vue warn]: Method "msg" has already been defined as a data property.`

**5、怎么给vue定义全局的方法？**

vue.prototype.方法名

**6、Vue 2.0 不再支持在 v-html 中使用过滤器怎么办？**

①全局方法（推荐）

`Vue.prototype.msg = function(msg) { return xxx }`

②computed方法

③$options.filters(推荐)

```javascript
filters: {
	msg: function(msg) {
		return msg.replace(/\n/g, "<br>")
	}
},
data: {
	content: 'xxx'
}
<div v-html="$options.filters.msg(content)"></div>
```

**7、怎么解决vue打包后静态资源图片失效的问题？**

答：将静态资源的存放位置放在src目录下

**8、vue中怎么重置data？**

`Object.assign()`

`Object.assign()`方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。

```javascript
var o1 = { a: 1 };
var o2 = { b: 2 };
var o3 = { c: 3 };
var obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1); // { a: 1, b: 2, c: 3 }, 注意目标对象自身也会改变。
```

注意，具有相同属性的对象，同名属性，后边的会覆盖前边的。

由于 `Object.assign()` 有上述特性，所以我们在Vue中可以这样使用：

Vue组件可能会有这样的需求：在某种情况下，需要重置Vue组件的data数据。此时，我们可以通过this.获取当前状态下的，通过options.data()获取该组件初始状态下的data。

然后只要使用`Object.assign(this.options.data())`就可以将当前状态的data重置为初始状态。

**9、页面中定义一个定时器，在哪个阶段清除？**

答案：在 **beforeDestroy** 中销毁定时器。

①为什么销毁它：

在页面a中写了一个定时器，比如每隔一秒钟打印一次1，当我点击按钮进入页面b的时候，会发现定时器依然在执行，这是非常消耗性能的。

②解决方案1：

```javascript
mounted(){
 this.timer = setInterval(()=>{
    console.log(1)
 },1000)
},
beforeDestroy(){
 clearInterval(this.timer)
}
```

方案1有两点不好的地方，引用尤大的话来说就是：

它需要在这个组件实例中保存这个 timer，如果可以的话最好只有生命周期钩子可以访问到它。这并不算严重的问题，但是它可以被视为杂物。

我们的建立代码独立于我们的清理代码，这使得我们比较难于程序化的清理我们建立的所有东西。

方案2（**推荐**）：该方法是通过$once这个事件侦听器在定义完定时器之后的位置来清除定时器

```text
mounted(){
 const timer = setInterval(()=>{
    console.log(1)
 },1000)
 this.$once('hook:beforeDestroy',()=>{
  clearInterval(timer)
 })
}
```

**10、父组件如何获取子组件的数据，子组件如何获取父组件的数据，父子组件如何传值？**

$children  $refs  $emit

$parent props

**③** **inheritAttrs**

这是@2.4新增的属性和接口。inheritAttrs属性控制子组件html属性上是否显示父组件的提供的属性。

```vue
//父组件
<HelloWorld ref="hello" :desc="desc" :keysword="keysword" :message="message"></HelloWorld>
```

实际情况，我们只需要message，那其他两个属性则会被当做普通的html元素插在子组件的根元素上。

这样做会使组件预期功能变得模糊不清，这个时候，在子组件中写入，**inheritAttrs：false** ，这些没用到的属性便会被去掉，true的话，就会显示。

如果，父组件中没被需要的属性，跟子组件本来的属性冲突的时候，则依据父组件。

上述这些没被用到的属性，如何被获取呢？这就用到了**$attrs**

**③** **$attrs**

作用：可以获取到没有使用的注册属性，如果需要，我们在这也可以**往下继续**传递。

就上上述没有被用到的desc和keysword就能通过$attrs获取到。

通过$attrs的这个特性可以父组件传递到孙组件，免除父组件传递到子组件，再从子组件传递到孙组件的麻烦

可以看出通过 **v-bind=”$attrs”**将数据传到孙组件中

除了以上，**provide / inject** 也适用于 隔代组件通信，尤其是获取祖先组件的数据，非常方便。

④ provide inject

```vue
<template>
  <div>
		<childOne></childOne>
  </div>
</template>
<script>
  import childOne from '../components/test/ChildOne'
  export default {
    name: "Parent",
    provide: {
      for: "demo"
    },
    components:{
      childOne
    }
  }
```

在这里我们在父组件中provide for这个变量，然后直接设置三个组件（childOne、childTwo 、childThird）并且一层层不断内嵌其中， 而在最深层的childThird组件中我们可以通过inject获取for这个变量

```vue
<template>
  <div>
    {{demo}}
  </div>
</template>
<script>
  export default {
    name: "",
    inject: ['for'],
    data() {
      return {
        demo: this.for
      }
    }
  }
</script>
```

**11、自定义指令如何定义，它的生命周期是什么？**

通过 `Vue.directive()` 来定义全局指令

有几个可用的钩子（生命周期）, 每个钩子可以选择一些参数. 钩子如下:

**bind**: 一旦指令附加到元素时触发

**inserted**: 一旦元素被添加到父元素时触发

**update**: 每当元素本身更新(但是子元素还未更新)时触发

**componentUpdate**: 每当组件和子组件被更新时触发

**unbind**: 一旦指令被移除时触发。

bind和update也许是这五个里面最有用的两个钩子了

每个钩子都有el, binding, 和vnode参数可用.

update和componentUpdated钩子还暴露了oldVnode, 以区分传递的旧值和较新的值.

el就是所绑定的元素.

binding是一个保护传入钩子的参数的对象. 有很多可用的参数, 包括name, value, oldValue, expression, arguments, arg及修饰语.

vnode有一个更不寻常的用例, 它可用于你需要直接引用到虚拟DOM中的节点.

binding和vnode都应该被视为只读.

**12、vue生命周期，各个阶段简单讲一下？**

**breforeCreate（）：实例创建前**，这个阶段实例的data和methods是**读不到**的。

**created（）：实例创建后**，这个阶段已经完成数据观测，**属性**和**方法**的运算，**watch**/event事件回调，mount挂载阶段**还没有开始**。$el属性目前不可见，数据并没有在DOM元素上进行渲染。

**created**完成之后，进行template编译等操作，将template编译为render函数，有了render函数后才会执行beforeMount（）

**beforeMount（）：在挂载开始之前**被调用：相关的 render 函数首次被调用

**mounted（）：挂载之后调用**，el选项的DOM节点被新创建的 vm.$el 替换，并挂载到实例上去之后调用此生命周期函数，此时实例的数据在DOM节点上进行渲染

后续的钩子函数执行的过程都是需要外部的触发才会执行

有数据的变化，会调用**beforeUpdate**，然后经过Virtual Dom，最后**updated**更新完毕，当组件被销毁的时候，会调用**beforeDestory**，以及**destoryed**。

**13、watch 和 computed的区别？**

computed：

①有**缓存**机制；②不能接受参数；③可以依赖其他computed，甚至是其他组件的data；④不能与data中的属性重复

watch：

①可接受两个参数；②监听时可触发一个**回调**，并做一些事情；③监听的属性必须是存在的；④允许**异步**

watch配置：handler、deep（是否深度）、immeditate （是否立即执行）

总结：

当有一些数据需要随着另外一些数据变化时，建议使用computed

当有一个通用的响应数据变化的时候，要执行一些业务逻辑或异步操作的时候建议使用watch

---

**computed：** 是计算属性，依赖其它属性值，并且 computed 的值有缓存，只有它依赖的属性值发生改变，下一次获取 computed 的值时才会重新计算 computed 的值；
**watch：** 更多的是「观察」的作用，类似于某些数据的监听回调 ，每当监听的数据变化时都会执行回调进行后续操作；
**运用场景：**

- 当我们需要进行数值计算，并且依赖于其它数据时，应该使用 computed，因为可以利用 computed 的缓存特性，避免每次获取值时，都要重新计算；
- 当我们需要在数据变化时执行异步或开销较大的操作时，应该使用 watch，使用 watch 选项允许我们执行异步操作 ( 访问一个 API )，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这些都是计算属性无法做到的。

**14、computed中的getter和setter**

① computed 中可以分成 getter（读取） 和 setter（设值）

② 一般情况下是没有 setter 的，computed 预设只有 getter ，也就是只能读取，不能改变设值。

注意：**不是说我们更改了getter里使用的变量，就会触发computed的更新，前提是computed里的值必须要在模板里使用才行**。如果将{{fullName}}去掉，get方法是不会触发的。

注意：**并不是触发了setter也就会触发getter，他们两个是相互独立的。我们这里修改了fullName会触发getter是因为setter函数里有改变firstName 和 lastName 值的代码，这两个值改变了，fullName依赖于这两个值，所以便会自动改变。**

**15、导航钩子**

①全局导航守卫

前置守卫

```js
router.beforeEach((to, from, next) => {
	//do something
})
```

后置钩子（没有next参数）

```javascript
router.afterEach((to, from) => { 
	//do something 
})
```

②路由独享守卫

```javascript
cont router = new  VueRouter({
 routes: [
  {
    path: '/file',
    component: File,
    beforeEnter: (to, from ,next) => {
       // do someting
    }
   }
 ]
});
```

③ 组件内的导航钩子

组件内的导航钩子主要有这三种：beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave。他们是直接在路由组件内部直接进行定义的

**beforeRouteEnter**

注：beforeRouteEnter 不能获取组件实例 this，因为当守卫执行前，组件实例被没有被创建出来，我们可以通过给 next 传入一个**回调**来访问组件实例。在导航被确认时，会执行这个回调，这时就可以访问组件实例了

仅仅是 beforRouteEnter 支持给 next 传递回调，其他两个并不支持，因为剩下两个钩子可以正常获取组件实例 this

**16、如何通过路由将数据传入下一个跳转的页面呢？**

答： params 和 query

params

```javascript
传参
this.$router.push({
 name:"detail",
 params:{
   name:'xiaoming',
 }
});
接受
this.$route.params.name
```

query

```javascript
传参
this.$router.push({
  path:'/detail',
  query:{
    name:"xiaoming"
  }
 })
接受 //接收参数是this.$route
this.$route.query.id  
```

**那query和params什么区别呢？**

① params只能用**name**来引入路由，query既可以用name又可以用path（通常用path）

② params类似于post方法，参数不会再地址栏中显示；query类似于get请求，页面跳转的时候，可以在地址栏看到请求参数

**17、this.$router 和this.$route有何区别？**

$router为VueRouter**实例**，想要导航到不同URL，则使用$router.push方法

$route为当前router跳转对象，里面可以获取name、path、query、params等

**18、es6 的特有的类型， 常用的操作数组的方法都有哪些？**

es6新增的主要的特性：

① let const 两者都有块级作用域

② 箭头函数

③ 模板字符串

④ 解构赋值

⑤ for of循环

⑥ import 、export 导入导出

⑦ set数据结构

⑧ ...展开运算符

⑨ 修饰器 @

⑩ class类继承

⑪ async、await

⑫ promise

⑬ Symbol

⑭ Proxy代理

操作数组常用的方法：

es5：concat 、join 、push、pop、shift、unshift、slice、splice、substring和substr 、sort、 reverse、indexOf和lastIndexOf 、every、some、filter、map、forEach、reduce

es6：find、findIndex、fill、copyWithin、Array.from、Array.of、entries、values、key、includes

**19、vue双向绑定原理？**

通过 `Object.defineProperty()` 来劫持各个属性的setter,getter，在数据变动时发布消息给订阅者，触发相应的监听回调

**20、vue-router的实现原理，history和hash模式有什么区别？**

vue-router有两种模式，hash模式和history模式

**hash模式**

url中带有#的便是hash模式，#后面是hash值，它的变化会触发**hashchange** 这个事件。

通过这个事件我们就可以知道 hash 值发生了哪些变化。然后我们便可以监听hashchange来实现更新页面部分内容的操作：

另外，hash 值的变化，并不会导致浏览器向服务器发出请求，浏览器不发出请求，也就不会刷新页面。

**history模式**

history api可以分为两大部分，切换和修改

① 切换历史状态

包括back,forward,go三个方法，对应浏览器的前进，后退，跳转操作

```text
history.go(-2);//后退两次
history.go(2);//前进两次
history.back(); //后退
hsitory.forward(); //前进
```

② 修改历史状态

包括了**pushState**,**replaceState**两个方法,这两个方法接收三个参数:stateObj,title,url

```text
history.pushState({color:'red'}, 'red', 'red'})
window.onpopstate = function(event){
  console.log(event.state)
  if(event.state && event.state.color === 'red'){
    document.body.style.color = 'red';
  }
}
history.back();
history.forward();
```

通过pushstate把页面的状态保存在state对象中，当页面的url再变回这个url时，可以通过event.state取到这个state对象，从而可以对页面状态进行还原，这里的页面状态就是页面字体颜色，其实滚动条的位置，阅读进度，组件的开关的这些页面状态都可以存储到state的里面。

history缺点：

1：hash 模式下，仅hash符号之前的内容会被包含在请求中，如[http://www.a12c.com](https://link.zhihu.com/?target=http%3A//www.a12c.com),因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回404错误。

2：history模式下，前端的URL必须和实际向后端发起请求的URL一致。如[http://www.a12c.com/book/a](https://link.zhihu.com/?target=http%3A//www.a12c.com/book/a)。如果后端缺少对/book/a 的路由处理，将返回404错误

**21、vue能监听到数组变化的方法有哪些？为什么这些方法能监听到呢？**

Vue.js观察数组变化主要通过以下7个方法（push、pop、shift、unshift、splice、sort、reverse）

大家知道，通过Object.defineProperty()劫持数组为其设置getter和setter后，调用的数组的push、splice、pop等方法改变数组元素时并不会触发数组的setter，继而数组的数据变化并不是响应式的，但是vue实际开发中却是实时响应的，是因为vue重写了数组的push、splice、pop等方法

从源码中可以看出，ob.dep.notify()将当前数组的变更通知给其订阅者，这样当使用重写后方法改变数组后，数组订阅者会将这边变化更新到页面中

**22、怎么缓存当前组件，怎么更新**

keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，避免重新渲染 ，其有以下特性：

- 一般结合路由和动态组件一起使用，用于缓存组件；
- 提供 include 和 exclude 属性，两者都支持字符串或正则表达式， include 表示只有名称匹配的组件会被缓存，exclude 表示任何名称匹配的组件都不会被缓存 ，其中 exclude 的优先级比 include 高；
- 对应两个钩子函数 activated 和 deactivated ，当组件被激活时，触发钩子函数 activated，当组件被移除时，触发钩子函数 deactivated。

```vue
<keep-alive>
    <router-view></router-view>
</keep-alive>
<!-- 这里是需要keepalive的 -->
<keep-alive>
    <router-view v-if="$route.meta.keepAlive"></router-view>
</keep-alive>
<!-- 这里不会被keepalive -->
<router-view v-if="!$route.meta.keepAlive"></router-view>
{
  path: '',
  name: '',
  component: ,
  meta: {keepAlive: true} // 这个是需要keepalive的
},
{
  path: '',
  name: '',
  component: ,
  meta: {keepAlive: false} // 这是不会被keepalive的
}
```

如果缓存的组件想要清空数据或者执行初始化方法，在加载组件的时候调用activated钩子函数，如下：

```javascript
activated: function () {
    this.data = '';
}
```

**23、axios怎么解决跨域？**

使用axios直接进行跨域访问不可行，我们需要配置代理

代理可以解决的原因：

因为客户端请求服务端的数据是存在跨域问题的，而**服务器和服务器之间可以相互请求数据，是没有跨域的概念**（如果服务器没有设置禁止跨域的权限问题），也就是说，我们可以配置一个代理的服务器可以请求另一个服务器中的数据，然后把请求出来的数据返回到我们的代理服务器中，代理服务器再返回数据给我们的客户端，这样我们就可以实现跨域访问数据

1.配置BaseUrl

```javascript
import axios from 'axios'
Vue.prototype.$axios = axios
axios.defaults.baseURL = '/api'  //关键代码
```

2.配置代理

在config文件夹下的index.js文件中的proxyTable字段中，作如下处理：

```javascript
proxyTable: {
 '/api': {
   target:'http://api.douban.com/v2', // 你请求的第三方接口
   changeOrigin:true, 
// 在本地会创建一个虚拟服务端，然后发送请求的数据，并同时接收请求的数据，
//这样服务端和服务端进行数据的交互就不会有跨域问题
   pathRewrite:{  // 路径重写，
    '^/api': ''  
// 替换target中的请求地址，也就是说以后你在请求http://api.douban.com/v2/XXXXX
//这个地址的时候直接写成/api即可。
   }
  }
}
```

在具体使用axios的地方，修改url如下即可

```text
axios.get("/movie/top250").then((res) => {
  res = res.data
  if (res.errno === ERR_OK) {
    this.themeList=res.data;
  }
 }).catch((error) => {
  console.warn(error)
})
```

原理：

因为我们给url加上了前缀/api，我们访问/movie/top250就当于访问了：localhost:8080/api/movie/top250（其中localhost:8080是默认的IP和端口）。

在index.js中的proxyTable中拦截了/api,**并把/api及其前面的所有**替换成了target中的内容，因此实际访问Url是[http://api.douban.com/v2/movie/top250](https://link.zhihu.com/?target=http%3A//api.douban.com/v2/movie/top250)。

至此，纯前端配置代理解决axios跨域得到解决

**24、怎么实现路由懒加载**

第一种（最常用）：

```javascript
const Foo = () => import('./Foo.vue')
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
```

第二种：

```javascript
const router = new Router({
  routes: [
   {
     path: '/index',
     component: (resolve) => {
        require(['../components/index'], resolve) // 这里是你的模块 不用import去引入了
     }
    }
  ]
})
```

第三种require.ensure（官方推荐）：

```javascript
// r就是resolve
const list = r => require.ensure([], () => r(require('../components/list/list')), 'list');
// 路由也是正常的写法  这种是官方推荐的写的 按模块划分懒加载 
const router = new Router({
  routes: [
  {
    path: '/list/blog',
    component: list,
    name: 'blog'
  }
 ]
})
```

**25、动态路由**



**26、切换路由，滚动到顶部或者原先位置**

当创建一个 Router 实例，可以提供一个 **scrollBehavior** 方法：

```javascript
注意: 这个功能只在 HTML5 history 模式下可用。
const router = new VueRouter({
 routes: [...],
 scrollBehavior (to, from, savedPosition) {
  // return 期望滚动到哪个的位置
 }
})
```

scrollBehavior 方法接收 to 和 from 路由对象。第三个参数 savedPosition 当且仅当 popstate 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。

```javascript
scrollBehavior (to, from, savedPosition) {
 return { x: 0, y: 0 }
}
对于所有路由导航，简单地让页面滚动到顶部。
```



```javascript
返回 savedPosition，在按下 后退/前进 按钮时，在滚动条位置，就会像浏览器的原生表现那样：
scrollBehavior (to, from, savedPosition) {
 if (savedPosition) {
  return savedPosition
 } else {
  return { x: 0, y: 0 }
 }
}
```

还可以在main.js入口文件配合vue-router写这个

```javascript
router.afterEach((to,from,next) => {
  window.scrollTo(0,0);
});
```

**27、vue-router响应参数变化**

当使用路由参数时，比如：

```text
{path:’/list/:id’component:Foo}
```

从 /list/aside导航到 /list/foo，原来的组件实例会被复用。

因为两个路由都渲染同个组件Foo，比起销毁再创建，复用则更加高效。

不过，这也意味着组件的**生命周期钩子不会再被调用**。

如果跳转到相同的路由还会报以下错误:

```javascript
"Navigating to current location is not allowed"
```

这个时候我们需要重写push方法，在src/router/index.js 里面import VueRouter from 'vue-router'下面写入下面方法即可

```javascript
const routerPush = VueRouter.prototype.push
VueRouter.prototype.push = function push(location) {
  return routerPush.call(this, location).catch(error=> error)
}
```

**如何响应不同的数据呢？**

① 复用组件时，想对路由参数的变化作出响应的话，你可以简单地 **watch (监测变化) $route 对象**：

```javascript
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}
```

② 使用**beforeRouteUpdate**

```javascript
const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
    // react to route changes...
    // don't forget to call next()
  }
}
```

注意：

（1）从同一个组件跳转到同一个组件。

（2）生命周期钩子created和mounted都不会调用。

**28、vue模板中为什么以_、$开始的变量无法渲染？**

名字以 _ 或 $开始的属性不会被 Vue 实例代理，因为它们可能与 Vue 的内置属性与 API 方法冲突。用 vm.$data._property 访问它们。

**29、vue中，如何监听一个对象内部的变化？**

方法①：对整个obj深层监听

```javascript
watch:{
 obj:{
  handler(newValue,oldValue){
   console.log('obj changed')
  },
  deep: true,//深度遍历
  immediate: true 
//默认第一次绑定的时候不会触发watch监听，值为true时可以在最初绑定的时候执行
 }
}
```

方法② ：指定key

```javascript
watch: {
    "dataobj.name": {
      handler(newValue, oldValue) {
        console.log("obj changed");
      }
    }
  }
```

方法③：computed

```javascript
computed(){
 ar(){
  return this.obj.name
 }
}
```

**30、v-for循环时为什么要加key？**

key的作用主要是为了高效的更新虚拟DOM，是因为Virtual DOM 使用Diff算法实现的原因。

当某一层有很多相同的节点时，也就是列表节点时，Diff算法的更新过程默认情况下也是遵循以上原则。

所以我们需要使用key来给每个节点做一个唯一标识，Diff算法就可以正确的识别此节点，找到正确的位置区插入新的节点。

**31、$nextTick用过吗，有什么作用？**

在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

解决的问题：**有些时候在改变数据后立即要对dom进行操作**，此时获取到的dom仍是获取到的是数据刷新前的dom，无法满足需要，这个时候就用到了$nextTick。

**32、vue和react的区别是什么？**

① React严格上只针对MVC的view层,Vue则是MVVM模式

② virtual DOM不一样,vue会跟踪每一个组件的依赖关系,不需要重新渲染整个组件树.而对于React而言,每当应用的状态被改变时,全部组件都会重新渲染,所以react中会需要shouldComponentUpdate这个生命周期函数方法来进行控制

③ 组件写法不一样, React推荐的做法是 JSX + inline style, 也就是把HTML和CSS全都写进JavaScript了,即'all in js'; Vue推荐的做法是webpack+vue-loader的单文件组件格式,即html,css,jd写在同一个文件;

④ 数据绑定: vue实现了数据的双向绑定,react数据流动是单向的

⑤ state对象在react应用中不可变的,需要使用setState方法更新状态;在vue中,state对象不是必须的,数据由data属性在vue对象中管理

**33、直接给一个数组项赋值，Vue 能检测到变化吗？**
由于 JavaScript 的限制，Vue 不能检测到以下数组的变动：

- 当你利用索引直接设置一个数组项时，例如：`vm.items[indexOfItem] = newValue`
- 当你修改数组的长度时，例如：`vm.items.length = newLength`

为了解决第一个问题，Vue 提供了以下操作方法：

```javascript
 // Vue.set 
Vue.set(vm.items, indexOfItem, newValue)
 // vm.$set，Vue.set的一个别名 
vm.$set(vm.items, indexOfItem, newValue)
 // Array.prototype.splice 
vm.items.splice(indexOfItem, 1, newValue) 
```


为了解决第二个问题，Vue 提供了以下操作方法：

```javascript
// Array.prototype.splice 
vm.items.splice(newLength) 
```

**34、Vue 的父组件和子组件生命周期钩子函数执行顺序？**

Vue 的父组件和子组件生命周期钩子函数执行顺序可以归类为以下 4 部分：

- 加载渲染过程
  父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted
- 子组件更新过程
  父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated
- 父组件更新过程
  父 beforeUpdate -> 父 updated
- 销毁过程
  父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed

**35、在哪个生命周期内调用异步请求？**

可以在钩子函数 created、beforeMount、mounted 中进行调用，因为在这三个钩子函数中，data 已经创建，可以将服务端端返回的数据进行赋值。但是本人推荐在 created 钩子函数中调用异步请求，因为在 created 钩子函数中调用异步请求有以下优点：

- 能更快获取到服务端数据，减少页面 loading 时间；
- ssr 不支持 beforeMount 、mounted 钩子函数，所以放在 created 中有助于一致性；

**36、在什么阶段才能访问操作DOM？**

在钩子函数 mounted 被调用前，Vue 已经将编译好的模板挂载到页面上，所以在 mounted 中可以访问操作 DOM。vue 具体的生命周期示意图可以参见如下，理解了整个生命周期各个阶段的操作，关于生命周期相关的面试题就难不倒你了。

**37、父组件可以监听到子组件的生命周期吗？**
比如有父组件 Parent 和子组件 Child，如果父组件监听到子组件挂载 mounted 就做一些逻辑处理，可以通过以下写法实现：

```vue
// Parent.vue
 <Child @mounted="doSomething"/>
// Child.vue
 mounted() { 
  this.$emit("mounted");
 } 
```

以上需要手动通过 $emit 触发父组件的事件，更简单的方式可以在父组件引用子组件时通过 @hook 来监听即可，如下所示：

```javascript
 //  Parent.vue
 <Child @hook:mounted="doSomething" ></Child>
  doSomething() {
    console.log('父组件监听到 mounted 钩子函数 ...'); 
},
   //  Child.vue mounted(){ 
   console.log('子组件触发 mounted 钩子函数 ...');
 }, 
  // 以上输出顺序为：
  // 子组件触发 mounted 钩子函数 ...
 // 父组件监听到 mounted 钩子函数 ...   
```


当然 @hook 方法不仅仅是可以监听 mounted，其它的生命周期事件，例如：created，updated 等都可以监听。

组件调用了$on监听的事件名符合以hook:开头，当前实例的vm._hasHookEvent会为true，组件就会在对应生命周期的时候通过emit触发对应的事件vm.$emit('hook:'+hook)

**38、组件中 data 为什么是一个函数？**

为什么组件中的 data 必须是一个函数，然后 return 一个对象，而 new Vue 实例里，data 可以直接是一个对象？

```text
// data
data() {
  return {
	message: "子组件",
	childName:this.name
  }
}

// new Vue
new Vue({
  el: '#app',
  router,
  template: '<App/>',
  components: {App}
})
```

因为组件是用来复用的，且 JS 里对象是引用关系，如果组件中 data 是一个对象，那么这样作用域没有隔离，子组件中的 data 属性值会相互影响，如果组件中 data 选项是一个函数，那么每个实例可以维护一份被返回对象的独立的拷贝，组件实例之间的 data 属性值不会互相影响；而 new Vue 的实例，是不会被复用的，因此不存在引用对象的问题。

**39、v-model 的原理？**

我们在 vue 项目中主要使用 v-model 指令在表单 input、textarea、select 等元素上创建双向数据绑定，我们知道 v-model 本质上不过是语法糖，v-model 在内部为不同的输入元素使用不同的属性并抛出不同的事件：

- text 和 textarea 元素使用 value 属性和 input 事件；
- checkbox 和 radio 使用 checked 属性和 change 事件；
- select 字段将 value 作为 prop 并将 change 作为事件。

以 input 表单元素为例：

```vue
<input v-model='something'>
相当于
<input v-bind:value="something" v-on:input="something = $event.target.value">
```


如果在自定义组件中，v-model 默认会利用名为 value 的 prop 和名为 input 的事件，如下所示：

```javascript
父组件：
<ModelChild v-model="message"></ModelChild>
子组件：
<div>{{value}}</div>
props:{
    value: String
},
methods: {
  test1(){
     this.$emit('input', '小红')
  },
},
```

**40、Vue 组件间通信有哪几种方式？**

Vue 组件间通信是面试常考的知识点之一，这题有点类似于开放题，你回答出越多方法当然越加分，表明你对 Vue 掌握的越熟练。Vue 组件间通信只要指以下 3 类通信：父子组件通信、隔代组件通信、兄弟组件通信，下面我们分别介绍每种通信方式且会说明此种方法可适用于哪类组件间通信。

**（1）`props / $emit` 适用 父子组件通信**

这种方法是 Vue 组件的基础，相信大部分同学耳闻能详，所以此处就不举例展开介绍。

**（2）`ref` 与 `$parent / $children` 适用 父子组件通信**

- `ref`：如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果用在子组件上，引用就指向组件实例
- `$parent` / `$children`：访问父 / 子实例

**（3）`EventBus （$emit / $on）` 适用于 父子、隔代、兄弟组件通信**

这种方法通过一个空的 Vue 实例作为中央事件总线（事件中心），用它来触发事件和监听事件，从而实现任何组件间的通信，包括父子、隔代、兄弟组件。

**（4）`$attrs`/`$listeners` 适用于 隔代组件通信**

- `$attrs`：包含了父作用域中不被 prop 所识别 (且获取) 的特性绑定 ( class 和 style 除外 )。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 ( class 和 style 除外 )，并且可以通过 `v-bind="$attrs"` 传入内部组件。通常配合 inheritAttrs 选项一起使用。
- `$listeners`：包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。它可以通过 `v-on="$listeners"` 传入内部组件

**（5）`provide / inject` 适用于 隔代组件通信**

祖先组件中通过 provider 来提供变量，然后在子孙组件中通过 inject 来注入变量。 provide / inject API 主要解决了跨级组件间的通信问题，不过它的使用场景，主要是子组件获取上级组件的状态，跨级组件间建立了一种主动提供与依赖注入的关系。

**（6）Vuex 适用于 父子、隔代、兄弟组件通信**

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。每一个 Vuex 应用的核心就是 store（仓库）。“store” 基本上就是一个容器，它包含着你的应用中大部分的状态 ( state )。

- Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
- 改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation。这样使得我们可以方便地跟踪每一个状态的变化。

**41、你使用过 Vuex 吗？**

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。每一个 Vuex 应用的核心就是 store（仓库）。“store” 基本上就是一个容器，它包含着你的应用中大部分的状态 ( state )。

（1）Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。

（2）改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation。这样使得我们可以方便地跟踪每一个状态的变化。
主要包括以下几个模块：

- State：定义了应用状态的数据结构，可以在这里设置默认的初始状态。
- Getter：允许组件从 Store 中获取数据，mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性。
- Mutation：是唯一更改 store 中状态的方法，且必须是同步函数。
- Action：用于提交 mutation，而不是直接变更状态，可以包含任意异步操作。
- Module：允许将单一的 Store 拆分为多个 store 且同时保存在单一的状态树中。

**42、使用过 Vue SSR 吗？说说 SSR？**

> Vue.js 是构建客户端应用程序的框架。默认情况下，可以在浏览器中输出 Vue 组件，进行生成 DOM 和操作 DOM。然而，也可以将同一个组件渲染为服务端的 HTML 字符串，将它们直接发送到浏览器，最后将这些静态标记"激活"为客户端上完全可交互的应用程序。
> 即：SSR大致的意思就是vue在客户端将标签渲染成的整个 html 片段的工作在服务端完成，服务端形成的html 片段直接返回给客户端这个过程就叫做服务端渲染。

**服务端渲染 SSR 的优缺点如下：**
**（1）服务端渲染的优点：**

- 更好的 SEO： 因为 SPA 页面的内容是通过 Ajax 获取，而搜索引擎爬取工具并不会等待 Ajax 异步完成后再抓取页面内容，所以在 SPA 中是抓取不到页面通过 Ajax 获取到的内容；而 SSR 是直接由服务端返回已经渲染好的页面（数据已经包含在页面中），所以搜索引擎爬取工具可以抓取渲染好的页面；
- 更快的内容到达时间（首屏加载更快）： SPA 会等待所有 Vue 编译后的 js 文件都下载完成后，才开始进行页面的渲染，文件下载等需要一定的时间等，所以首屏渲染需要一定的时间；SSR 直接由服务端渲染好页面直接返回显示，无需等待下载 js 文件及再去渲染等，所以 SSR 有更快的内容到达时间；

**（2) 服务端渲染的缺点：**

- 更多的开发条件限制： 例如服务端渲染只支持 beforCreate 和 created 两个钩子函数，这会导致一些外部扩展库需要特殊处理，才能在服务端渲染应用程序中运行；并且与可以部署在任何静态文件服务器上的完全静态单页面应用程序 SPA 不同，服务端渲染应用程序，需要处于 Node.js server 运行环境；
- 更多的服务器负载：在 Node.js 中渲染完整的应用程序，显然会比仅仅提供静态文件的 server 更加大量占用CPU 资源 (CPU-intensive - CPU 密集)，因此如果你预料在高流量环境 ( high traffic ) 下使用，请准备相应的服务器负载，并明智地采用缓存策略。

**43、vue-router 路由模式有几种？**

vue-router 有 3 种路由模式：hash、history、abstract，对应的源码如下所示：

```javascript
switch (mode) {
  case 'history':
	this.history = new HTML5History(this, options.base)
	break
  case 'hash':
	this.history = new HashHistory(this, options.base, this.fallback)
	break
  case 'abstract':
	this.history = new AbstractHistory(this, options.base)
	break
  default:
	if (process.env.NODE_ENV !== 'production') {
	  assert(false, `invalid mode: ${mode}`)
	}
}
```

其中，3 种路由模式的说明如下：

- hash: 使用 URL hash 值来作路由。支持所有浏览器，包括不支持 HTML5 History Api 的浏览器；
- history : 依赖 HTML5 History API 和服务器配置。具体可以查看 HTML5 History 模式；
- abstract : 支持所有 JavaScript 运行环境，如 Node.js 服务器端。如果发现没有浏览器的 API，路由会自动强制进入这个模式.

**44、能说下 vue-router 中常用的 hash 和 history 路由模式实现原理吗？**

**（1）hash 模式的实现原理**
早期的前端路由的实现就是基于 location.hash 来实现的。其实现原理很简单，location.hash 的值就是 URL 中 # 后面的内容。比如下面这个网站，它的 location.hash 的值为 '#search'：

```javascript
https://www.word.com#search
```

hash 路由模式的实现主要是基于下面几个特性：

- URL 中 hash 值只是客户端的一种状态，也就是说当向服务器端发出请求时，hash 部分不会被发送；
- hash 值的改变，都会在浏览器的访问历史中增加一个记录。因此我们能通过浏览器的回退、前进按钮控制hash 的切换；
- 可以通过 a 标签，并设置 href 属性，当用户点击这个标签后，URL 的 hash 值会发生改变；或者使用 JavaScript 来对 loaction.hash 进行赋值，改变 URL 的 hash 值；
- 我们可以使用 hashchange 事件来监听 hash 值的变化，从而对页面进行跳转（渲染）。

**（2）history 模式的实现原理**
HTML5 提供了 History API 来实现 URL 的变化。其中做最主要的 API 有以下两个：history.pushState() 和 history.repalceState()。这两个 API 可以在不进行刷新的情况下，操作浏览器的历史纪录。唯一不同的是，前者是新增一个历史记录，后者是直接替换当前的历史记录，如下所示：

```javascript
window.history.pushState(null, null, path);
window.history.replaceState(null, null, path);
```

history 路由模式的实现主要基于存在下面几个特性：

- pushState 和 repalceState 两个 API 来操作实现 URL 的变化 ；
- 我们可以使用 popstate 事件来监听 url 的变化，从而对页面进行跳转（渲染）；
- history.pushState() 或 history.replaceState() 不会触发 popstate 事件，这时我们需要手动触发页面跳转（渲染）。

**45、什么是 MVVM？**


Model–View–ViewModel （MVVM） 是一个软件架构设计模式，由微软 WPF 和 Silverlight 的架构师 Ken Cooper 和 Ted Peters 开发，是一种简化用户界面的事件驱动编程方式。由 John Gossman（同样也是 WPF 和 Silverlight 的架构师）于2005年在他的博客上发表
MVVM 源自于经典的 Model–View–Controller（MVC）模式 ，MVVM 的出现促进了前端开发与后端业务逻辑的分离，极大地提高了前端开发效率，MVVM 的核心是 ViewModel 层，它就像是一个中转站（value converter），负责转换 Model 中的数据对象来让数据变得更容易管理和使用，该层向上与视图层进行双向数据绑定，向下与 Model 层通过接口请求进行数据交互，起呈上启下作用。

（1）View 层
View 是视图层，也就是用户界面。前端主要由 HTML 和 CSS 来构建 。

（2）Model 层

Model 是指数据模型，泛指后端进行的各种业务逻辑处理和数据操控，对于前端来说就是后端提供的 api 接口。

（3）ViewModel 层

ViewModel 是由前端开发人员组织生成和维护的视图数据层。在这一层，前端开发者对从后端获取的 Model 数据进行转换处理，做二次封装，以生成符合 View 层使用预期的视图数据模型。需要注意的是 ViewModel 所封装出来的数据模型包括视图的状态和行为两部分，而 Model 层的数据模型是只包含状态的，比如页面的这一块展示什么，而页面加载进来时发生什么，点击这一块发生什么，这一块滚动时发生什么这些都属于视图行为（交互），视图状态和行为都封装在了 ViewModel 里。这样的封装使得 ViewModel 可以完整地去描述 View 层。

MVVM 框架实现了双向绑定，这样 ViewModel 的内容会实时展现在 View 层，前端开发者再也不必低效又麻烦地通过操纵 DOM 去更新视图，MVVM 框架已经把最脏最累的一块做好了，我们开发者只需要处理和维护 ViewModel，更新数据视图就会自动得到相应更新。这样 View 层展现的不是 Model 层的数据，而是 ViewModel 的数据，由 ViewModel 负责与 Model 层交互，这就完全解耦了 View 层和 Model 层，这个解耦是至关重要的，它是前后端分离方案实施的重要一环。

我们以下通过一个 Vue 实例来说明 MVVM 的具体实现，有 Vue 开发经验的同学应该一目了然：
（1）View 层

```vue
<div id="app">
    <p>{{message}}</p>
    <button v-on:click="showMessage()">Click me</button>
</div>
```


（2）ViewModel 层

```javascript
var app = new Vue({
    el: '#app',
    data: {  // 用于描述视图状态   
        message: 'Hello Vue!', 
    },
    methods: {  // 用于描述视图行为  
        showMessage(){
            let vm = this;
            alert(vm.message);
        }
    },
    created(){
        let vm = this;
        // Ajax 获取 Model 层的数据
        ajax({
            url: '/your/server/data/api',
            success(res){
                vm.message = res;
            }
        });
    }
})
```


（3） Model 层

```javascript
{
    "url": "/your/server/data/api",
    "res": {
        "success": true,
        "name": "IoveC",
        "domain": "www.cnblogs.com"
    }
}
```

**46、Vue 是如何实现数据双向绑定的？**

Vue 数据双向绑定主要是指：数据变化更新视图，视图变化更新数据。

即：

- 输入框内容变化时，Data 中的数据同步变化。即 View => Data 的变化。
- Data 中的数据变化时，文本节点的内容同步变化。即 Data => View 的变化。

其中，View 变化更新 Data ，可以通过事件监听的方式来实现，所以 Vue 的数据双向绑定的工作主要是如何根据 Data 变化更新 View。

Vue 主要通过以下 4 个步骤来实现数据双向绑定的：

实现一个监听器 Observer：对数据对象进行遍历，包括子属性对象的属性，利用 Object.defineProperty() 对属性都加上 setter 和 getter。这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化。
实现一个解析器 Compile：解析 Vue 模板指令，将模板中的变量都替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，调用更新函数进行数据更新。
实现一个订阅者 Watcher：Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁 ，主要的任务是订阅 Observer 中的属性值变化的消息，当收到属性值变化的消息时，触发解析器 Compile 中对应的更新函数。实现一个订阅器 Dep：订阅器采用 发布-订阅 设计模式，用来收集订阅者 Watcher，对监听器 Observer 和 订阅者 Watcher 进行统一管理。
以上四个步骤的流程图表示如下，如果有同学理解不大清晰的，可以查看作者专门介绍数据双向绑定的文章[《0 到 1 掌握：Vue 核心之数据双向绑定》](https://link.zhihu.com/?target=https%3A//juejin.im/post/5d421bcf6fb9a06af23853f1)，有进行详细的讲解、以及代码 demo 示例。

**47、Vue 框架怎么实现对象和数组的监听？**

如果被问到 Vue 怎么实现数据双向绑定，大家肯定都会回答 通过 Object.defineProperty() 对数据进行劫持，但是 Object.defineProperty() 只能对属性进行数据劫持，不能对整个对象进行劫持，同理无法对数组进行劫持，但是我们在使用 Vue 框架中都知道，Vue 能检测到对象和数组（部分方法的操作）的变化，那它是怎么实现的呢？我们查看相关代码如下：

```javascript
  /**
   * Observe a list of Array items.
   */
  observeArray (items: Array<any>) {
    for (let i = 0, l = items.length; i < l; i++) {
      observe(items[i])  // observe 功能为监测数据的变化
    }
  }
  /**
   * 对属性进行递归遍历
   */
  let childOb = !shallow && observe(val) // observe 功能为监测数据的变化
```

通过以上 Vue 源码部分查看，我们就能知道 Vue 框架是通过遍历数组 和递归遍历对象，从而达到利用 Object.defineProperty() 也能对对象和数组（部分方法的操作）进行监听。

**48、Proxy 与 Object.defineProperty 优劣对比** 

**Proxy 的优势如下:**

- Proxy 可以直接监听对象而非属性；
- Proxy 可以直接监听数组的变化；
- Proxy 有多达 13 种拦截方法,不限于 apply、ownKeys、deleteProperty、has 等等是 Object.defineProperty 不具备的；
- Proxy 返回的是一个新对象,我们可以只操作新的对象达到目的,而 Object.defineProperty 只能遍历对象属性直接修改；
- Proxy 作为新标准将受到浏览器厂商重点持续的性能优化，也就是传说中的新标准的性能红利；

**Object.defineProperty 的优势如下:**

- 兼容性好，支持 IE9，而 Proxy 的存在浏览器兼容性问题,而且无法用 polyfill 磨平，因此 Vue 的作者才声明需要等到下个大版本( 3.0 )才能用 Proxy 重写。

Vue 怎么用 vm.$set() 解决对象新增属性不能响应的问题 ？


受现代 JavaScript 的限制 ，Vue **无法检测到对象属性的添加或删除**。由于 Vue 会在初始化实例时对属性执行 getter/setter 转化，所以属性必须在 data 对象上存在才能让 Vue 将它转换为响应式的。但是 Vue 提供了 `Vue.set (object, propertyName, value) / vm.$set (object, propertyName, value)` 来实现为对象添加响应式属性，那框架本身是如何实现的呢？


我们查看对应的 Vue 源码：`vue/src/core/instance/index.js`

```text
export function set (target: Array<any> | Object, key: any, val: any): any {
  // target 为数组  
  if (Array.isArray(target) && isValidArrayIndex(key)) {
    // 修改数组的长度, 避免索引>数组长度导致splcie()执行有误
    target.length = Math.max(target.length, key)
    // 利用数组的splice变异方法触发响应式  
    target.splice(key, 1, val)
    return val
  }
  // key 已经存在，直接修改属性值  
  if (key in target && !(key in Object.prototype)) {
    target[key] = val
    return val
  }
  const ob = (target: any).__ob__
  // target 本身就不是响应式数据, 直接赋值
  if (!ob) {
    target[key] = val
    return val
  }
  // 对属性进行响应式处理
  defineReactive(ob.value, key, val)
  ob.dep.notify()
  return val
}
```


我们阅读以上源码可知，vm.$set 的实现原理是：

- 如果目标是数组，直接使用数组的 splice 方法触发相应式；
- 如果目标是对象，会先判读属性是否存在、对象是否是响应式，最终如果要对属性进行响应式处理，则是通过调用 defineReactive 方法进行响应式处理（ defineReactive 方法就是 Vue 在初始化对象时，给对象属性采用 Object.defineProperty 动态添加 getter 和 setter 的功能所调用的方法）

**49、虚拟 DOM 的优缺点？** 

**优点：**

- **保证性能下限：** 框架的虚拟 DOM 需要适配任何上层 API 可能产生的操作，它的一些 DOM 操作的实现必须是普适的，所以它的性能并不是最优的；但是比起粗暴的 DOM 操作性能要好很多，因此框架的虚拟 DOM 至少可以保证在你不需要手动优化的情况下，依然可以提供还不错的性能，即保证性能的下限；
- **无需手动操作 DOM：** 我们不再需要手动去操作 DOM，只需要写好 View-Model 的代码逻辑，框架会根据虚拟 DOM 和 数据双向绑定，帮我们以可预期的方式更新视图，极大提高我们的开发效率；
- **跨平台：** 虚拟 DOM 本质上是 JavaScript 对象,而 DOM 与平台强相关，相比之下虚拟 DOM 可以进行更方便地跨平台操作，例如服务器渲染、weex 开发等等。

**缺点:**

- **无法进行极致优化：** 虽然虚拟 DOM + 合理的优化，足以应对绝大部分应用的性能需求，但在一些性能要求极高的应用中虚拟 DOM 无法进行针对性的极致优化。

**50、虚拟 DOM 实现原理？**

虚拟 DOM 的实现原理主要包括以下 3 部分：

- 用 JavaScript 对象模拟真实 DOM 树，对真实 DOM 进行抽象；
- diff 算法 — 比较两棵虚拟 DOM 树的差异；
- pach 算法 — 将两个虚拟 DOM 对象的差异应用到真正的 DOM 树。

如果对以上 3 个部分还不是很了解的同学，可以查看本文作者写的另一篇详解虚拟 DOM 的文章《[深入剖析：Vue核心之虚拟DOM](https://link.zhihu.com/?target=https%3A//juejin.im/post/5d36cc575188257aea108a74%23heading-14)》

**51、Vue 中的 key 有什么作用？** 

key 是为 Vue 中 vnode 的唯一标记，通过这个 key，我们的 diff 操作可以更准确、更快速。Vue 的 diff 过程可以概括为：oldCh 和 newCh 各有两个头尾的变量 oldStartIndex、oldEndIndex 和 newStartIndex、newEndIndex，它们会新节点和旧节点会进行两两对比，即一共有4种比较方式：newStartIndex 和oldStartIndex 、newEndIndex 和 oldEndIndex 、newStartIndex 和 oldEndIndex 、newEndIndex 和 oldStartIndex，如果以上 4 种比较都没匹配，如果设置了key，就会用 key 再进行比较，在比较的过程中，遍历会往中间靠，一旦 StartIdx > EndIdx 表明 oldCh 和 newCh 至少有一个已经遍历完了，就会结束比较。具体有无 key 的 diff 过程，可以查看作者写的另一篇详解虚拟 DOM 的文章《[深入剖析：Vue核心之虚拟DOM](https://link.zhihu.com/?target=https%3A//juejin.im/post/5d36cc575188257aea108a74%23heading-14)》

所以 Vue 中 key 的作用是：key 是为 Vue 中 vnode 的唯一标记，通过这个 key，我们的 diff 操作可以更准确、更快速

**更准确**：因为带 key 就不是就地复用了，在 sameNode 函数 `a.key === b.key` 对比中可以避免就地复用的情况。所以会更加准确。
**更快速**：利用 key 的唯一性生成 map 对象来获取对应节点，比遍历方式更快，源码如下：

```javascript
 function createKeyToOldIdx (children, beginIdx, endIdx) {
  let i, key
  const map = {}
  for (i = beginIdx; i <= endIdx; ++i) {
    key = children[i].key
    if (isDef(key)) map[key] = i
  }
  return map
}
```

**52、你有对 Vue 项目进行哪些优化？**

如果没有对 Vue 项目没有进行过优化总结的同学，可以参考本文作者的另一篇文章[《 Vue 项目性能优化 — 实践指南 》](https://link.zhihu.com/?target=https%3A//juejin.im/post/5d548b83f265da03ab42471d)，文章主要介绍从 3 个大方面，22 个小方面详细讲解如何进行 Vue 项目的优化。
**（1）代码层面的优化**

- v-if 和 v-show 区分使用场景
- computed 和 watch 区分使用场景
- v-for 遍历必须为 item 添加 key，且避免同时使用 v-if
- 长列表性能优化
- 事件的销毁
- 图片资源懒加载
- 路由懒加载
- 第三方插件的按需引入
- 优化无限列表性能
- 服务端渲染 SSR or 预渲染

**（2）Webpack 层面的优化**

- Webpack 对图片进行压缩
- 减少 ES6 转为 ES5 的冗余代码
- 提取公共代码
- 模板预编译
- 提取组件的 CSS
- 优化 SourceMap
- 构建结果输出分析
- Vue 项目的编译优化

**（3）基础的 Web 技术的优化**

- 开启 gzip 压缩
- 浏览器缓存
- CDN 的使用
- 使用 Chrome Performance 查找性能瓶颈

**53、对于即将到来的 vue3.0 特性你有什么了解的吗？**

Vue 3.0 正走在发布的路上，Vue 3.0 的目标是让 Vue 核心变得更小、更快、更强大，因此 Vue 3.0 增加以下这些新特性：

**（1）监测机制的改变**

3.0 将带来基于代理 Proxy 的 observer 实现，提供全语言覆盖的反应性跟踪。这消除了 Vue 2 当中基于 Object.defineProperty 的实现所存在的很多限制：

- 只能监测属性，不能监测对象
- 检测属性的添加和删除；
- 检测数组索引和长度的变更；
- 支持 Map、Set、WeakMap 和 WeakSet。

新的 observer 还提供了以下特性：

- 用于创建 observable 的公开 API。这为中小规模场景提供了简单轻量级的跨组件状态管理解决方案。
- 默认采用惰性观察。在 2.x 中，不管反应式数据有多大，都会在启动时被观察到。如果你的数据集很大，这可能会在应用启动时带来明显的开销。在 3.x 中，只观察用于渲染应用程序最初可见部分的数据。
- 更精确的变更通知。在 2.x 中，通过 Vue.set 强制添加新属性将导致依赖于该对象的 watcher 收到变更通知。在 3.x 中，只有依赖于特定属性的 watcher 才会收到通知。
- 不可变的 observable：我们可以创建值的“不可变”版本（即使是嵌套属性），除非系统在内部暂时将其“解禁”。这个机制可用于冻结 prop 传递或 Vuex 状态树以外的变化。
- 更好的调试功能：我们可以使用新的 renderTracked 和 renderTriggered 钩子精确地跟踪组件在什么时候以及为什么重新渲染。

**（2）模板**


模板方面没有大的变更，只改了作用域插槽，2.x 的机制导致作用域插槽变了，父组件会重新渲染，而 3.0 把作用域插槽改成了函数的方式，这样只会影响子组件的重新渲染，提升了渲染的性能。
同时，对于 render 函数的方面，vue3.0 也会进行一系列更改来方便习惯直接使用 api 来生成 vdom 。


**（3）对象式的组件声明方式**


vue2.x 中的组件是通过声明的方式传入一系列 option，和 TypeScript 的结合需要通过一些装饰器的方式来做，虽然能实现功能，但是比较麻烦。3.0 修改了组件的声明方式，改成了类式的写法，这样使得和 TypeScript 的结合变得很容易。
此外，vue 的源码也改用了 TypeScript 来写。其实当代码的功能复杂之后，必须有一个静态类型系统来做一些辅助管理。现在 vue3.0 也全面改用 TypeScript 来重写了，更是使得对外暴露的 api 更容易结合 TypeScript。静态类型系统对于复杂代码的维护确实很有必要。

**（4）其它方面的更改**


vue3.0 的改变是全面的，上面只涉及到主要的 3 个方面，还有一些其他的更改：

- 支持自定义渲染器，从而使得 weex 可以通过自定义渲染器的方式来扩展，而不是直接 fork 源码来改的方式。
- 支持 Fragment（多个根节点）和 Protal（在 dom 其他部分渲染组建内容）组件，针对一些特殊的场景做了处理。
- 基于 treeshaking 优化，提供了更多的内置功能。

**54、完整的 vue-router 导航解析流程**

> 1.导航被触发；
> 2.在失活的组件里调用beforeRouteLeave守卫；
> 3.调用全局beforeEach守卫；
> 4.在复用组件里调用beforeRouteUpdate守卫；
> 5.调用路由配置里的beforeEnter守卫；
> 6.解析异步路由组件；
> 7.在被激活的组件里调用beforeRouteEnter守卫；
> 8.调用全局beforeResolve守卫；
> 9.导航被确认；
> 10..调用全局的afterEach钩子；
> 11.DOM更新；
> 12.用创建好的实例调用beforeRouteEnter守卫中传给next的回调函数。

**55、怎么理解单项数据流？**

所有的 prop 都使得其父子 prop 之间形成了一个**单向下行绑定**：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外改变父级组件的状态，从而导致你的应用的数据流向难以理解。

额外的，每次父级组件发生更新时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你不应该在一个子组件内部改变 prop。如果你这样做了，Vue 会在浏览器的控制台中发出警告。子组件想修改时，只能通过 $emit 派发一个自定义事件，父组件接收到后，由父组件修改。
有两种常见的试图改变一个 prop 的情形 :

- **这个 prop 用来传递一个初始值；这个子组件接下来希望将其作为一个本地的 prop 数据来使用。** 在这种情况下，最好定义一个本地的 data 属性并将这个 prop 用作其初始值：

```javascript
 props: ['initialCounter'],
 data: function () {
   return {
     counter: this.initialCounter
   }
 } 
```

- **这个 prop 以一种原始的值传入且需要进行转换。** 在这种情况下，最好使用这个 prop 的值来定义一个计算属性

```javascript
 props: ['size'],
 computed: { 
  normalizedSize: function () {
     return this.size.trim().toLowerCase() 
  }
 } 
```

**参考资料**

+ [知乎专栏](https://zhuanlan.zhihu.com/p/103763164)
+ [知乎专栏——活泼的小白洋](https://zhuanlan.zhihu.com/p/81713054)

