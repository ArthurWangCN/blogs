---
title: 一个文章电影类小程序
date: 2020-02-22 18:52:19
tags: [小程序, 项目]
categories: [前端, 小程序]
---
>本文记录一个基于微信小程序开发的文章和电影资讯类小程序

## 准备

代码结构：

```
article&movie
|---welcome		//启动页
|---posts  //文章页主页
	|---post-detail		//文章详情页
	|---posts-template	//文章主页模板文件
|---movies	//电影主页
	|---movie-list		//电影列表模板文件
	|---movie-item		//电影列表项模板文件
	|---movie-detail	//电影详情页
	|---stars			//评分星级模板文件
	|---movie-more		//更多电影页
	|---template-movie-gird		//电影网格模板文件
```

说明：文章页数据来自本地，电影信息从豆瓣API获取。


## welcome启动页
目的是初识小程序，略微熟悉小程序代码。

通过 welcom 启动页 「开启小程序之旅」 按钮点击进入 posts 文章页：

```
<view class="user-start" bindtap="onBtnTap">
  <text class="start-btn">开启小程序之旅</text>
</view>
```

值得注意的是，如果想要路由到tab栏中的某一页，必须使用 `wx.switchTab`，而 `wx.navigateTo` 和 `wx.redirectTo` 均不可用：

```
onBtnTap: function() {
  wx.switchTab({
    url: '../posts/posts',
  })
}
```


## posts文章页

