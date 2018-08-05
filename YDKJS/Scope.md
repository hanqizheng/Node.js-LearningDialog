# 作用域篇(scope)
2018-08-07

今天，咱们就来说说作用域。凡事要知其然并知其所以然。

## 知其然，问作用域为何物？

>几乎所有编程语言最基本的功能之一,就是能够储存变量当中的值,并且能在之后对这个
值进行访问或修改。事实上,正是这种储存和访问变量的值的能力将**状态**带给了程序。

因为有了变量，程序和我们仿佛就有了某种联系，我们可以通过声明变量，然后在接下来的程序中去拿来使用。通过这种方法编织我们的程序，让一切都变得easier。

但是，这些变量都放在哪里呢？我们在想要使用的时候，怎么去找到他们呢？

>这些问题说明需要一套设计良好的规则来存储变量,并且之后可以方便地找到这些变量。
这套规则被称为**作用域**。


## var a = 2; 可不简单
当我们看到`var a = 2;`这句话，肯定第一印象就是这是一句声明。声明一个变量a并且赋值为2。

但如果从JS引擎的角度看，这句话可没这么简单，他包含两个完全不同的声明。

>可以合理地假设编译器所产生的代码能够用下面的伪代码进行概括:“为一个变量分配内
存,将其命名为 a ,然后将值 2 保存进这个变量。”然而,这并不完全正确。

1. 遇到 var a ,编译器会询问作用域是否已经有一个该名称的变量存在于同一个作用域的
集合中。如果是,编译器会忽略该声明,继续进行编译;否则它会要求作用域在当前作
用域的集合中声明一个新的变量,并命名为 a 。

2. 接下来编译器会为引擎生成运行时所需的代码,这些代码被用来处理 a = 2 这个赋值
操作。引擎运行时会首先询问作用域,在当前的作用域集合中是否存在一个叫作 a 的
变量。如果是,引擎就会使用这个变量;如果否,引擎会继续查找该变量


## LHS和RSH
其实，引擎的查找方法是有两种的。LHS(Left-Hand Side),RHS(Right-Hand Side)。

如果这句话是一句赋值功能的语句，则会执行LHS

如果这句话是一句取值功能的语句，则会执行RHS

```js
function foo(a) {
  var b = a;
  return a + b;
}

var c = foo(2);
```
上面这段代码有哪些LHS 和 RHS呢？

`var c = foo(2);`中对foo进行RHS

`var c = foo(2);`中对c进行LHS

`function foo(a)`中对a(形参)进行赋值LHS

`var b = a;`函数体要使用a所以进行RHS

`var b = a;`中对b进行LHS 把a赋值给b

`return a + b;`对a进行RHS

`return a + b;`对b进行RHS

看了这个例子，小伙伴们应该差不多理解了

## 作用域的嵌套
在实际编程的过程中肯定会有函数嵌套函数这类情况，而我又知道函数拥有自己的作用域，所以就会形成作用域嵌套。

作用域嵌套会引起一些需要注意的地方

- 每个作用域可以命名相同名称的变量，之间不会干扰每个作用与使用的是自己内部的那个变量。

- 在某个作用域使用到的变量却没有在该作用域声明的话，他会从自己开始一层一层的向外扩展查找，直到找到第一个匹配的标识符，然后停止查找，如果在往外还有，但已经不会造成任何影响了，因为已经停止查找了。

- 全局的变量会自动成为全局对象的一个属性

## eval 和 with
这两个稍微提一下，不是很推荐使用的东西。

eval的参数接受一个字符串，效果就是在程序生成代码的时候，引擎会将eval的参数当作代码执行。这样就可以打破作用域的原始限制。

with自身简单理解为创建了一个块作用域。很不推荐使用

### eval 和 with 会导致性能低下

因为eval和with是在运行是将参数当作代码来运行，所以在词法分析阶段，引擎无法知道eval和with到底想要干什么，而引擎有一项工作是优化代码，对于eval和with，优化他们的最好办法就是什么都不做。

所以如果代码中有大量的eval或者with，就会导致代码的性能极其差。

## let & const
let 和 const 请阅读[let&const](https://github.com/hanqizheng/Node.js-LearningDialog/blob/master/Js%E5%9F%BA%E7%A1%80%E5%AD%A6%E4%B9%A0/let%26const.md)
