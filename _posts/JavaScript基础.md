---
layout: post
title: JS 基础
subtitle: JavaScript
author: oliver
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - JavaScript
---
[TOC]
# JavaScript中的类型应该包括这些：
- Number（数字）
- String（字符串）
- Boolean（布尔）
- Symbol（符号）（ES2015 新增）
- Object（对象）
    - Function（函数）
    - Array（数组）
    - Date（日期）
    - RegExp（正则表达式）
- null（空）
- undefined（未定义）

JavaScript 还有一种内置的 **Error（错误）类型**。但是，如果我们继续使用上面的分类，事情便容易得多

# Number
JavaScript **不区分整数值和浮点数值**，所有数字在 JavaScript 中均用**浮点数值**表示，所以在进行数字运算的时候要特别注意。

**要小心NaN**：如果把 NaN 作为参数进行任何数学运算，结果也会是 NaN：
```javascript
NaN + 5; //NaN
```
可以使用内置函数 isNaN() 来判断一个变量是否为 NaN：
```javascript
isNaN(NaN); // true
```

# 字符串
通过访问字符串的 length（编码单元的个数）属性，可以得到它的长度。
```javascript
"hello".length; // 5
```
字符串也有 methods（方法）能让你操作字符串和获取字符串的信息。
```javascript
"hello".charAt(0); // "h"
"hello, world".replace("world", "mars"); // "hello, mars"
"hello".toUpperCase(); // "HELLO"
```
# 其他类型
与其他类型不同，JavaScript 中的 null 表示一个**空值**（non-value），必须使用 null 关键字才能访问，undefined 是一个“undefined（未定义）”类型的对象，表示一个**未初始化的值**，也就是**还没有被分配的值**。

JavaScript 允许声明变量但不对其赋值，一个未被赋值的变量就是 undefined 类型。还有一点需要说明的是，undefined 实际上是一个不允许修改的常量。

JavaScript 包含布尔类型，这个类型的变量有两个可能的值，分别是 true 和 false（两者都是关键字）。根据具体需要，JavaScript 按照如下规则将变量转换成布尔类型：

- **false、0、空字符串（""）、NaN、null 和 undefined 被转换为 false**
- **所有其他值被转换为 true**

也可以使用 Boolean() 函数进行显式转换：
```javascript
Boolean(""); // false
Boolean(234); // true
```
# 变量
在 JavaScript 中声明一个新变量的方法是使用关键字 let 、const 和 var：

**let** 语句声明一个块级作用域的本地变量
```
// myLetVariable is *not* visible out here

for (let myLetVariable = 0; myLetVariable < 5; myLetVariable++) {
  // myLetVariable is only visible in here
}

// myLetVariable is *not* visible out here
```
**const** 允许声明一个不可变的常量。这个常量在定义域内总是可见的。
```javascript
const Pi = 3.14; // 设置 Pi 的值  
Pi = 1; // 将会抛出一个错误因为你改变了一个常量的值。
```
var 是最常见的声明变量的关键字。它没有其他两个关键字的种种限制。这是因为它是传统上在 JavaScript 声明变量的唯一方法。**使用 var 声明的变量在它所声明的整个函数都是可见的**。
```
// myVarVariable *is* visible out here 

for (var myVarVariable = 0; myVarVariable < 5; myVarVariable++) { 
  // myVarVariable is visible to the whole function 
} 

// myVarVariable *is* visible out here
```

JavaScript 与其他语言的（如 Java）的重要区别是在 JavaScript 中语句块（blocks）是**没有作用域**的，**只有函数有作用域**。

# 运算符
JavaScript 中的比较操作使用 <、>、<= 和 >=，这些运算符对于数字和字符串都通用。相等的比较稍微复杂一些。**由两个“=（等号）”组成的相等运算符有==类型自适应==的功能**，具体例子如下：
```
123 == "123" // true
1 == true; // true
```
如果在比较前**不需要自动类型转换**，应该使用由三个“=（等号）”组成的相等运算符：
```
1 === true; //false
123 === "123"; // false
```

# 控制结构
for...of 和 for...in
```
for (let value of array) {
  // do something with value
}

for (let property in object) {
  // do something with object property
}
```

&& 和 || 运算符使用**短路逻辑（short-circuit logic**），是否会执行第二个语句（操作数）取决于第一个操作数的结果。在需要访问某个对象的属性时，使用这个特性可以事先检测该对象是否为空：
```
var name = o && o.getName();
```

# 对象
JavaScript 中的对象，Object，可以简单理解成“名称-值”对（而不是键值对：现在，ES 2015 的映射表（Map），比对象更接近键值对）

有两种简单方法可以创建一个空对象：
```
var obj = new Object();
var obj = {}; //一般我们优先选择此种方法
```
“对象字面量”也可以用来在对象实例中定义一个对象：
```
var obj = {
    name: "Carrot",
    "for": "Max",//'for' 是保留字之一，使用'_for'代替
    details: {
        color: "orange",
        size: 12
    }
}
```
对象的属性可以通过链式（chain）表示方法进行**访问**：
```
obj.details.color; // orange
obj["details"]["size"]; // 12
```