### 文章页轮播图
小程序中轮播图可用自带的 [swiper](https://developers.weixin.qq.com/miniprogram/dev/component/swiper.html) 组件来实现：

```
<swiper catchtap="onSwiperTap" autoplay="true" indicator-dots="true">
	<swiper-item>
	  <image src="/images/vr.png" data-postId="0004"></image>
	</swiper-item>
	<swiper-item>
	  <image src="/images/wx.png" data-postId="0003"></image>
	</swiper-item>
	<swiper-item>
	  <image src="/images/iqiyi.png" data-postId="0005"></image>
	</swiper-item>
</swiper>
```

点击路由到相应的文章详情页：

```
onSwiperTap: function (event) {
  var postId = event.target.dataset.postid;		//点击获取元素上绑定的postid
  wx.navigateTo({
    url: 'post-detail/post-detail?id=' + postId,
  })
},
```

### posts_template

使用模板可以更好的实现代码复用，提高开发效率。

模板文件中：

```
<template name="post-item">xxx</template>
```

使用模板：

```
<import src="posts_template/posts_template />
<template is="post-item"></template>
```

注意要在 wxss 文件中引入模板的样式：

```
@import "posts_template/posts_template";
```

template没有 js 和 json 文件，所以逻辑要写到引用的文件里。

### 文章详情页

通过 onLoad 函数传的 `options` 获取路由中的 id ，遍历获得相应的文章数据：

```
var postId = options.id;
var postList = postsData.postList;		//postsData为本地数据
postList.forEach(item => {
  if(item.postId === postId) {
    this.setData({ postItem: item });
  }
})
```

#### 文章收藏模块
文章页没有与服务器通信，所以用本地存储来做收藏状态的纪录。

页面 onLoad 的时候从本地存储拿 `post_collected`，存在的话拿到文章是否收藏的状态，不存在的话给文章的收藏状态设为 false：
```
var postCollected = wx.getStorageSync('post_collected');
if(postCollected){
  var pageCollected = postCollected[postId];
  if(pageCollected) {
    this.setData({collected: pageCollected});
  } else {
    postCollected[postId] = false;
    wx.setStorageSync('post_collected', postCollected);
} else {
  var postCollected = {};
  postCollected[postId] = false;
  wx.setStorageSync('post_collected', postCollected);
}

```

点击收藏按钮：

```
onCollectTap: function() {
  var postCollected = wx.getStorageSync('post_collected');
  postCollected[this.data.postId] = !postCollected[this.data.postId];
  this.setData({collected: postCollected[this.data.postId]});
  wx.setStorageSync('post_collected', postCollected);
  //收藏成功与取消收藏成功弹窗
  wx.showToast({
    title: postsCollected[this.data.postId] ? '收藏成功' : '取消成功',
    duration: 1000
  });
}
```

页面通过判断 `collected` 来决定显示收藏图片。

```
<image wx:if="{{collected}}" class="detail-collect" catchtap="onCollectTap" src="/images/icon/collection.png"></image>
<image wx:else class="detail-collect" catchtap="onCollectTap" src="/images/icon/collection-anti.png"></image>
```

#### 背景音乐模块

初始化一个音乐播放器：

```
initMusicPlayer: function() {
  const backgroundAudioManager = wx.getBackgroundAudioManager();
  const music = this.data.postItem.music;
  backgroundAudioManager.title = music.title;
  backgroundAudioManager.singer = music.singer;
  backgroundAudioManager.coverImgUrl = music.coverImgUrl;
  // 设置了 src 之后会自动播放
  backgroundAudioManager.src = music.url;
  return backgroundAudioManager;
}
```

监控播放器状态可用 `onPause` 和 `onPlay` 以及 `onEnded`。


## movies电影页

电影数据来自豆瓣API，目前豆瓣已经无法调用，所以选择了其他的源：[豆瓣API](https://github.com/zce/douban-api-docs/blob/master/docs/movie.md)

电影页大量使用 template 模板实现代码复用。

### 电影页首页

模板嵌套： movie-list -> movie-item -> star

电影页重点是数据的请求与处理:

```
//在util.js中封装一个发送网络请求函数
function _http(url, callback) {
  wx.request({
    url: url,
    method: 'GET',
    header: {
      'content-type': 'json'
    },
    success: function (res) {
      if (res.data) {
        callback(res.data)
      }
    }
  })
}
```

在 `movie.js` 中只需调用即可：

```
var top250Url = app.globalData.doubanDataUrl + '/v2/movie/top250?start=0&&count=3';
this.getMovieData(top250Url, 'top250');
getMovieData: function(url, sectionType) {
	var _this = this;
	wx.request({
	  url: url,
	  method: 'GET',
	  header: {
	    'content-type': 'json'
	  },
	  success: function(res) {
	    if (res.data) {
	      _this.processDoubanData(res.data, sectionType);
	    }
	  }
	})
},
```

processDoubanData 为成功后数据处理函数，主要获取电影的一些信息。

通过 setData 在页面中获取，一层一层往模板里传递即可。

+ onMoreTap：点击更多跳转到 movie-more 页面;
+ onMovieTap：点击电影跳转电影详情页 movie-detail;

### 更多电影页

引入 template-movie-grid 模板，进入到相应的页面后，通过点击更多传过来的 `sectionType` 来判断是哪个 更多 按钮进来的，日然后通过 `options.sectionType` 来发起不同的网络请求拿到对应的数据进行页面渲染：


页面实现的有下拉刷新和上拉加载，可通过小程序自带的 `onPullDownRefresh` 和 `onReachBottom` 处理函数实现：

```
//页面相关事件处理函数--监听用户下拉动作
onPullDownRefresh: function(){
  var refreshUrl = this.data.requestUrl + '?start=0&&count=20';
  this.data.movieLists = {};
  this.data.isFirstLoad = true;  //判断是否为第一次加载，第一次加载20条，非第一次加载拼接之前的数据
  this.data.totalCount = 0;		//总条目数清零
  util._http(refreshUrl, this.processDoubanData);
  wx.showNavigationBarLoading();	//在当前页面显示导航条加载动画
}
```

```
//页面上拉触底事件的处理函数
onReachBottom: funtion() {
  if(this.data.totalCount > this.data.pageCount) {
    var moreUrl = this.data.requestUrl + "?start=" + this.data.pageCount + "&&count=20";   //触底上拉请求20条数据
    util._http(moreUrl, this.processDoubanData);
    wx.showNavigationBarLoading();
  } else {
    wx.showToast({
      title: '没有更多数据',
      icon: 'none'
    })
  }
}
```

### 电影详情页

通过路由带的参数发特定的请求取到电影详情：

```
var detailUrl = app.globalData.doubanDataUrl + '/v2/movie/subject/' + movieId;
util._http(detailUrl, this.processDetailData);
```

点击图片预览大图： 

```
var postUrl = event.currentTarget.dataset.poster;
wx.previewImage({
  urls: [postUrl],
})
```

### 评分转星级

```
//豆瓣电影评分转星级
function ratingToStar(rating) {
  var rating = rating.toString().substring(0,1);
  var starArr = []
  for(var i=1; i<=5; i++) {
    if(i<=rating) {
      starArr.push(1);
    }else{
      starArr.push(0);
    }
  }
  return starArr;
}
```

结果为一个五位数组，例如：`[1, 1, 1, 0, 0]`，1代表全星，0代表无星，如果需要半星，只需再加一个数字进行表示，在 wxml 中进行判断即可。

```
<block wx:for="{{stars}}" wx:key="item">
  <image wx:if="{{item}}" src="/images/icon/star.png"></image>
  <image wx:else src="/images/icon/none-star.png"></image>
</block>
```

### 电影搜索

由于搜索接口不能调，所以用了一个不太好使的接口，时常出问题。大致思路是获取到 input 框的值，然后拼接成 url 发送请求。


```
onSearchConfirm:function(event) {
  var searchTxt = event.detail.value;
  var searchUrl = app.globalData.searchDoubanDataUrl + '/v2/movie/search?q=' + searchTxt;
  this.getMovieData(searchUrl, 'search');
},
```