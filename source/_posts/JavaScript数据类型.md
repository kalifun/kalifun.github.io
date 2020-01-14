---
title: JavaScriptの数据类型
date: 2019-06-06 09:15:00
categories: JavaScript
tags:
  - javascript
---

# JavaScriptの数据类型

## 简介

- **数值(number)：整数和小数**
- **字符串(string):文本**
- **布尔值(boolean)：真假**
- **undefined：表示未定义或者不存在**
- **null：表示空值**
- **对象(object)：各种值组成的集合**

**利用typeof可以返回数据类型**

## null和undefined

### 概述

> #### null与undefined都可以表示“没有”，含义非常相似。

```javascript
var a = undefined;
//或
var a = null;
```

**区别：null是表示一个空对象，转为数值时为0；undefined是表示此处未定义的原始值，转为数值时为NaN。**

## 布尔值

> 布尔值代表真和假两个状态。

下列运算符会返回布尔值：

- **前置逻辑运算符：!**
- **相等运算符：===，！==，==，！=**
- **比较运算符：>,>=,<,<=**

## 字符串

> #### 字符串就是零个或者多个字符组成的，放在单引号或者双引号之中。

#### 转义

> ##### 反斜杠(\)在字符串内有特殊含义，用来表示一些特殊字符，所以又称为转义字符。

- **\0 : null(\u000)**
- **\b : 后退键(\u0008)**
- **\f : 换页符(\u000C)**
- **\n : 换行符(\u000A)**
- **\r : 回车键(\u000D)**
- **\t : 制表符(\u0009)**
- **\v : 垂直制表符(\u000B)**
- **\\'  : 单引号(\u0027)**
- **\\" : 双引号(\u0022)**
- **\\\ : 反斜杠(\u005C)**

#### 字符串与数组

> 字符串可以被视为字符数组，因此可以使用数组的方括号运算符，用来返回某个位置的字符

```javascript
> var s = 'hello';
> s[0]
'h'
> s[1]
'e'
> s[2]
'l'
> s[3]
'l'
> s[4]
'o'
> s[5]
undefined
```

```javascript
var s = 'hello';

delete s[0];
s // "hello"

s[1] = 'a';
s // "hello"

s[5] = '!';
s // "hello"
```

**上述代码表示,字符串内部的的那个字符无法改变和增删，这些操作会默默地失败。**

#### length属性

```JavaScript
var s = 'hello';
s.length // 5

s.length = 3;
s.length // 5

s.length = 7;
s.length // 5
```

**length属性返回字符串的长度，该属性是不可以修改的。**

#### 字符集

**JavaScript使用Unicode字符集**

```javascript
var s = '\u00A9';
s // "©"
```

#### Base64转码

**有时候文本里面包含一些不可打印的符号，比如ASCII码0到31的符号都打印不出来，这时候可以使用Base64编码。另一个场景是，有时候需要以文本格式传递二进制数据，那么也可以使用Base64编码。**

- **btoa():任意值转为Base64编码。**
- **atob()：Base编码转为原来的值。**

```javascript
var string = 'Hello World!';
btoa(string) // "SGVsbG8gV29ybGQh"
atob('SGVsbG8gV29ybGQh') // "Hello World!"
```

## 对象

### 概述

#### 生成方法

> ##### 对象(object)就是一组“键值对”(key-value)的集合，是一种无序的复合数据集合。

```javascript
var obj = {
  foo: 'Hello',
  bar: 'World'
};
```

#### 键名

> 对象的所有键名都是字符串，所有加不加引号都可以。

#### 对象的引用

```javasc
var v1 = {};
var v2 = v1;
v1.a = 1;
v2.a
// 1
```

### 属性的操作

#### 属性的读取

**读取对象的属性，有两种方法，一种的使用点运算符，还有一种是使用方括号运算符。**

```javascript
var obj = {
  p: 'Hello World'
};

obj.p // "Hello World"
obj['p'] // "Hello World"
```

#### 属性赋值

点运算符和方括号运算符，不仅可以用来读取值，还可以用来赋值。

```javascript
var obj = {};

obj.foo = 'Hello';
obj['bar'] = 'World';
```

#### 属性的查看

**查看一个对象本身的所有属性，可以使用Object.keys方法。**

```javascript
var obj = {
  key1: 1,
  key2: 2
};

Object.keys(obj);
// ['key1', 'key2']
```

#### 属性的删除：delete命令

**delete命令用于删除对象的属性，删除成功后返回true。**

```javascript
> var obj = {p: 1};
undefined
> Object.keys(obj)
[ 'p' ]
> delete obj.p
true
> obj.p
undefined
> Object.keys(obj)
[]
```

#### 属性是否存在：in运算符

**in运算符用于检查对象是否包含某个属性（检查的是键名，不是键值），如果包含则返回true。**

```javascript
> var obj = {p: 1};
undefined
> 'p' in obj
true
> 'toString' in obj
true

```

**你会发现对象并无toString属性，但是返回的却是true。说明这个属性是继承的。**

**这是我们可以使用hasOwnProperty方法判断，是否对象是否是自身属性。**

```javascript
> if ('toString' in obj) { console.log(obj.hasOwnProperty('toString'))}
false
```

#### 属性的遍历：for…in循环

```javascript
var obj = {a: 1, b: 2, c: 3};

for (var i in obj) {
  console.log('键名：', i);
  console.log('键值：', obj[i]);
}
// 键名： a
// 键值： 1
// 键名： b
// 键值： 2
// 键名： c
// 键值： 3
```

- **它遍历的是对象所有可遍历的属性，会跳过不可遍历的属性。**
- **它不仅遍历对象自身的属性，还好遍历继承的属性。**

#### with语句

