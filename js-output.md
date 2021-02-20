---
title: JS一些输出是什么
date: 2020-05-12 22:39:58
tags: JavaScript
categories: [前端, JavaScript]
---

### 1. 暂时性死区
通过 `let` 和 `const` 关键字声明的变量也会提升，但是和 `var` 不同，它们不会被初始化。在我们声明（初始化）之前是不能访问它们的。这个行为被称之为 `暂时性死区`。当我们试图在声明之前访问它们时，JavaScript 将会抛出一个 `ReferenceError` 错误。

```javascript
function sayHi() {
	console.log(name);	// undefined
	console.log(age);	// Uncaught ReferenceError: Cannot access 'age' before initialization
	var name = 'Lydia';
	let age = 20;
}
```

### 2. 箭头函数的this
对于箭头函数，`this` 关键字指向的是它 `当前周围作用域`（简单来说是包含箭头函数的常规函数，如果没有常规函数的话就是全局对象），这个行为和常规函数不同。

```javascript
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2
  },
  perimeter: () => 2 * Math.PI * this.radius
}

shape.diameter()	// 20 常规函数，this就是shape
shape.perimeter()	// NaN 周围作用域window，没有radius，返回undefined
```

### 3. 一元运算符
一元操作符加号尝试将 `boolean` 转为 `number` ：

```javascript
+true	// 1
!"Lydia"	// false
```

### 4. new 一个 Number
`new Number()` 是一个内建的函数构造器。虽然它看着像是一个 number，但它实际上并不是一个真实的 number：它有一堆额外的功能并且它是一个对象。

```javascript
let a = 3
let b = new Number(3)	// Number {3}
let c = 3

console.log(a == b)		// true
console.log(a === b)	// false
console.log(b === c)	// false
```
当我们使用 == 操作符时，它只会检查两者是否拥有相同的值。因为它们的值都是 3，因此返回 true。

当我们使用 `===` 操作符时，两者的值以及类型都应该是相同的。`new Number()` 是一个对象而不是 number，因此返回 false。

### 5. 类的静态方法
静态方法被设计为只能被创建它们的构造器使用，并且不能传递给实例：

```javascript
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor
    return this.newColor
  }

  constructor({ newColor = 'green' } = {}) {
    this.newColor = newColor
  }
}

const freddie = new Chameleon({ newColor: 'purple' })
freddie.colorChange('orange')	// TypeError: freddie.colorChange is not a function
```
+ 通过关键字 `static` 定义静态方法 colorChange。
+ 直接通过类调用静态方法，而不是对象实例调用。
+ freddie.colorChange 之所以会报错，它是调用的实例方法colorChange ，然而并没有定义。


### 6. 给构造函数添加属性
不能像常规对象那样，给构造函数添加属性。如果你想一次性给所有实例添加特性，你应该使用原型。

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person("Lydia", "Hallie");
Person.getFullName = function () {
  return `${this.firstName} ${this.lastName}`;
}

console.log(member.getFullName());	// 报错，member.getFullName is not a function
```

**为什么这样设计？**
假设我们将这个方法添加到构造函数本身里。也许不是每个 Person 实例都需要这个方法。这将浪费大量内存空间，因为它们仍然具有该属性，这将占用每个实例的内存空间。相反，如果我们只将它添加到原型中，那么它只存在于内存中的一个位置，但是所有实例都可以访问它！

### 7. 使用new和不使用的区别

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}

const lydia = new Person('Lydia', 'Hallie')
const sarah = Person('Sarah', 'Smith')

console.log(lydia)	// Person {firstName: "Lydia", lastName: "Hallie"}
console.log(sarah)	// undefined
```

对于 sarah，我们没有使用 new 关键字。当使用 new 时，this 引用我们创建的空对象。当未使用 new 时，this 引用的是全局对象（global object）。

我们说 this.firstName 等于 "Sarah"，并且 this.lastName 等于 "Smith"。实际上我们做的是，定义了 global.firstName = 'Sarah' 和 global.lastName = 'Smith'。而 sarah 本身是 undefined。


