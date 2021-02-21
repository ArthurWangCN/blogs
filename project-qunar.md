---
title: 一个基于Vue的旅行App
date: 2020-02-11 01:33:46
tags: [项目, 前端]
categories: [前端, Vue]
---

>本文记录一个基于Vue开发的旅行app

## 准备
+ Axios: Ajax数据获取
+ Vue Router: 多页面之间的路由
+ Vuex: 各个组件之间数据共享
+ 异步组件: 代码上线性能更优
+ Stylus: 样式
+ 递归组件: 组件调用自身
+ 插件: swiper better-scroll
+ 公用组件: 公用组件拆分


代码结构：

```
travel
|---Home
   	|---Header
    |---Swiper
    |---Icons
    |---Recommend
	|---Weekend
|---City
	|---CityHeader
	|---CitySearch
	|---CityList
	|---CityAlphabet
|---Detail
	|---DetailHeader
	|---DetailBanner
	|---DetailList
```

## Home页面

### Header组件
主要是城市选择功能，点击路由到 `city` 页面

```
<router-link to="/city">
	<div class="header-right">
		{{this.city}}
		<span class="iconfont icon-arrow-down">&#xe64a;</span>
	</div>
</router-link>
```

其中 this.city 是Vuex提供的当前城市

```
...mapState(['city'])
```

