---
title: PHP第一季（五） 数组
date: 2017-04-02 00:29:09
tags: [PHP,李炎恢]
categories: 后台
---
传统上把数组(array)定义为一组有某种共同特性的元素，包括相似性和类型。每个元素由一个特殊的标识符来区分，称之为 `键`(key)；而每个键对应一个 `值`(value)。

![表格](http://blogpic.at15cm.com/%E6%95%B0%E7%BB%84%E8%A1%A8%E6%A0%BC.png)
根据上表创建一个数组

```
$userNames = array('JLin','SCurry','KIvring','CPaul','KThompson');
```
这是索引数组初始化：
数字索引的初始值是从0开始计算的：
userNames[0]-userNames[4]，代表这5个人的名字(喜欢的NBA球星)

还有一种是通过 `range()` 函数自动创建一个数组

```
$numbers=range(1,10);    
$letters=range('a','z');
```
可以加第三个参数 `$numbers=range(1,10,2)`，第三个参数表示步长

### 访问数组的内容
要访问一个变量的内容，可以直接使用其名称。如果该变量是一个数组，可以使用变量名称和关键字或索引的组合来访问其内容。
```
$numbers[0]、$numbers[1]、$numbers[2];
```

### 改变数组的值
```
$numbers[0]="DRose";
```

### 使用循环访问数组
由于数组使用有序的数字作为索引，所以使用一个for循环就可以很容易地显示数组的内容：

```
for ($i=0;$i<10;$i++) {
	echo $numbers[$i];
}
```

也可以使用foreach循环来遍历数组：

```
foreach ($numbers as $value) {
	echo $value;
}
```
测试是否为数组变量：is_array();
print_r 函数：打印关于变量的易于理解的信息

## 自定义键数组
### 初始化相关数组
```
$players=('JLin'=>7,'SCurry'=>30,'KThompson'=>11);
```
### 访问数组元素
```
$players["JLin"];
```
### 追加数组
首先，创建只有一个元素的数组，然后追加元素。
```
$players = array("JLin"=>7);
$players["SCurry"]=30;
```

### 直接添加数组
无需创建，直接添加，添加第一个元素自动建立数组。
```
$players["KThompson"]=11;
```

### 使用循环语句
因为相关数组的索引不是数字，因此无法使用for循环语句中使用一个简单的计数器对数组进行操作。但是可以使用foreach循环或list()和each()结构。
+ foreach
```
foreach ($players as $key=>$value) {
	echo $key."=>".$value."<br />";
}
```
+ each()
each()函数返回数组的当前的键值对，并将数组指针向前移动一步；
这里有一个指针，默认情况下指针指向第一个键值对；
这里第一个键值对是 'JLin'=>7；
如果each($numbers)，那么获取的就是第一个键值对 'JLin'=>7；
each()这个函数返回的是一个数组；
each()将第一个键值对获取到，然后包装成一个新数组；
```
//用布尔值的理念
while (!!$element=each($players)) {
	echo $element["key"];
	echo "=>";
	echo $element["value"];
	echo "<br />";
}
```

+ list()函数
list()函数用来将一个数组分解为一系列的值。
可以按照如下方式将函数each()返回的两个值分开：
```
list($name,$age)=each($ages);
```
当使用each()函数时，数组将记录当前元素。如果希望在相同的脚本中两次使用该数组，就必须使用函数reset()将当前元素重新设置到数组开始处。
```
reset($prices);
```

### 确定唯一的数组元素
array_unique();
它会删除掉里面相同值的元素。

### 置换数组键和值
array_flip();
它会对调数组中的key和value;


## 数组里的数组
>数组不一定就是一个关键字和值的简单列表----数组中的每个位置用来保存另一个数组。使用这种方法，可以创建一个二维数组。可以把二维数组当成一个矩阵，或者是一个具有宽度和高度或者行和列的网格。

|  球员   |   球衣号  |  球队  |
|:------:|:-------:|:--------:|
|  JLin  |   7     |   NETS |
|  SCurry|   30    | WARRIORS |
|  PGergeo|  13    | Pacers  |

```
$players = array(
	array("JLin",7,"NETS");
	array("SCurry",30,"WARRIORS");
	array("PGergeo",13,"Pacers");
);
```

显示这个二维数组：
```
echo "|".$players[0][0]."|".$players[0][1]."|".$players[0][2]."|<br />";
echo "|".$players[1][0]."|".$players[1][1]."|".$players[1][2]."|<br />";
echo "|".$players[2][0]."|".$players[2][1]."|".$players[2][2]."|<br />";
```

此外，还可以使用双重for循环来实现同样的效果：
```
for ($row=0;$row<3;$row++) {
	for ($column=0;$column<3;$column++) {
		echo "|".$players[$row][$column];
	}
	echo "|<br />";
}
```

**使用列明的二维数组：**
```
$players=array(	
	array("球员"=>"JLin","球衣号"=>7,"球队"=>"NETS"),
	array("球员"=>"SCurry","球衣号"=>30,"球队"=>"WARRIORS"),
	array("球员"=>"PGergeo","球衣号"=>13,"球队"=>"Pacers")
	);
```

显示这个二维数组：
第一种方式：
```
for ($row=0;$row<3;$row++) {
	echo "|".$products[$row]["产品名"]."|".
$products[$row]["数量"]."|".$products[$row]["价格"]."|<br />";
}
```

第二种方式：
```
for ($row=0;$row<3;$row++) {
	while (!!list($key, $value)=each($products[$row])) {
		echo "|".$value;
	}
	echo "|<br />";
	}
```

## 数组的排序
+ sort()
使用sort()函数将数组按字母升序进行排序。
```
$products=array("orange","banner","apple");
sort($products);
```

使用sort()函数将数字升序进行排序。
```
$prices=array(100,10,4,23,78);
sort($prices);
```

sort()函数的第二个参数是可选的。这个可选参数可以传递`SORT_REGULAR`（默认值）、`SORT_NUMERIC` 或 `SORT_STRING`。指定排序类型的功能是非常有用的。比如，当要比较可能包含有数字2和12的字符串时，从数字角度看，2要小于12，但是作为字符串，"12"却要小于"2"。

+ asort()
对数组进行排序并保持索引关系
```
$prices=array("c"=>苹果,"a"=>猪肉,"b"=>饼干);
asort($prices);
```

+ ksort()
对数组按照键名排序
```
ksort($prices)
```

反向排序：sort()、asort()和ksort()都是正向排序，当然也有相对应的反向排序.
实现反向：rsort()、arsort()和krsort()。

+ shuffle()
在一些应用程序中，可能希望按另一种方式对数组排序。函数shuffle()将数组个元素进行随机排序。
	
+ array_reverse()
返回单元顺序相反的数组

+ array_unshift()
将新元素添加到数组头

+ array_push()
将每个新元素添加到数组的末尾

+ array_shift()
删除数组头第一个元素

+ array_pop()
删除并返回数组末尾的一个元素

+ array_rand()
返回数组中的一个或多个键

## 数组的指针操作
在数组中浏览：each()、current()、reset()、end()、next()、pos()、prev();
调用next()或each()将使指针前移一个元素。
调用each($array_name)会在指针前移一个位置之前返回当前元素。
调用next($array_name)是将指针前移，然后再返回新的当前元素。
要反向遍历一个数组，可以使用end()和prev()函数。prev()函数和next()函数相反。它是将当前指针往回移一个位置然后再返回新的当前元素。

## 统计数组个数
count()和sizeof()统计数组下标的个数
array_count_values()统计数组内下标值的个数

## 将数组转换成标量变量
extract()——从数组中将变量导入到当前的符号表
对于一个非数字索引数组，而该数组又有许多关键字-值对，可以使用函数extract()将它们转换成一系列的标量变量。extract()函数原型如下：
```
extract(array var_array,[int extract_type],[string prefix]);
```
函数extract()的作用是通过一个数组创建一系列的标量变量，这些变量的名称必须是数组中关键字的名称，而变量值则是数组中的值。
```
$array=array("key1"=>"value1","key2"=>"value2","key3"=>"value3");
extract($array);
echo $key1.$key2.$key3;
```
 
