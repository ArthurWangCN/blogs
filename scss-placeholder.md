---
title: 理解Sass的选择占位符%placeholder
date: 2020-05-07 21:08:15
tags: [CSS, Sass]
categories: [前端, CSS]
---

Sass中提供多种方法来共用相同的CSS代码。你可以使用`@include`定义好的[@mixin](http://thesassway.com/intermediate/leveraging-sass-mixins-for-cleaner-code)在你的CSS样式中插入新的CSS样式，你也可以使用@extend定义好的CSS类选择器，向你的CSS样式中插入新的CSS样式。在Sass3.2中引入了一个新的特性——选择器占位符 `%placeholder`，能过`@extend`可以得到更有效的输出。

### @extend 如何工作

使用[`@extend`](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#extend)我们可以使用已定义好的选择器：

```scss
.icon {
  transition: all ease 1s;
  margin: 0 .5em;
}
.error-icon {
  @extend .icon;
  /* error icon style goes here... */
}
.info-icon {
  @extend .icon;
  /* info icon style goes here... */
}
```

编译后：

```css
.icon, .info-icon, .error-icon {
  transition: all ease 1s;
  margin: 0 0.5em;
}
.error-icon {
	/* error icon style goes here... */
}
.info-icon {
  /* info icon style goes here... */
}
```

通过 `@extend` 可以直接在 .error-icon 和 .info-icon 中插入定义好的 .icon 属性。只要你修改了 .icon 样式， .error-icon 和 .info-icon 也会做出对应的变化。

但是我们可能根本用不到 .icon 这个类名对应的样式，我们可以通过使用选择器占位符 `%placeholder` 来解决这种现象。



###  使用选择器占位符%placeholder

选择器占位符`%placeholder`可以很好的解决上面提到的问题。选择器占位符很类似于CSS的类，不同的是他不是使用`(.)`开始，而是使用`(%)`开始，而且编译出来的CSS代码中并不会包括`%placeholder`规则中的样式，除非是通过`@extend`对其进行调用。

```scss
%icon {
  transition: all ease 1s;
  margin: 0 .5em;
}
.error-icon {
  @extend %icon;
  /* error icon style goes here... */
}
.info-icon {
  @extend %icon;
  /* info icon style goes here... */
}
```

编译后

```css
.info-icon, .error-icon {
  transition: all ease 1s;
  margin: 0 0.5em;
}
.error-icon {
  /* error icon style goes here... */
}
.info-icon {
  /* info icon style goes here... */
}
```

注意，编译出来后的CSS代码不再包括 `.icon`。



### @extend 和 @include

乍一看，选择器占位符 `%placeholder` 看起来和具有相同参数的 `@mixin` 一样。虽然从功能上来说（在浏览器上渲染的效果是完全相同的）他们是相同，但编译出来的CSS却大大的不同。

使用 `@mixin .icon` 来实现：

```scss
@mixin icon {
  transition: all ease 1s;
  margin: 0 .5em;
}
.error-icon {
  @include icon;
  /* error icon style goes here... */
}
.info-icon {
  @include icon;
  /* info icon style goes here... */
}
```

编译后：

```css
.error-icon {
  transition: all ease 1s;
  margin: 0 0.5em;
  /* error icon style goes here... */
}
.info-icon {
  transition: all ease 1s;
  margin: 0 0.5em;
  /* info icon style goes here... */
}
```

仅从维护的角度来说，这是一个很好的扩展的示例，但编译出来的CSS实在是糟糕，因为编译出来的CSS样式，没有把相同的样式合并在一起。

### placeholder的局限

使用`@extend`调用定义好的选择器占位符`%placeholder`有所限制，他不能在不同的`@media`中运行。

```scss
%icon {
  transition: all ease 1s;
  margin: 0 .5em;
}
@media screen {
  .error-icon {
    @extend %icon;
    /* error icon style goes here... */
  }
  .info-icon {
    @extend %icon;
    /* info icon style goes here... */
  }
}
```

此时编译器会报错：

```
From line 1, column 1 of stdin: 
  ╷
1 │ %icon {
  │ ^^^^^^
  ╵
You may not @extend selectors across media queries.
  ╷
7 │     @extend %icon;
  │     ^^^^^^^^^^^^^
  ╵
  stdin 7:5  root stylesheet on line 7 at column 5
```

Sass 为什么要这样？

因为`@extend`是将一个选择器样式扩展到另一个选择器当中，而实际上在不同的`@media`中却无需复制这些样式。

虽然他可以通过其他的方式来工作，在`@media`块中定义选择器占位符，在`@extend`调用时，将会将整个样式包含在`@media`区块中：

```scss
@media screen {
	%icon {
		transition: all ease 1s;
		margin: 0 .5em;
	}
}
.error-icon {
	@extend %icon;
}
.info-icon {
	@extend %icon;
}
```

编译后：

```css
@media screen {
  .info-icon, .error-icon {
    transition: all ease 1s;
    margin: 0 0.5em;
  }
}
```



### 总结

`@extend`和`@include`都具有强大的功能，尽管细节上有一些差别，这就要问你自己，编译出来的CSS样式，接近重用的样式对你是不是很重要。在某些情况下,`@extend`可以大大的减化你的CSS输出，并且显著的提高你的CSS性能。



本文来自 [大漠 — sass选择占位符%placeholder](https://www.w3cplus.com/preprocessor/understanding-placeholder-selectors.html)

