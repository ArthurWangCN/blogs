---
title: 解耦HTML、CSS和JavaScript 
date: 2017-02-20 17:47:09
tags: 前端
categories: 前端
---
当前在互联网上，任何一个稍微复杂的网站或者应用程序都会包含许多 `HTML`、`CSS` 和 `JavaScript`。随着互联网运用的发展以及我们对它的依赖性日益增加，设定一个关于组织和维护你的前端代码的计划是绝对需要的。

当今的一些大型互联网公司，由于越来越多的人会接触到日益增加的前端代码，它们会试图去坚持代码的模块化。这样更改程序的部分代码，并不会无意中过多地影响后续不相关部分的执行过程。

防止意想不到的后果不是一个容易解决的问题，尤其是HTML，CSS和JavaScript本质上是 `相互依赖` 的。更糟糕的是，当涉及到前端代码时，一些传统计算机科学原则，比如关注分离，这一长期运用在服务端开发中，很少会讨论到。

在本文中，我将会讲讲我所学到的如何去 `解耦` 我的HTML，CSS和JavaScript代码。从个人以及他人经验所得，这种的最好办法并不是那么显而易见，而通常是不直观的，而且有时还会与许多所谓的最佳实践相违背。

## 目标
HTML，CSS和JavaScript之间总会存在耦合关联。不管怎样，这些技术与生俱来就是要和其它进行交互。举个例子，一种飞闪转换效果可能会在样式表中用带有类选择器定义，但它经常由HTML初始化，并通过用户交互，如编写JavaScript，来触发。由于前端代码的有些耦合是不可避免的，你的目标就不应该是简单地消除之间的耦合，而应该是减少代码间不必要的依赖耦合关系。一个后端开发者应该能够对HTML模板中的标记进行更改，而无需担心意外破坏CSS规则或者一些JavaScript功能。由于当今的web团队日渐增大且专业化，这个目标比以往更甚。

## 反模式
前端代码的紧耦合现象并不总是很明显。事实上复杂的是，一方面看起来似乎松耦合，但从另一方面则是紧耦合。以下是我曾经多次做过或者看过，以及吸取我的过错中，总结的所有的反模式。对每一个模式，我会尝试去解释为何耦合这么糟糕，并且指出如何去避免它。

### 过度复杂的选择器
`CSS Zen Garden` 向世界展示了你可以完全改变整个网站的外观而无需更改任意一个的HTML标记。这是语义网运动的典型代表，主要原则之一就是去避免使用表象类。乍一看，CSS Zen Garden 可能看起来像是一个很好的解耦合例子，毕竟，把样式从标记语言中分离出来是它的重点所在。但是，若按照这样做，问题就来了，你会经常需要在你的样式表里有这样的选择器，如下：

`#sidebar section:first-child h3+p {}`

CSS Zen Garden 中，虽然HTML几乎与CSS完全分离，但CSS会强耦合到HTML中去，此时就需要你对标记语言的结构有深层次的理解。这可能看起来似乎并不是很糟糕，尤其是某人维护着CSS，同时需要维护HTML，但一旦你增加了许多人手进去，这种情况就变得无法控制了。如果某个开发者在某种情况下在第一个前增加了，上面的规则就无法生效，然而他也不清楚其中缘由。

只要你网站的标记很少改动，CSS Zen Garden就是一个非常不错的主意。但是这和当今的Web应用不尽然都是这种情况。与冗长而又复杂的CSS选择器相比，最好的办法是在可视化组件本身的根元素增加一个或多个类选择器。比如，如果侧边栏有子菜单，只需要为每个子菜单元素增加submenu类选择器，而不要用这样的形式：

`ul.sidebar>li>ul {}`

这种方式的结果是在HTML中需要更多的类选择器，但从长远来看，这又降低了耦合度，以及让代码更可重用和可维护，并且还能让你的标记自文档化。如果HTML里没有类选择器，那些对CSS不熟悉的开发者就不清楚HTML的改动如何影响了其它代码。另一方面，在HTML中使用类选择器能很清晰地看到那些样式或者功能被使用到了。

