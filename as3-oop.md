---
title: AS3:类和对象
date: 2018-09-12 21:46:56
tags: AS3
categories: 游戏开发
---
## 第一个类的例子

```
package
{
  public class Hello
  {
    public var helloString:String = "world";
    public function Hello() {
    }
    public function sayHello():void {
      trace("Hello," + helloString + "!");
    }
  }
}
```

AS3中 `class` 如果想要被外部访问，必须放在一个 `package` 中。

package 花括号中只能放一个 class，但同一个 .as 文件花括号外可以有多个包外类。


## class 和 object 的创建和使用
### 创建class
1. 选定项目目录下的一个子目录，即一个 package。创建一个 .as 文件，文件名为Class的名字。例如SampleClass。
2. 文件的开头写入 **package** 关键字和package的路径。比如SampleClass.as在的项目目录下的一个叫sample的子目录中，写成 package sample {}。
3. 在 package 花括号里，写入 **class** 关键字和类的名字。例如：class SampleClass {}。如果需要其他类，在class前 import 。例如：import other.OtherClass。
4. 在 class 关键字后的花括号里写入类的定义。


```
package 包路径 {
  public class 类名 {
    //静态属性
    //静态方法
    //实例属性
    //构造函数
    //实例方法
  }
}
```


### 创建类的实例
(1) 使用 **import** 关键字导入所需要的类

```
import sample.SampleClass;
```

(2) 使用 **new** 关键字加上类的构造函数

```
var sampleObject1 = new SampleClass();
var sampleObject2:SampleClass = new SampleClass();		//推荐
```

## 实例属性和实例方法

+ 实例属性：存储值，这些值用来描述每个对象的状态。
+ 实例方法：描述这个实例有哪些行为，以函数形式存在。

必须先创建实例，在访问实例属性和方法。

### 声明实例属性

```
var 属性名称:属性类型;
var 属性名称:属性类型 = 值;
```

可在var前加internal、public、private、protect，如果不加默认 **internal**，包内成员可以访问，包外不可以。

### 声明实例方法

```
function 方法名称(参数...): 返回值类型 {
  //方法内容
}
```

可加访问控制，默认 **internal**。


还有一种声明方式是将方法声明为 **Function** 类型的实例属性：

```
var 方法名称:Function = function (参数...): 返回值类型 {
  //方法内容
}
```

### 访问实例属性和方法

可用 `.` 运算符和 `[]` 运算符来访问。

如果希望在运行时再决定使用哪个属性或方法可使用 `[]` 运算符，注意 `[]` 运算符中是字符串。

```
foo.sampleA
foo.sampleMethod()
foo['sample' + getLetter()]
```



## 静态属性和静态方法

静态属性和静态方法不依赖实例而独立存在。

静态属性和静态方法只为每个类创建一次，再这个类被调用是创建。

经常应用于工厂模式、singleton模式、享元模式。

### 声明静态属性和静态方法

可加访问控制，默认 **internal**：

```
static var 属性:属性类型;
static var 属性:属性类型 = 值;
```

如果声明常量用 **static** 和 **const**，并且声明必须赋值。尝试在其他地方赋值就会报错。

```
static const 属性:属性类型 = 值;
```

静态方法：

```
static function 方法名称(参数...): 返回值类型 {
  //方法内容
}
```

也可用Function类型来声明，但是很少用。

### 访问静态属性和静态方法

```
package org.book.basicoop.staticSample {
  public class StaticSample {
    public static var CLASSNAME:String = "staticSample";
    public static function hello():void {
      trace("static hello");
    }
    public function accessStatic():void {
      trace(CLASSNAME);
      hello();
      trace(StaticSample.CLASSNAME);	//推荐类内访问
      StaticSample.hello();		//推荐类内访问
    }
  }
}
```

类外访问：

```
import org.book.basicoop.staticSample.StaticSample;
trace(StaticSample.CLASSNAME);
StaticSample.hello();
```

不能通过实例来访问静态成员，编译时会报错。

同名会优先访问实例属性和实例方法。

### 应用

+ 使用静态属性集中管理数据
+ 使用静态属性部分市县Enumeration
+ 实现工具类


## 构造函数
AS3中，创建对象自动调用构造函数，删除由垃圾回收机决定。

如果类中没有构造函数，编译器会在编译时自动生成一个默认的空的构造函数。

构造函数可以有参数。

构造函数不支持重载。

构造函数只能用 **public** 访问控制，也可省略不写。

构造函数不能声明返回类型，必须留空。只有当new关键字加上构造函数一起使用时才会返回类的实例。

但是构造函数中可以使用 **return**，但是不能返回东西，可使用return来进行流程控制。


## 动态类和密封类

从是否可以添加实例属性或方法可将类分为 **动态类** 和 **密封类**。

通过类名前 **dynamic** 关键字区分。

注意：运行时加上的方法只能访问对象的公共成员。

for...in 和 for each...in 只能遍历动态类对象的动态属性。


## this
>this关键字持有对当前对象的引用

