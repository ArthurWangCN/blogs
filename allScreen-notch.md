---
title: 华为刘海屏适配官方技术指导
date: 2018-06-07 10:44:33
tags: [Android, 适配]
categories: Android
---
>现在市场上主流的刘海屏设计，是屏幕的正上方居中位置（下图黑色区域）挖出一个孔，屏幕被挖掉的区域无法正常显示内容，这种类型的屏幕就是刘海屏，也有其他叫法：挖孔屏、凹凸屏等等，本文统一按刘海屏(Notch)命名。

## 刘海屏机型介绍
### iPhoneX
5.8 英寸,屏幕分辨率为 1242 x 2800，屏幕比例20:9

![iphoneX](http://blogpic.at15cm.com/allscreen-iphoneX.jpeg)

### 夏普S2
5.5 吋 1080*2040, Full HD，显示比例是17:9，刘海高度是121px。

![夏普S2](http://blogpic.at15cm.com/allscreen-sharpS2.jpeg)

### Vivo X21
6.28英寸 FHD+ 分辨率：2280 x 1080

![Vivo X21](http://blogpic.at15cm.com/allscreen-vivoX21.jpeg)

### 华为刘海屏
2018年上半年已经发布的华为旗舰手机P20和Nova e3就是刘海屏产品，预计销量在1000w+；后面荣耀旗舰的刘海屏产品也会陆续发布，销量都在1000w+的级别。华为刘海屏问题设计理念：尽量减少APP的开发工作量处理逻辑：

![flowChart](http://blogpic.at15cm.com/allscreen-flowChart.jpeg)


## 系统刘海屏方案导致的问题
谷歌要求如果应用未适配刘海屏，需要系统对全屏显示的应用界面做特殊移动处理（竖屏下移处理，横屏右移处理），如果应用页面布局不能做到自适应，就会出现布局问题；如果应用布局能够做到自适应，也会有黑边无法全屏显示的体验问题，建议应用适配刘海屏解决：

（1）下移导致的小说页码和内容出现截断：

![内容出现截断](http://blogpic.at15cm.com/allscreen-bug1.jpeg)

（2）下移导致相机预览界面布局问题：

![相机预览界面布局](http://blogpic.at15cm.com/allscreen-bug2.jpeg)

（3）下移导致的黑边问题（所有没有状态栏的竖屏界面都有这个问题）。

![黑边问题](http://blogpic.at15cm.com/allscreen-bug3.jpeg)

（4）右移导致的黑边问题（所有横屏页面都会有这个体验问题）

![黑边问题](http://blogpic.at15cm.com/allscreen-bug4.jpeg)


## 非系统刘海屏方案导致的问题

（1）状态栏背景高度写死的问题

![状态栏背景高度写死](http://blogpic.at15cm.com/allscreen-bug5.jpeg)

（2）应用搜索框出现遮挡问题

![搜索框出现遮挡](http://blogpic.at15cm.com/allscreen-bug6.jpeg)

## 应用搜索框出现遮挡问题华为刘海屏适配方案

### O版本适配
谷歌未提供统一方案，需要使用华为提供的刘海屏SDK进行适配，同时华为刘海屏SDK方案也会继承到华为P版本，在华为P版本中将同时支持两种方案：华为O版本方案+谷歌P版本方案；

### P版本适配
谷歌已经提供统一的适配方案，建议应用采用谷歌统一的方案进行适配。本文只涉及华为提供的O版本刘海屏适配方案

### 华为刘海屏API
华为提供了刘海屏API，可以通过反射的方式调用。

#### 判断是否刘海屏接口

（1）接口描述：

类文件： `com.huawei.android.util.HwNotchSizeUtil` 

接口： `public static boolean hasNotchInScreen()` 

接口说明：是否是刘海屏手机(true：是刘海屏,false：非刘海屏)


（2）调用范例参考：

```
public static boolean hasNotchInScreen(Context context) 
{ 
    boolean ret = false; 
    try 
    { 
        ClassLoader cl = context.getClassLoader(); 
        Class HwNotchSizeUtil = cl.loadClass("com.huawei.android.util.HwNotchSizeUtil"); 
        Method get = HwNotchSizeUtil.getMethod("hasNotchInScreen"); 
        ret = (boolean) get.invoke(HwNotchSizeUtil); 
    } 
    catch (ClassNotFoundException e) 
    { 
        Log.e("test", "hasNotchInScreen ClassNotFoundException"); 
    } 
    catch (NoSuchMethodException e) 
    { 
        Log.e("test", "hasNotchInScreen NoSuchMethodException"); 
    } catch (Exception e) 
    { 
        Log.e("test", "hasNotchInScreen Exception"); 
    } 
    finally 
    { 
        return ret; 
    } 
}```


#### 获取刘海尺信息接口

（1）接口描述：

类文件： `com.huawei.android.util.HwNotchSizeUtil`

接口 ： `public static int[] getNotchSize()` 

接口说明： 获取刘海尺：width、height(int[0]值为刘海宽度；int[1]值为刘海高度)

（2）调用范例参考：

```
public static int[] getNotchSize(Context context) 
{ 
    int[] ret = new int[]{0, 0}; 
    try 
    { 
        ClassLoader cl = context.getClassLoader(); 
        Class HwNotchSizeUtil = cl.loadClass("com.huawei.android.util.HwNotchSizeUtil"); 
        Method get = HwNotchSizeUtil.getMethod("getNotchSize"); 
        ret = (int[]) get.invoke(HwNotchSizeUtil); 
    } 
    catch (ClassNotFoundException e) 
    { 
        Log.e("test", "getNotchSize ClassNotFoundException"); 
    } 
    catch (NoSuchMethodException e) 
    { 
        Log.e("test", "getNotchSize NoSuchMethodException"); 
    } 
    catch (Exception e) 
    { 
        Log.e("test", "getNotchSize Exception"); 
    } 
    finally 
    { 
        return ret; 
    }
}```


#### 应用页面设置使用刘海区显示

（1）方案一： 华为新增的 `Meta-data` 属性 `android.notch_support` 在应用的 `AndroidManifest.xml` 中增加 `meta-data` 属性，此属性不仅可以针对 Application 生效，也可以对 Activity 配置生效，具体方式如下所示：

①对Application生效，意味着该应用的所有页面，系统都不会做竖屏场景的特殊下移或者是横屏场景的右移特殊

处理：

![code1](http://blogpic.at15cm.com/allscreen-code1.jpeg)

② 对Activity生效，意味着可以针对单个页面进行刘海屏适配，设置了该属性的Activity系统将不会做特殊处理：

![code2](http://blogpic.at15cm.com/allscreen-code2.jpeg)


（2）方案二：给 `window` 添加华为新增的 `FLAG_NOTCH_SUPPORT` 方式

①接口描述：

类文件： `com.huawei.android.view.LayoutParamsEx`

接口： `public void addHwFlags(int hwFlags)`

接口说明： 通过添加窗口FLAG的方式设置页面使用刘海区： `public static final int FLAG_NOTCH_SUPPORT=0x00010000;`

②调用范例参考：

```
/*刘海屏全屏显示FLAG*/
public static final int FLAG_NOTCH_SUPPORT=0x00010000;
/** 
  * 设置应用窗口在华为刘海屏手机使用挖孔区 
  * @param window 应用页面window对象 
*/
public static void setFullScreenWindowLayoutInDisplayCutout(Window window) 
{ 
    if (window == null) { return; } 
    WindowManager.LayoutParams layoutParams = window.getAttributes(); 
    try 
    { 
        Class layoutParamsExCls = Class.forName("com.huawei.android.view.LayoutParamsEx"); 
        Constructor con=layoutParamsExCls.getConstructor(LayoutParams.class); 
        Object layoutParamsExObj=con.newInstance(layoutParams); 
        Method method=layoutParamsExCls.getMethod("addHwFlags", int.class); 
        method.invoke(layoutParamsExObj, FLAG_NOTCH_SUPPORT); 
    } 
    catch (ClassNotFoundException | NoSuchMethodException | IllegalAccessException |InstantiationException | InvocationTargetException e) 
    { 
        Log.e("test", "hw notch screen flag api error"); 
    } 
    catch (Exception e) { 
        Log.e("test", "other Exception"); 
    }
}```

使用和不使用刘海区效果对比

![solution1](http://blogpic.at15cm.com/allscreen-solution1.jpeg)

设置使用刘海区效果图

![设置使用刘海区效果图](http://blogpic.at15cm.com/allscreen-solution2.jpeg)

不使用刘海区的效果图

![不使用刘海区的效果图](http://blogpic.at15cm.com/allscreen-solution3.jpeg)


## UI适配
通过增加上面适配方案提到的配置（`meta-data` 或者是 `Flag`），应用在华为刘海屏手机上就能够默认使用刘海区显示了，但是为了避免出现UI被刘海区遮挡的问题，还是需要应用自己做一些额外的UI适配工作：

（1）判断是否刘海屏，通过华为刘海屏SDK的API判断

（2）如果是刘海屏手机需要应用自己调整布局避开刘海区

布局原则：保证重要的文字、图片和视频信息、可点击的控件和图标还有应用弹窗等等布局建议显示在状态栏区域以下（安全区域）；不重要，遮挡不会出现问题的布局可以延伸到状态栏区域（危险区域）显示，按照这种布局原则修改，可以一次修改就能适配所有的刘海屏手机：

![布局原则](http://blogpic.at15cm.com/allscreen-solution4.jpeg)

获取系统状态栏高度接口：

```
public static int getStatusBarHeight(Context context) 
{ 
    int result = 0; 
    int resourceId = context.getResources().getIdentifier("status_bar_height", "dimen", "android"); 
    if (resourceId > 0) 
    { 
        result = context.getResources().getDimensionPixelSize(resourceId); 
    } 
    return result;
}```


窗口显示在状态栏区域以下的另外一个原因：

华为侧在设置-显示-显示区域控制，用户可以选择“隐藏显示区域”，该模式下，屏幕上方不规则开孔区域将不作为显示区域，并以黑色填充。

![隐藏显示区域](http://blogpic.at15cm.com/allscreen-solution5.jpeg)

验证方法

有意愿适配的app可以申请华为刘海屏样机进行适配调试，需要提供样机接收人信息，我们尽快安排手机投递，信息包括：公司、姓名、电话、邮编和地址，邮件反馈到：hwthirdparty@huawei.com。暂时无法获取样机的App可以使用华为终端开放实验室的远程真机调试。

新用户如需申请使用华为终端开放实验室的云测功能，需加入安卓绿色联盟，成为会员。然后通过以下步骤申请：

（1）登录 <https://deveco.huawei.com/>；

（2）使用所在公司尾缀的邮箱进行账号注册；

（3）将您新申请的账号、所在公司、个人姓名及电话、负责的应用名称发送至deveco@huawei.com，申请成为安卓绿色联盟会员，通过审核后，将为您开通使用权限。

用注册好的账号登录并选择刘海屏手机：nova 3e（屏幕纵横比：2.11）

![远程真机调试](http://blogpic.at15cm.com/allscreen-solution6.jpeg)

点击“立即体验”，上传APK进行调试。

![远程真机调试](http://blogpic.at15cm.com/allscreen-solution7.jpeg)


## 参考文献
+ [华为刘海屏适配官方技术指导](http://mini.eastday.com/bdmip/180411011257629.html)
+ [最详细的Android P版本刘海屏适配指南来了](http://developer.huawei.com/ict/forum/thread-48787.html)