### 8. 标记模板字面量
如果使用标记模板字面量，第一个参数的值总是包含字符串的数组。其余的参数获取的是传递的表达式的值:

```javascript
function getPersonInfo(one, two, three) {
  console.log(one)	// ["", " is ", " years old", raw: Array(3)]
  console.log(two)	// Lydia
  console.log(three)	// 21
}

const person = 'Lydia'
const age = 21

getPersonInfo`${person} is ${age} years old`
```

### 9. 对象的键
所有对象的键（不包括 Symbol）在底层都是字符串，即使你自己没有将其作为字符串输入。

```javascript
const obj = { 1: 'a', 2: 'b', 3: 'c' }
const set = new Set([1, 2, 3, 4, 5])

obj.hasOwnProperty('1')		// true
obj.hasOwnProperty(1)		// true
set.has('1')				// false
set.has(1)					// true
```
对于集合，它不是这样工作的。在我们的集合中没有 '1'：set.has('1') 返回 false。它有数字类型为 1，set.has(1) 返回 true。


### 10. 生成器函数
一般的函数在执行之后是不能中途停下的。但是，生成器函数却可以中途“停下”，之后可以再从停下的地方继续。当生成器遇到 `yield` 关键字的时候，会生成 yield 后面的值。注意，生成器在这种情况下不 `返回 (return )` 值，而是 `生成 (yield)` 值。

```javascript
function* generator(i) {
  yield i;
  yield i * 2;
}

const gen = generator(10);

console.log(gen.next().value);	// 10
console.log(gen.next().value);	// 20
```
**代码解析：**
首先，我们用10作为参数i来初始化生成器函数。然后使用next()方法一步步执行生成器。第一次执行生成器的时候，i的值为10，遇到第一个yield关键字，它要生成i的值。此时，生成器“暂停”，生成了10。

然后，我们再执行next()方法。生成器会从刚才暂停的地方继续，这个时候i还是10。于是我们走到了第二个yield关键字处，这时候需要生成的值是i*2，i为10，那么此时生成的值便是20。所以这道题的最终结果是10,20。


### 11. parseInt解析
parseInt只返回了字符串中第一个字母. 设定了 进制 后 (也就是第二个参数，指定需要解析的数字是什么进制: 十进制、十六机制、八进制、二进制等等……),parseInt 检查字符串中的字符是否合法. 一旦遇到一个在指定进制中不合法的字符后，立即停止解析并且忽略后面所有的字符。

```javascript
const num = parseInt("7*6", 10);	// 7
```

*就是不合法的数字字符。所以只解析到"7"，并将其解析为十进制的7. num的值即为7.


### 12. delete操作符
`delete` 操作符返回一个 `布尔值`： true指删除成功，否则返回false. 但是通过 `var`, `const` 或 `let` 关键字声明的变量无法用 `delete` 操作符来删除。

```javascript
const name = "Lydia";
age = 21;

console.log(delete name);	// false
console.log(delete age);	// true
```

name 变量由 const 关键字声明，所以删除不成功:返回 false. 而我们设定age等于21时,我们实际上添加了一个名为age的属性给全局对象。对象中的属性是可以删除的，全局对象也是如此，所以 delete age 返回 true。


### 13. Object.defineProperty

```javascript
const person = { name: "Lydia" };

Object.defineProperty(person, "age", { value: 21 });

console.log(person);	// { name: "Lydia", age: 21 }
console.log(Object.keys(person));	// ["name"]
```

通过 `defineProperty` 方法，我们可以给对象添加一个新属性，或者修改已经存在的属性。而我们使用 `defineProperty` 方法给对象添加了一个属性之后，属性默认为 `不可枚举`(not enumerable). `Object.keys` 方法仅返回对象中 `可枚举`(enumerable) 的属性，因此只剩下了 "name".

