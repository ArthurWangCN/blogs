---
title: AS3：implements 与 interface
date: 2018-08-11 00:42:56
tags: [游戏开发, AS3]
categories: 游戏开发
---
## implements
指定类可以实现一个或多个接口。

### 用法
myClass implements interface01 [, interface02 , …]

### 语言版本
ActionScript 3.0

RuntimeVersions: Flash Player 9

指定类可以实现一个或多个接口。当类在实现某个接口时，此类必须定义该接口中声明的所有方法。 实现接口的类的任何实例都被视为此接口定义的数据类型中的成员。因此，如果类实例是第一个操作数，并且接口为第二个操作数，is 运算符就会返回 true；此外，还会基于由接口定义的数据类型进行强制类型转换。

注意：若要使用此关键字，必须在 FLA 文件的“Publish Settings”对话框的“Flash”选项卡上指定 ActionScript 2.0 和 Flash Player 6 或更高版本。仅支持在外部脚本文件中使用此关键字，而不支持在使用“Actions”面板编写的脚本中使用此关键字。


## interface
定义接口

### 用法
interface InterfaceName [extends InterfaceName ] {}

### 语言版本
ActionScript 3.0

RuntimeVersions: Flash Player 9

定义接口。接口是定义了一组方法的数据类型；这些方法必须由实现接口的任意类定义。

接口与类相似，但存在以下重要区别：

接口仅包含方法的声明，而不包含其实现。也就是说，实现接口的每个类都必须为该接口中声明的每个方法提供实现。

接口方法定义不能包含任何属性（如 public 或 private），但在实现接口的类的定义中，已实现的方法必须标记为 public。

通过 extends 语句可以使用一个接口继承多个接口，通过 implements 语句可以使用一个类继承多个接口。
与 ActionScript 2.0 不同，ActionScript 3.0 允许在接口定义中使用 getter 和 setter 方法。

注意：若要使用此关键字，必须在 FLA 文件的“Publish Settings”对话框的“Flash”选项卡上指定 ActionScript 2.0 和 Flash Player 6 或更高版本。仅支持在外部脚本