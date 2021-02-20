---
title: CSS 回顾
date: 2020-02-12 15:15:45
tags: CSS
categories: [前端, CSS]
---

## CSS选择器

### 选择器分类

+ 元素选择器 `a{}`
+ 伪元素选择器 `::before{}`
+ 类选择器 `.link{}`
+ 属性选择器 `[type=radio]{}`
+ 伪类选择器 `:hover{}`
+ ID选择器 `#id{}`
+ 组合选择器 `[type=checkbox] + label{}`
+ 否定选择器 `:not(.link){}`
+ 通用选择器 `*{}`

### 选择器权重
+ ID选择器 +100
+ 类 属性 伪类 +10
+ 元素 伪元素  +1
+ 其它选择器 +0
+ important优先级最高
+ 元素属性优先级高
+ 相同权重后写的生效

##非布局样式

### 字体族
+ serif sans-serif monospace cursive fantasy
+ 多字体fallback
+ 网络字体、自定义字体
+ iconfont

### 行高
行高是由 line box 决定的，line box 是由 inline box 组成的。
inline box的高度会决定行高的高度。

行内默认对准是 baseline 对齐，baseline 和底线是有偏差的。底线对齐：

```
vertical-align: bottom;
```

### 背景
+ 背景颜色
+ 渐变色背景
+ 多背景叠加
+ 背景图片和属性(雪碧图)
+ base64和性能优化
+ 多分辨率适配

### 边框
+ 边框的属性:线型 大小 颜色
+ 边框背景图
+ 边框衔接(三角形)

### 滚动

overflow: visible/hidden/scroll/auto

### 文字折行
+ overflow-wrap(word-wrap)通用换行控制，是否保留单词
+ word- break针对多字节文字，中文句子也是单词
+ white-space空白处是否断行

### 装饰性属性及其它
+ 字重(粗体) font-weight
+ 斜体 font-style: itatic
+ 下划线 text-decoration
+ 指针 cursor

### CSS Hack
+ Hack即不合法但生效的写法
+ 主要用于区分不同浏览器
+ 缺点:难理解难维护易失效
+ 替代方案:特性检测
+ 替代方案:针对性加 class

### 伪类和伪元素的区别?
+ 伪类表状态
+ 伪元素是真的有元素
+ 前者单冒号,后者双冒号


## CSS布局

### 表格布局

### flexbox布局

+ 弹性盒子
+ 盒子本来就是并列的
+ 指定宽度即可


### float布局

+ 元素“浮动”
+ 脱离文档流
+ 但不脱离文本流
+ 对自身的影响:
	+ 形成“块”(BFc)
	+ 位置尽量靠上
	+ 位置尽量靠左(右)
+ 对兄弟的影响
	+ 上面贴非 float 元素
	+ 旁边贴 float 元素
	+ 不影响其它块级元素位置
	+ 影响其它块级元素內部文本
+ 对父级元素的影响
	+ 从布局上“消失”
	+ 高度塌陷

### inline-block布局

+ 像文本一样排 block 元素
+ 没有清除浮动等问题
+ 需要处理间隙 `font-size: 0;`

## 一些问题
### 实现两栏(三栏)布局的方法
+ 表格布局
+ float + margin布局
+ inline-bock布局
+ flexbox布局

### position: absolute/ fixed有什么区别?
+ 前者相对最近的 absolute、relative
+ 后者相对屏幕( viewport)

### display: inline- block的间隙
原因:字符间距
解决:消灭字符或者消灭间距

### 如何清除浮动
+ 让盒子负责自己的布局
+ `overflow: hidden(auto)`
+ `::after(clear: both)`

### 如何适配移动端页面?
+ viewport
+ rem/viewport /media query
+ 设计上:隐藏 折行 自适应


## CSS效果

### box shadow
+ 营造层次感(立体感)
+ 充当没有宽度的边框
+ 特殊效果(画叮当猫)

### text-shadow
+ 立体感
+ 印刷品质感

### border-radius
+ 圆角矩形
+ 圆形
+ 半圆/扇形
+ 一些奇怪的角角

