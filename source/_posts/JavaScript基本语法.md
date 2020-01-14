---
title: JavaScriptの基本语法
date: 2019-06-05 09:15:00
categories: JavaScript
tags:
  - javascript
---
# JavaScriptの基本语法

## 基本语法

### 语句

> **JavaScript我理解为行语句，一行一行执行。很像Java都需要以";"结尾。**

### 变量

```javascript
var a = 1;
```

**很好理解，var是声明变量；a则是变量名；1则是将值赋值给变量a。引用变量a就会得到数值1.**

**设置变量并不一定需要var来声明。当你没有使用var时，其实你已经创建了一个全局变量。**

**注：当已经存在的变量时再次被声明时，后面的声明是无效的。**

### 变量提升

> #### JavaScript引擎的工作方式是，先解析代码。获取所以被声明的变量，在逐行执行，这意味着所有的变量将到程序的行首。这叫做变量提升。

### 标识符

> #### 标识符(identifier)指的是识别各种值得合法名称。

- **第一个字符，可以是任意的Unicode字母，以及($),(_)**
- **第二个字符及后面的字符，除了Unicode字母，美元符号，下划线还可以是数字。**

### 注释

> #### 被JavaScript引擎忽略的部分就叫注释，主要最有用来进行代码的解释。同时也可以将我们不想要执行的代码注释不执行。

```javascript
// 这是单行注释
/* 这是多行注释 */
```

### 区块

> #### JavaScript使用大括号，将多个相关的语句组合在一起，称为“区块”(block)

### 条件语句

#### if语句

```javascript
if (条件语句，获得bool值) {
   	为true时执行代码块
}eles {
    为false时执行代码块
}
```

```javascript
if (条件语句，获得bool值){
    为true执行
}else if(上面为false时，判断条件语句){
    为true执行，为false继续往下执行
}else if(继续判断条件语句){
    为true执行
}else{
	上述都不满足条件时执行。
}
```

#### switch结构

> ##### 当我们多个if...else...时，也许使用switch更方便

```javascript
switch (xxx) {
    case "xxx":
        xxxxxxx
        break;
    case "xxxx":
        xxxxxxx
        break;
    default:
        xxxxxx
}
```

**记得在每个case下面接break，不然执行完会继续跳到下个case。需使用break进行跳出switch。**

**switch语句部分和case语句部分都可以使用正则表达式。**

#### 三元运算符？：

```javascript
(条件) ? 表达式1 ： 表达式
```

**示例：**

```javascript
var even = (n % 2 === 0) ? true : false
```

**下面是if..else..示例**

```javascript
var even;
if (n % 2 === 0) {
    even = true;
}else {
    even = false;
}
```

###  循环语句

#### while循环

```javascript
while (条件) {
    代码块
}
```

**示例：**

```javascript
var i = 0;
while (i<100) {
    console.log("当前i的值为："+ i);
    i += 1;
}
```

#### for循环

```javascript
for (初始化表达式;条件;递增表达式) {
    代码块
}
```

- **初始化表达式：确定循环变量的初始值，只在循环开始时执行一次。**
- **条件表达式：每轮循环开始时，都要执行这个条件表达式，只有值为真时才会继续进行循环。**
- **递增表达式：每轮循环的最后一个操作，通常用来递增循环变量。**

**示例：**

```javascript
var x = 5;
for (var i = 0; i < x ; i ++) {
    console.log(”当前i的值为：“+i);
}
```

#### do...while循环

```javascript
do{
    代码块
}while (条件);
```

**do...while和while唯一的区别在于，do...while必须执行一次。示例：**

```javascript
var x = 3;
var i = 0;
do{
    console.log(i);
    i++;
}while(i<x);
```

```bash
0
1
2
```

#### break语句和continue语句

> **break和continue都具有跳转的作用。**

**break语句用于跳转代码块或者循环。**

```javascript
var i = 0;
while(i < 20) {
    console.log("循环的次数："+i);
    i += 1;
    if (i===10) break;
}
```

```javascript
var j = 100;
for (var i=0;i<j;i++) {
    console.log("当前的i值为："+i);
    if (i === 10) break;
}
```

**continue是立即停止本轮循环，跳转到循环的头部，执行下一次循环。**

```javascript
var i = 0;

while (i < 100){
  i++;
  if (i % 2 === 0) continue;
  console.log('i 当前为：' + i);
}
```

**上面的代码只有i的值为奇数时才会输入，为偶数的时候执行下一轮循环。**

#### 标签(label)

```
label:
	代码块
```

**标签可以是任意的标识符，但不能是保留字，语句部分可以是任意语句。**

**标签通常与break和continue语句配合使用，跳出特定循环。**

```javascript
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) break top;
      console.log('i=' + i + ', j=' + j);
    }
  }

// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
```

```javascript
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) continue top;
      console.log('i=' + i + ', j=' + j);
    }
  }

// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
// i=2, j=0
// i=2, j=1
// i=2, j=2
```