```javascript
with(对象) {
    代码块
}
```

## 函数

### 概述

#### 函数的声明

> ##### JavaScript有三种声明函数的方法。

**(1)function命名**

```javascript
function 函数名称(参数) {
    代码块
}
```

**(2)函数表达式(变量赋值)**

```javascript
var 变量名称 = function(参数) {
    代码块
};
执行时
变量名称()
```

**这种方法将一个匿名函数赋值给变量，这是，这个匿名函数又称之为函数表达式。**	

**(3)Function构造函数**

```javascript
var add = new Function(
	'x',
    'y',
    'return x+y'
);

等同于

function add(x,y) {
    return x+y;
}
```

#### 函数的重复声明

**如果同一个函数被多次声明，后面的声明就会覆盖前面的声明。**

#### 圆括号运算符，return语句和递归

**调用函数时，要使用圆括号运算符。圆括号之中，可以加入函数的参数。**

#### 第一等公民

> 由于函数与其他数据类型的地位平等，所以在JavaScript中称函数为第一公民。

```javascript
function add(x, y) {
  return x + y;
}

// 将函数赋值给一个变量
var operator = add;
```

#### 函数名的提升

**JavaScript引擎将函数名视同变量名，所以采用function命令声明函数时，整个函数将会像变量声明一样，被提升到代码头部。**

```javascript
test()
function test() {
    
}
不报错
```

**若采用其他的声明方式，下面则会报错。**

```javascript
test()
var test = function() {
    
}
报错
```

### 函数的属性和方法

#### name属性

**函数的name返回函数名**

```javascript
> function test(){}
undefined
> test().name
TypeError: Cannot read property 'name' of undefined
> test.name
'test'
```

#### length属性

**函数length属性返回函数预期传入的参数个数。**

```javascript
> function test(a,b,c) {}
undefined
> test.length
3
```

#### toString()

**函数的toString方法返回一个字符串，内容是函数的源码。**

```javascript
> test.toString()
'function test(a,b,c) {}'
```

### 函数作用域

#### 定义

> 作用域指的是变量存在的范围。JavaScript只有两种作用域：一种是全局作用域；另一种是函数作用域。

**对于顶层函数来说，函数外部声明的变量就是全局变量，它可以在函数内部读取。**

```javascript
var i =  1;
function print() {
    console.log(v);
}
print()
```

**在函数内部定义的变量，外部无法读取，称之为“局部变量”。**

```javascript
function print() {
    var v = 1;
}
v
```

#### 函数内部的变量提升

**与全局作用域一样，函数作用域内部也会产生“变量提升现象”。var 命令声明的变量，不管在上面位置，变量声明都会被提升到函数的头部。**

```javascript
function foo(x) {
  if (x > 100) {
    var tmp = x - 100;
  }
}

// 等同于
function foo(x) {
  var tmp;
  if (x > 100) {
    tmp = x - 100;
  };
}
```

#### 函数本身的作用域

**函数也是一个值，也有自己的作用域，它的作用域与变量一样，就是其声明时所在的作用域，与其运行时所在的作用域无关。**

### 参数

#### 概述

> 函数运行的时候，有时需要提供外部数据，不同的外部数据会得到不同的结果，这种外部数据就叫参数。

#### 参数的省略

> 函数的参数不是必须的

```javascript
> function test(a,b,c) {console.log(a)}
> test(3,2,1)
3
> test(3)
3
> test(3,3)
3
> test()
undefined
```

#### 传递方式

**函数内部修改参数值，不影响外部值。**

```javascript
var p = 2;

function f(p) {
  p = 3;
}
f(p);

p // 2
```

#### 同名参数

**如果有同名参数，则取最后出现的那个参数。**

```javascript
function f(a, a) {
  console.log(a);
}

f(1, 2) // 2
```

#### arguments对象

> 由于 JavaScript 允许函数有不定数目的参数，所以需要一种机制，可以在函数体内部读取所有参数。这就是`arguments`对象的由来。

```javascript
var f = function (one) {
  console.log(arguments[0]);
  console.log(arguments[1]);
  console.log(arguments[2]);
}

f(1, 2, 3)
// 1
// 2
// 3
```

**正常模式下，argument对象可以进行修改。**

```javascript
var f = function(a, b) {
  arguments[0] = 3;
  arguments[1] = 2;
  return a + b;
}

f(1, 1) // 5
```

##### 与数组的关系

**需要注意的是，虽然`arguments`很像数组，但它是一个对象。数组专有的方法（比如`slice`和`forEach`），不能在`arguments`对象上直接使用。**

```javascript
var args = Array.prototype.slice.call(arguments);

// 或者
var args = [];
for (var i = 0; i < arguments.length; i++) {
  args.push(arguments[i]);
}
```

##### callee属性

**返回它所对应的函数**

```javascript
var f = function () {
  console.log(arguments.callee === f);
}

f() // true
```

### 函数的其他知识点

#### 闭包

#### 立即调用函数表达式

### eval命令

#### 基本用法

**eval命令接受一个字符串作为参数，并将这个字符串当语句执行。**

```javascript
eval('var a = 1;');
a // 1
```

#### eval的别名调用

**JavaScript 的标准规定，凡是使用别名执行`eval`，`eval`内部一律是全局作用域。**

## 数组

### 定义

**数组（array）是按次序排列的一组值。每个值的位置都有编号（从0开始），整个数组用方括号表示。**

```javasc
var arr = ['a', 'b', 'c'];
```

### 数组的本质

**本质上，数组属于一种特殊的对象。`typeof`运算符会返回数组的类型是`object`。**

```javascript
typeof [1, 2, 3] // "object"

var arr = ['a', 'b', 'c'];

Object.keys(arr)
// ["0", "1", "2"]
```