### Swiper组件
#### vue-awesome-swiper
借助 [vue-awesome-swiper](https://github.com/surmon-china/vue-awesome-swiper) 插件实现首页轮播图效果

```
<div class="swiper-wrapper">
	<swiper :options="swiperOption" v-if="showSwiper">
		<swiper-slide v-for="item of swiperList" :key="item.id">
          <img class="swiper-img" :src="item.imgUrl" alt="">
        </swiper-slide>
        <div class="swiper-pagination"  slot="pagination"></div>
	</swiper>
</div>
```
swiper相关的设置可以在 data 里的 swiperOption 里配置

```
swiperOption: {
	autoplay: true,
	loop: true,
	pagination: {
		el: '.swiper-pagination'
	}
}
```

轮播图片通过ajax动态获取，接收从 `Home.vue` 传的 swiperList 数组。

#### 使用 >>> 穿透scoped

其中分页器需要把颜色调成白色：

```
.swiper-wrapper >>> .swiper-pagination-bullet-active
    background #ffffff !important
```

这里用到了 `>>>` 来穿透scoped

vue引用了第三方组件，需要在组件中局部修改第三方组件的样式，而又不想去除scoped属性造成组件之间的样式污染。此时只能通过 `>>>`，穿透scoped。

有些 Sass 之类的预处理器无法正确解析 `>>>`。可以使用 `/deep/` 操作符( >>> 的别名)

```
<style scoped>
  外层 >>> 第三方组件 {
  	样式
  }
/deep/  第三方组件 {
	样式
}
</style>
```

### Icons组件
重点是一个页面8个icon区域，从 `Home.vue` 拿 iconList 数据，超出8个就要进行分页，同时利用 swiper 实现左右滑动。

拿到 iconList 后对数据进行拆分，形成一个二维数组：

```
computed: {
  pages () {
    const pages = []
	  this.iconList.forEach((item, index) => {
        const page = Math.floor(index / 8)
        if (!pages[page]) {
          pages[page] = []
        }
        pages[page].push(item)
      })
    return pages
  }
}
```

数据循环渲染：

```
<div class="icons-wrapper">
  <swiper>
    <swiper-slide v-for="(page, index) in pages" :key="index">
      <div class="icon" v-for="item in page" :key="item.id">
        <div class="icon-img">
          <img class="icon-img-content" :src="item.imgUrl" alt="">
        </div>
        <p class="icon-desc">{{item.desc}}</p>
      </div>            
    </swiper-slide>
  </swiper>
</div>
```

首先在 swiper-slide 上对 pages 进行循环，然后在内部的图片和文字描述上对上层循环的 page 进行循环，以此来实现分页。

### Recommed组件  Weekend组件

组件唯一重点是动态路由，跳转到详情页的时候带一个参数 id：

```
<router-link :to="'/detail/'+item.id">
	//内容
</router-link>
```

```
const routes = [{
  name: '/detail/:id',
  name: 'detail',
  component: Detail
}]
```
在详情页组件 	`Detail.vue` 中发送ajax请求时，也要带一个参数：

```
getDetailInfo () {
  axios.get('/mock/details.json', {
    params: {
      id: this.$route.params.id
    }
  }).then(this.getDetailInfoSucc)
}
```

## City页面
### CityHeader组件
无特殊说明

### CityList组件
城市列表组件，数据是从 `City.vue` 传过来的 cities 和 hotCities，页面滚动使用 [better-scroll](http://ustbhuangyi.github.io/better-scroll/doc/api.html) 插件来到近似原生APP的效果，同时也为了实现点击字母滚动到相应的部分。

```
mounted () {
  this.scroll = new BScroll(this.$refs.wrapper, {})
}
```

城市列表部分对 cities 进行两次循环：

```
<div class="area"
  v-for="(item, key) in cities"
  :key="key"
  :ref="key"
>
  <div class="city-title">{{key}}</div>
  <div class="city-alphbet-list">
    <div class="city-alphbet-item border-topbottom"
      v-for="innerItem in item"
      :key="innerItem.id"
      @click="handleCityClick(innerItem.name)"
    >
      {{innerItem.name}}
    </div>
  </div>
</div>
```

外层 .area 循环开头字母，内层 .city-alphabet-item 循环城市名。

对于和字母组件联动实现点击字母跳转到相应的部分，需要先在 `CityAlphabet.vue` 里获取点击的字母，通过 `$emit` 传给他们共同的父组件 `City.vue`，然后在传给 `CityList.vue`，拿到字母 letter 后进行监听，通过 better-scorll 滚动到相应的区域：

```
watch: {
  letter () {
    if (this.letter) {
      const ele = this.$refs[this.letter][0]
      this.scroll.scrollToElement(ele)
    }
  }
}
```

点击城市名可以实现城市改变，同时跳转到首页：

```
methods: {
  handleCityClick (city) {
    this.cityChange(city)
    this.$router.push('/')
  },
  ...mapMutations(['cityChange'])
}
```

### CityAlphabet组件
字母列表循环 cities 里的 key：

```
<ul class="alphabet-list">
  <li class="alphabet-item"
    v-for="(item, key) in cities"
    :key="key"
    :ref="key"
    @click="handleLetterClick"
    @touchstart="handleTouchStart"
    @touchmove="handleTouchMove"
    @touchend="handleTouchend"
  >
    {{key}}
  </li>
</ul>
```

点击事件向父组件传递点击对应的字母：

```
handleLetterClick (e) {
  this.$emit('letterChange', e.target.innerText)
}
```

触摸状态 touchStatus 初始为 false，触摸滚动开始为true，触摸滚动结束为false。通过计算触摸当前距顶部距离计算当前字母：

```
handleTouchStart () {
  this.touchStatus = true
},
handleTouchMove (e) {
  if (this.touchStatus) {
    if (this.timer) {
      clearTimeout(this.timer)
    }
    this.timer = setTimeout(() => {
      const touchY = e.touches[0].clientY - 79
      const index = Math.floor((touchY - this.startY) / 20)
      if (index >= 0 && index < this.letters.length) {
        this.$emit('letterChange', this.letters[index])
      }
    }, 7)
  }
},
handleTouchend () {
  this.touchStatus = false
}
```

`this.startY` 是 A 距离顶部的距离：

```
this.startY = this.$refs['A'][0].offsetTop
```




### CitySearch组件
给 input 的 v-model 设置一个 keyword， 对 keyword 进行监听：

```
watch: {
  keyword () {
    if (this.timer) {
      clearTimeout(this.timer)
    }
    if (!this.keyword) {
      this.list = []
      return
    }
    this.timer = setTimeout(() => {
      const result = []
      for (let i in this.cities) {
        this.cities[i].forEach((value) => {
          if (value.spell.indexOf(this.keyword) > -1 || value.name.indexOf(this.keyword) > -1) {
            result.push(value)
          }
        })
      }
      this.list = result
    }, 100)
  }
}
```
对从 `City.vue` 传过来的 cities 数据进行 forEach 遍历，如果存在拼写名 value.spell 或 城市名 value.name 和 keyword  有匹配，就把当前数据添加到 result 里，并且也页面中循环这个数据。同样点击城市后可以修改 vuex 中的数据，实现城市之间的切换。

页面滚动同样使用 better-scroll，同时使用定时器来提高页面性能。


## Detail页面 
### DetailHeader组件
动态绑定属性实现向下滚动时头部有透明度渐隐渐现效果

### DetailBanner组件
通过公用组件 `Gallery.vue` 来实现画廊效果， `FadeAnimation.vue` 来实现画廊组件的渐隐渐现。

```
<fade-animation>
  <common-gallery
    v-if="showGallery"
    :images="galleryImgs"
    @close="galleryClose"
  ></common-gallery>
</fade-animation>
```

### DetailList组件
递归组件 ，自身调用自身

```
<div v-if="item.children" class="item-children">
  <detail-list :detailList="item.children"></detail-list>
</div>
```

## 公用组件
### FadeAnimation

transition 实现渐隐渐现

### Gallery
画廊组件，通过 swiper 实现


## 图标
采用了 [Iconfont-阿里巴巴矢量图标库](https://www.iconfont.cn/)




页面：

![qunar](http://blogpic.at15cm.com/qunar-pic-1.jpg)

![qunar](http://blogpic.at15cm.com/qunar-pic-2.jpg)

![qunar](http://blogpic.at15cm.com/qunar-pic-3.jpg)