# 数组

创建数组的传统方法是：
```
var a = new Array();
a[0] = "dog";
a[1] = "cat";
a[2] = "hen";
a.length; // 3
```
使用数组字面量法更方便
```
var a = ["dog", "cat", "hen"];
a.length; // 3
```

如果试图访问一个**不存在**的数组索引，会得到 undefined：
```
typeof(a[90]); // undefined
```
如果想在数组后追加元素，只需要：
```
a.push(item);
```
除了 forEach() 和 push()，Array（数组）类还自带了许多方法。建议查看[ Array 方法的完整文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)。



方法名称 | 描述
---|---
a.toString() | 返回一个包含数组中所有元素的字符串，每个元素通过逗号分隔。
a.concat(item1[, item2[, ...[, itemN]]]) | 返回一个数组，这个数组包含原先 a 和 item1、item2、……、itemN 中的所有元素。
a.join(sep)	| 返回一个包含数组中所有元素的字符串，每个元素通过指定的 sep 分隔。
a.pop() |	删除并返回数组中的最后一个元素。
a.push(item1, ..., itemN) |	将 item1、item2、……、itemN 追加至数组 a。
a.reverse() |	数组逆序（会更改原数组 a）。
a.shift() |	删除并返回数组中第一个元素。
a.slice(start, end) |	返回子数组，以 a[start] 开头，以 a[end] 前一个元素结尾。
a.sort([cmpfn])	|依据可选的比较函数 cmpfn 进行排序，如果未指定比较函数，则按字符顺序比较（即使被比较元素是数字）。
a.splice(start, delcount[, item1[, ...[, itemN]]])	|从 start 开始，删除 delcount 个元素，然后插入所有的 item。
a.unshift(item1[, item2[, ...[, itemN]]])| 将 item 插入数组头部，返回数组新长度（考虑 undefined）。

# 函数


这个剩余参数操作符在函数中以：...variable 的形式被使用，它将包含在调用函数时使用的未捕获整个参数列表到这个变量中。我们同样也可以将 for 循环替换为 for...of 循环来返回我们变量的值。
```
function avg(...args) {
  var sum = 0;
  for (let value of args) {
    sum += value;
  }
  return sum / args.length;
}

avg(2, 3, 4, 5); // 3.5
```

下面这个例子隐藏了局部变量：
```
var a = 1;
var b = 2;
(function() {
    var b = 3;
    a += b;
})();

a; // 4
b; // 2
```

# 自定义对象
JavaScript 是一种基于原型的编程语言，并没有 class 语句，而是把函数用作类。

Person.**prototype** 是一个可以被Person的所有实例共享的对象。它是一个名叫**原型链（prototype chain）**的查询链的一部分：当你试图访问一个 Person 没有定义的属性时，解释器会首先检查这个 Person.prototype 来判断是否存在这样一个属性。所以，任何分配给 Person.prototype 的东西对通过 this 对象构造的实例都是可用的。

这个特性功能十分强大，JavaScript 允许你在程序中的任何时候**修改原型（prototype）中的一些东西**，也就是说你可以在运行时(runtime)给已存在的对象添加额外的方法：
```
s = new Person("Simon", "Willison");
s.firstNameCaps();  // TypeError on line 1: s.firstNameCaps is not a function

Person.prototype.firstNameCaps = function() {
    return this.first.toUpperCase()
}
s.firstNameCaps(); // SIMON
```
有趣的是，你还可以给 JavaScript 的内置函数原型（prototype）添加东西。让我们给 String 添加一个方法用来返回逆序的字符串：
```
var s = "Simon";
s.reversed(); // TypeError on line 1: s.reversed is not a function

String.prototype.reversed = function() {
    var r = "";
    for (var i = this.length - 1; i >= 0; i--) {
        r += this[i];
    }
    return r;
}
s.reversed(); // nomiS
```
定义新方法也可以在字符串字面量上用（string literal）。
```
"This can now be reversed".reversed(); // desrever eb won nac sihT
```
正如我前面提到的，原型组成链的一部分。那条链的根节点是 Object.**prototype**.
## 内部函数
JavaScript 允许在一个函数内部定义函数，这一点我们在之前的 makePerson() 例子中也见过。关于 JavaScript 中的嵌套函数，**一个很重要的细节是，它们可以访问父函数作用域中的变量：**
```
function parentFunc() {
  var a = 1;

  function nestedFunc() {
    var b = 4; // parentFunc 无法访问 b
    return a + b;
  }
  return nestedFunc(); // 5
}
```
这也是一个减少使用全局变量的好方法。当编写复杂代码时，程序员往往试图使用全局变量，将值共享给多个函数，但这样做会使代码很难维护。内部函数可以共享父函数的变量，所以你可以使用这个特性把一些函数捆绑在一起，这样可以有效地防止“污染”你的全局命名空间——你可以称它为“局部全局（local global）”。虽然这种方法应该谨慎使用，但它确实很有用，应该掌握。

# 闭包
闭包是 JavaScript 中最强大的抽象概念之一——但它也是最容易造成困惑的。它究竟是做什么的呢？