## 多个类选择器的职责
一个类选择器往往是用来同时作为样式和，但事实上，这是把元素的表现和功能耦合起来了。

`<button class="add-item"></button>`

以上例子描述了一个带有add-item类样式的”添加到购物车”按钮。

如果开发者想为此元素添加一个单击事件监听器，用已经存在的类选择器作为钩子非常的容易。我的意思是，既然已经存在了一个，为何要添加另一个呢？但是想想看，有很多像这样的按钮，遍布了整个网站，都调用了相同的JavaScript功能。再想想看，如果市场团队想要其中一个和其它看起来完全不同但功能相同的按钮呢。也许这样就需要更多显著的色彩了。

问题就来了，因为监听单击事件的。还有，如果你测试的代码同时也希望使用add-item类选择器，那么你不得不要去更新那么代码用到的地方。更糟糕的是，如果这个”添加到购物车”功能不仅仅是当前应用用到的话，也就是说，把这份代码抽象出来作为一个独立的模块，那么即使一个简单的样式修改，可能会在完全不同的应用中引发问题。

使用做法是，如果你需要这么做，使用一种方式来避免样式和行为类选择器之间的耦合。

我的个人建议是让JavaScript钩子使用前缀，比如：js-*。这样的话，当开发者在HTML源代码中看到这样的类选择器，他就完全明白个中原因了。所以，上述的”添加到购物车”的例子可以重写成这样：

`<button  class="js-add-to-cart add-item"></button>`

现在，如果需要一个看起来不同的按钮，你可以很简单地修改下样式类选择器，而不管行为的类选择器：

`<button class="js-add-to-cart add-item-special"></button>`

## JavaScript更多的样式操作
JavaScript能用类选择器去DOM中查找元素，同样，它也能通过增加或移除类选择器来改变元素的样式。但如果这些类选择器和当初加载页面时不同的话也会有问题。当JavaScript代码使用太多的组成样式操作时，那些CSS开发者就会轻易去改变样式表，却不知道破坏了关键功能。也并不是说，JavaScript不应该在用户交互之后改变可视化组件的外观，而是如果这么做，就应该使用一种一致的接口，应该使用和默认样式不一致的类选择器。

和 `js-*` 前缀的类选择器类似，我推荐使用 `is-*` 前缀的类选择器来定义那些要改变可视化组件的状态，这样的CSS规则可以像这样：

`.pop-up.is-visible {}`

注意到状态类选择器(is-visible)是连接在组件类选择器(pop-up)后，这很重要。因为状态规则是描述一个的状态，不应该单独列出。如此不同就可以用来区分更多和默认组件样式不同的状态样式。

另外，可以让我们可以编写测试场景来保证像 `is-*` 这样的前缀约定是否遵从。一种测试这些规则的方式是使用 `CSSLint` 和 `HTML Inspector`。

更多关于特定状态类选择可以查阅Jonathan Snnok编写的非常优秀的SMACSS书籍。

##JavaScript 选择器
jQuery和新的API，像 `document.querySelectorAll`，让用户非常简单地通过一种他们已经非常熟悉的语言–CSS选择器来查找DOM中的元素。虽然如此强大，但同样有CSS选择器已经存在的相同的问题。JavaScript选择器不应过度依赖于DOM结构。这样的选择器非常慢，并且需要更深入认识HTML知识。

就第一个例子来讲，负责HTML模板的开发者应该能在标记上做基本的改动，而不需担心破坏基本的功能。如果有个功能会被破坏，那么它就应该在标记上显而易见。

我已经提及到应该用 `js-*` 前缀的类选择器来表示JavaScript钩子。另外针对消除样式和功能类选择器之间的二义性，需要在标记中表达出来。当某人编写HTML看到 `js-*` 前缀的类选择器时，他就会明白这是别有用途的。但如果JavaScript代码使用特定的标记结构查找元素时，正在触发的功能在标记上就不那么明显了。