用 `defineProperty` 方法添加的属性默认不可变。你可以通过 writable, configurable 和 enumerable 属性来改变这一行为。这样的话， 相比于自己添加的属性，defineProperty 方法添加的属性有了更多的控制权。


### 14. JSON.stringify第二个参数

```javascript
const settings = {
  username: "lydiahallie",
  level: 19,
  health: 90
};

const data = JSON.stringify(settings, ["level", "health"]);
console.log(data);	// "{"level":19, "health":90}"
```

`JSON.stringify` 的第二个参数是 `替代者`(replacer). 替代者(replacer)可以是个函数或数组，用以控制哪些值如何被转换为字符串。

如果替代者(replacer)是个 `数组` ，那么就只有包含在数组中的属性将会被转化为字符串。在本例中，只有名为"level" 和 "health" 的属性被包括进来， "username"则被排除在外。 data 就等于 "{"level":19, "health":90}".

而如果替代者(replacer)是个 `函数`，这个函数将被对象的每个属性都调用一遍。 函数返回的值会成为这个属性的值，最终体现在转化后的JSON字符串中（译者注：Chrome下，经过实验，如果所有属性均返回同一个值的时候有异常，会直接将返回值作为结果输出而不会输出JSON字符串），而如果返回值为undefined，则该属性会被排除在外。


### 15. padStartf方法
使用padStart方法，我们可以在字符串的开头添加填充。传递给此方法的参数是字符串的总长度（包含填充）。
如果传递给padStart方法的参数小于字符串的长度，则不会添加填充。

```javascript
const name = "Lydia Hallie"
console.log(name.padStart(13))	// " Lydia Hallie"
console.log(name.padStart(2))	// "Lydia Hallie"
```

字符串Lydia Hallie的长度为12, 因此name.padStart（13）在字符串的开头只会插入1（13 - 12 = 1）个空格。


### 16. 函数的prototype
常规函数，例如giveLydiaPizza函数，有一个prototype属性，它是一个带有constructor属性的对象（原型对象）。 然而，箭头函数，例如giveLydiaChocolate函数，没有这个prototype属性。 尝试使用giveLydiaChocolate.prototype访问prototype属性时会返回undefined。

```javascript
function giveLydiaPizza() {
  return "Here is pizza!"
}

const giveLydiaChocolate = () => "Here's chocolate... now go hit the gym already."

console.log(giveLydiaPizza.prototype)	// {constructor: ...}
console.log(giveLydiaChocolate.prototype)	// undefined
```

### 17. Object.entries
Object.entries()方法返回一个给定对象自身可枚举属性的键值对数组，下述情况返回一个二维数组，数组每个元素是一个包含键和值的数组：
`[['name'，'Lydia']，['age'，21]]`

```javascript
const person = {
  name: "Lydia",
  age: 21
}

for (const [x, y] of Object.entries(person)) {
  console.log(x, y)	// name Lydia and age 21
}
```

使用for-of循环，我们可以迭代数组中的每个元素，上述情况是子数组。 我们可以使用const [x，y]在for-of循环中解构子数组。 x等于子数组中的第一个元素，y等于子数组中的第二个元素。

第一个子阵列是[“name”，“Lydia”]，其中x等于name，而y等于Lydia。 第二个子阵列是[“age”，21]，其中x等于age，而y等于21。


### 18. 箭头函数返回值
对于箭头函数，如果只返回一个值，我们不必编写花括号。但是，如果您想从一个箭头函数返回一个对象，您必须在圆括号之间编写它，否则不会返回任何值：

```javascript
const getList = ([x, ...y]) => [x, y]
const getUser = user => { name: user.name, age: user.age }

const list = [1, 2, 3, 4]
const user = { name: "Lydia", age: 21 }

console.log(getList(list))	// [1, [2, 3, 4]]
console.log(getUser(user))	// undefined

const getUser = user => ({ name: user.name, age: user.age })	// { name: "Lydia", age: 21 }
```