### background

+ 纹理、图案
+ 渐变
+ 雪碧图动画
+ 背景图尺寸适应

### clip-path

+ 对容器进行裁剪
+ 常见几何图形
+ 自定义路径

### 3D-transform

+ 3D空间进行变换

### 一些问题

#### 用一个div画XXX

+ box-shadow无限投影
+ ::before
+ ::after

#### 如何产生不占空间的边框

+ box-shadow
+ outline

#### 如何实现ios图标的圆角

`clip-path:(svg)`

#### 实现3D效果

```
1.perspective: 500px
2.transform-style: preserve-3d
3.transfrom: translate rotate ...
```

## CSS动画

### 补间动画

+ 位置 平移 (left/right/margin/transform)
+ 方位 旋转(transform)
+ 大小 缩放 (transform)
+ 透明度 (opacity)
+ 其它-线性变换(transform)

### 关键帧动画

+ 相当于多个补间动画
+ 与元素状态的变化无关
+ 定义更灵活

### 逐帧动画

+ 适用于无法补间计算的动画
+ 资源较大
+ 使用steps()

### 一些问题

#### CSS动画的实现方式有几种
+ transition
+ keyframes(animation)

#### 过渡动画和关键帧动画的区别

+ 过渡动画需要有状态变化
+ 关键帧动画不需要状态变化
+ 关键帧动画能控制更精细 


#### 如何实现逐帧动画

+ 使用关键帧动画
+ 去掉补间( steps)

#### CSS动画的性能

1. 性能不坏
2. 部分情况下优于JS
3. 但JS可以做到更好
4. 部分高危属性  box-shadow等


## CSS预处理器

+ 基于CSS的另一种语言
+ 通过工具编译成CSS
+ 添加了很多CSS不具备的特性
+ 能提升CSS文件的组织
+ 嵌套 反映层级和约束
+ 变量和计算 减少重复代码
+ Extend和Mixin 代码片段
+ 循环 适用于复杂有规律的样式
+ import CSS 文件模块化

### 一些问题

#### 常见的CSS预处理器
+ Less(Nodejs)
+ sass(Ruby有Node版本)

#### 预处理器的作用

+ 帮助更好地组织cSS代码
+ 提高代码复用率
+ 提升可维护性

#### 预处理器的能力

+ 嵌套反映层级和约束
+ 变量和计算减少重复代码
+ Extend和 Mixin代码片段
+ 循环适用于复杂有规律的样式
+ import CSS文件模块化

#### 预处理器的优缺点

+ 优点:提高代码复用率和可维护性
+ 缺点:需要引入编译过程有学习成本


## CSS工程化方案

### Postcss

+ PostCSS本身只有解析能力
+ 各种神奇的特性全靠插件
+ 目前至少有200多个插件
+ import模块合井
+ autoprefixier自动加前缀
+ cssnano压缩代码
+ cssnext使用css新特性
+ precss变量、 mixin、循环等

### webpack和CSS

+ css-loader将CSS变成JS
+ style-loader将JS样式插入head
+ ExtractTextPlugin将CSS从JS中提取出来
+ css modules解决CSS命名冲突的问题
+ less-loader sass-loader各类预处理器
+ postcss-loader Postcss处理

### 一些问题

#### 如何解决CSS模块化问题

+ Less Sass等CSS预处理器
+ PostCSS插件( postcss-import/ precss等)
+ webpack处理CSS(css-loader+ style-loader)

#### PostCSS可以做什么?
+ 取决于插件可以做什么
+ autoprefixer cssnext press等兼容性处理
+ import模块合并
+ css语法检查  兼容性检查
+ 压缩文件

#### CSS modules 是做什么的,如何使用

+ 解决类名冲突的问题
+ 使用 Postcss或者 webpack等构建工具进行编译
+ 在HTML模板中使用编译过程产生的类名

#### 为什么使用JS来引用、加载css

+ JS作为入口,管理资源有天然优势
+ 将组件的结构、样式、行为封装到一起,增强内聚
+ 可以做更多处理( webpack)