为了避免使用冗长而又复杂的选择器遍历DOM，坚持使用单一的类或者ID选择器。 考虑以下代码：

`var saveBtn = document.querySelector("#modal div:last-child>button:last-child")`

这么长的选择器是可以节省你在HTML中添加一个类选择器，但同样让你的代码对于标记更改非常容易受到影响。如果设计者突然决定要把保持按钮放在左边，而让取消按钮放在右边，这样的选择器就不再匹配了。

一个更好的方式(使用上述的前缀方法)是仅仅使用类选择器。

`var saveBtn = document.querySelector(".js-save-btn")`

现在标记可以更改它想改的，并且只要类选择还是在正确的元素上，一切都会很正常。

### 类选择器就是你的契约
使用合适的类选择器以及可预测的类名约定可以减少几乎每一种HTML，CSS和JavaScript之间的耦合。起初由于为了展现HTML需要知道很多类选择器的名称，这种在标记中使用很多类选择器看起来像是强耦合的迹象。但是我发觉，使用类选择器和传统编程设计中的事件或者观察者模式非常相似。在事件驱动编程中，为了不直接在对象A上调用对象B，而是对象A简单地在提供的环境中发布一个特定的事件，然后对象B能够订阅那个事件。这样，对象B就不需要知道任何关于对象A的接口，而仅仅需要知道监听什么事件。按理说，事件系统需要某种形式上的耦合，因为对象B需要知道订阅的事件名称，但和对象A需要知道对象B的公共方法相比，这已经更松散的耦合了。

HTML类选择器都非常相似。与CSS文件中定义复杂的选择器(就像HTML的内部接口一样)不同的是，它可以通过单一类选择器简单定义一个可视化组件的外观。CSS文件不需要关心HTML对类选择器的使用与否。同样，JavaScript不用那些需要更深入理解HTML结构的复杂DOM遍历功能，而是仅仅监听与类名一致的元素的用户交互。类选择器应该像是胶水一样，把HTML，CSS和JavaScript连接在一起。从个人经验得知，它们也是最容易以及最好的方式把三者技术连接起来，而不是混合过度。

## 未来
网页超文本技术工作小组(WHATWG)正在致力于web组件的规范，能让开发者把HTML，CSS和；但是无论如何，理解这些更广泛的原则以及为何需要它们仍然很重要。即使这些实践在Web组件时代会变得不那么重要，但其中的理论仍然适用。在大型团队和大型应用中的实践仍然要适用于小模块的编写中，反之则不需要。

## 结论
可维护的HTML，CSS和JavaScript的标志是每个开发者可以容易并且很自信地编写代码库的每个部分，而不需担心这些修改会无意中影响到其它不相关部分。阻止这样意想不到的后果的最佳方式之一是，通过一组能够表达其义的，任何开发者碰到时能想出它的用途的，可预测的人性化的类选择器名，把这三者技术结合在一起。

为避免上述的反模式，请把下述的原则谨记于心：
1. 在CSS和JavaScript里，优先考虑显式组件和行为类选择器，而不是复杂的CSS选择器。
2. 命名组件要基于 `它们是什么`，而不是它们在哪里
3. 不用为样式和行为使用相同的类选择器
4. 把状态样式和默认样式区分开来

在HTML中这样运用类选择器经常会需要很多需要表现的类选择器，但获取的是可预见性和可维护性，这点值得肯定。毕竟，为HTML增加类选择器是相当容易的，不需要开发者有多少技能。

摘自Nicolas Gallagher的原话：
>当你要寻找一种方式来减少花费在编写和修改CSS的时间上来制作HTML和CSS时，这就涉及到你必须接受如果你想更改样式，你是不想花费更多时间去更改HTML元素上的类选择器。这对前端和后端开发者都有一定的实用性，任何人都可以重新安排预构建的乐高积木。这样没有人会去展示CSS的魔力了。