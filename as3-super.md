---
title: AS3：super()的用法
date: 2018-06-04 11:51:00
tags: [游戏开发, AS3]
categories: 游戏开发
---
super是执行父类构造函数,构造方法中如果写了程序，则会调用该程序。如果程序中的方法,子类重写了，则执行重写的方法。

其可以分为两种调用方式，分别为 **隐式调用** 和 **显示调用**：

`super()` 语句显式地调用其直接超类的构造函数。如果未显式调用超类构造函数，编译器会在构造函数体中的第一个语句前自动插入一个调用。还可以使用 super 前缀作为对超类的引用来调用超类的方法。

如果决定在同一构造函数中使用 super() 和 super，务必先调用 super()。否则，super 引用的行为将会与预期不符。另外，super() 构造函数也应在 throw 或 return 语句之前调用。


### 第一种情况：隐式调用
在子类继承自父类的时候，不管你写不写super.()，子类执行了父类的构造函数（所以呢，写构造函数的时候不要写太多东西），例：

```
package
{
  import flash.display.Sprite
  public class SuperExp extends Sprite
  {
    public function SuperExp()
    {
      trace ("调用了父类的构造函数");
    }
  }
}
package
{
  public class main extends SuperExp
  {
    public fuction main()
    {
      //虽然什么都不写，但是还是输出了：调用了父类的构造函数
    }
  }
}```


### 第二种情况：显式调用
这个主要是想用父类的方法，其实可以想象是创建了一个父类的对象，例：

```
package
{
  import flash.display.Sprite
  public class SuperExp extends Sprite
  {
    private var Name:String;
    public function SuperExp()
    {
      trace ("调用了父类的构造函数");
    }
    public function Fun_c(Name:String):void
    {
      trace ("Name")
    }
  }
}
package
{
  public class main extends SuperExp
  {
    public fuction main()
    {
      //虽然什么都不写，但是还是输出了：调用了父类的构造函数
      super.Name="输出了父类的方法"
      super.Fun_c(Name);
      //这里调用了父类的Fun_c方法，和父类的变量，输出了:调用父类的方法
    }
  }
}```