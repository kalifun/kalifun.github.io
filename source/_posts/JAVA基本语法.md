---
title:  JAVA基本语法
date: 2019-01-29 16:51:00
categories: Java
tags:
  - springboot
---
# JAVA基本语法
### 标识符
> 变量，常量，方法，枚举，类，接口的你敢由程序员指定的名字。
-  区分大小写
-  首字符，可以是下划线(_)或者美元符或者字母，但不能是数字
-  除首字符外其他字符，可以是下划线(_)，美元符，字母和数字。
- 关键字不能作为标识符
### 关键字
>  Java的全部关键字都是小写字母

### 保留字
>  一些字符序列即不能当作标识符使用，也不是关键字，也不能在程序中使用，这些字符序列被称为保留字。
-  1.goto：在java中不再使用goto语句，因为“无限跳转”语句会破坏程序结构。在java语言中goto的替换语句可以通过break，continue，return实现“有限跳转”。
- 2.const：在其他语言是声明常量关键字，在java语言中声明变量使用public static final方式声明。
  
## Java 分隔符
### 分号
>  分号在java中，它表示一条语句结束。
```java
int i = 1+2+3;
等价于
int i 1+2
+3;
```
### 大括号
> ### 左右大括号括起的语句集合称为语句块(block)或复合语句，在语句块可以有n条语句。在定义类或方法时，语句块也被做分隔类体或方法体。语句块可以嵌套，且嵌套层次没有限制。
```java
package basic_grammar;

public class brace {
    public  static  void  main (String args[]) {
        int m = 5;
        if (m<10) {
            System.out.println(m+"<10");
        }
    }
}
```
### 空百
> ### java源码中需要有空白，空白数不限。
```java
        if (m<10) {
            System.out.println(m+"<10");
        }
        等于
                if (m<10) {
            System.out.println(m+"<10");}
        等于
                if (m<10) 
                {
            System.out.println(m+"<10");
        }
```
### 常量
> ### 常量事实上是那些不可修改的变量，常量和变量类似都需要被初始化。在声明常量的同时需要赋予初始值，常量一旦被初始化就不能被修改。final 数据类型 变量名 = 初始值
```java
package basic_grammar;

public class constant {
//    静态常量，替代const
    public  static  final  double PI = 3.14;
//    声明成员常量
    final  int y = 2;
//    声明局部常量
    public  static  void  main(String[] args){
        final  double pi = 3.14;
    }
}
```
## 数据类型
### 基础数据类型
> ### 基本数据类型分为4大类，8种数据类型。
- ####  整数类型：byte,long,int,short
- #### 浮点类型：float,double
- #### 字符类型：char
- #### 布尔类型：boolean
### 整数类型
> #### byte,long,int,short。它们的区别仅仅在于宽度和范围不同。Java的整数都是有符号的。
|整数类型|宽度|范围|
|:------:|:------:|:-------:|
|byte|1个字节(8位)|-128~127|
|int|4个字节|-2^{31} ~ 2^{31} -1|
|short|2个字节|-2^{15} ~ 2^{15}-1|
|long|8个字节|-2^{63} ~2^{63}-1|
#### Java默认的整数类型为int.long类型需要在后面添加l或者L.
```java
package basic_grammar;

public class Shaping {
    public static void main(String[] args){
        System.out.println("默认整数常量 ="+16);
        byte a = 16;
        short b = 16;
        int c = 16;
        long d = 16L;
        long e = 16l;
        System.out.println("byte整数 = "+a);
        System.out.println("Short整数 = "+b);
        System.out.println("int整数 = "+c);
        System.out.println("long整数 = "+d);
        System.out.println("long整数 = "+e);
    }
}
```
```
默认整数常量 =16
byte整数 = 16
Short整数 = 16
int整数 = 16
long整数 = 16
long整数 = 16
```  
### 浮点类型
>  浮点类型主要用来储存小数数值，也可以用来储存范围较大的整数。它分为浮点数(float)和双精度浮点数(double)。双精度浮点数比浮点数使用的内存空间多，可以表示数字范围和精确度比较大。
|浮点类型|宽度|
|:------:|:------:|
|float|4个字节|
|doble|8个字节|
```java
package basic_grammar;

import org.omg.PortableInterceptor.SYSTEM_EXCEPTION;

public class Floating_point_number {
    public static void main(String[] args){
        System.out.println("默认浮点数 ="+366.66);
        float floatnum = 123.23f;
        double doublenum = 456.12;
        final  double PI = 3.1415926d;

        System.out.println("float  ="+floatnum);
        System.out.println("double ="+doublenum);
        System.out.println("PI ="+PI);

    }
}
```
 浮点数默认是double类型，float浮点常数类型需要在数值后面添加f或者F。double浮点数也可以在后面添加d或者D。
## 数字表示方式
### 进制数表示
-  二进制数：以0b或0B为前缀。
-  八进制数：以0位前缀。
-  十六进制数：以0x或0X为前缀。
```
int decimalint = 28;
int binaryint1 = 0b11100;
int binaryint2 = 0B111100;
int octalint = 034;
int hexadecimalint1 = 0x1C;
int hexadecimalint2 = 0X1C;
```
### 指数表示
>   如果需要采用十进制来表示指数，需要使用大写或小写e来表示。e2 = 10^2
```
double myMoney = 3.36e2;
double interestRate = 1.56e-2;
```
 3.36e2 = 3.36x10^2,1.56e-2 = 1.56x10^-2
### 字符类型
>  字符类型表示单个字符。java中以char来声明字符类型，字符类型必须以单引号括起里的单个字符。
  java字符采用双字节Unicode编码，占两个字节。可以用十六进制编码形式表示、
```java
package basic_grammar;

public class character {
    public static void main(String[] args){
        char c1 = 'A';
        char c2 = '\u0041';
        char c3 = '华';
        System.out.println(c1);
        System.out.println(c2);
        System.out.println(c3);
    }
}
```
```
A
A
华
```
java为了表示一些特殊字符，需要加上反斜杠(\n)，被称之为转义字符。
|字符表示|Unicode编码|说明|
|:------:|:-------:|:------:|
|\t|\u0009|tab|
|\n|\u000a|换行|
|\r|\u000d|回车|
|\"|\u0022|双引号|
|\'|\u0027|单引号|
|\\ |\u005c|反斜线|
```java
package basic_grammar;

public class character2 {
    public static void main(String[] args){
        String tab1 = "Hello\tWorld";
        String tab2 = "Hello\u0009World";
        String Wrap1 = "Hello\nWorld";
        String  enter1 = "Hello\rWorld";
        String Double_quotes1 = "Hello\"World";
        String apostrophe1 = "Hello\'World";
        String Backslash1 = "Hello\\World";


        System.out.println("Tab1 : "+tab1);
        System.out.println("Tab2 : "+tab2);
        System.out.println("Wrap1 : "+Wrap1);
        System.out.println("Enter1 : "+enter1);
        System.out.println("Double_quotes1 : "+Double_quotes1);
        System.out.println("apostrophe1 : "+apostrophe1);
        System.out.println("Backslash1 : "+Backslash1);

    }
}
```
```
Tab1 : Hello    World
Tab2 : Hello    World
Wrap1 : Hello
World
World1 : Hello
Double_quotes1 : Hello"World
apostrophe1 : Hello'World
Backslash1 : Hello\World
```
### 布尔类型
>  java中声明布尔类型的关键字是boolean。不能用0或1来代替，因为不属于数值类型，不能与数字类型等进行转换。
```
boolean isMan = true;
boolean isWoman = false;
```
## 数值类型相互转换
### 自动类型转换
>  自动类型转换就是需要类型之间转换是自动的，不需要任何操作手段。
[![](https://image.kalifun.top/temp/1901/1e4ed96f6a3c8f6f.png)](https://image.kalifun.top/temp/1901/1e4ed96f6a3c8f6f.png)
char可以自动转换为int，long，float，double。但是byte，short不能自动转换为char，且char不能转换为byte，short类型。
### 强制类型转换
>  强制类型转换是在变量或常量前加上（目标类型）实现。强制类型转换主要是用于大宽度类型转换为小宽度类型。
```
int i = 1;
byte b = (byte)i;
```
### 引用数据类型
>  在Java中除了8中数据类型外，其他数据类型都是引用数据类型。引用数据类型用了表示复杂数据类型。包含：类，接口和数组声明的类型。
```java
int x = 1;
int y = x;

String str1 = "Hello";
String str2 = str1;     //此时str1和str2指向相同地址
str2 = "world";      //此时str1和str2指向不同的地址
```
## 算术运算符
### 一元运算符
> #### 一元运算符有三种。-，++，--
|运算符|名称|说明|例子|
|:-----:|:------:|:------:|:------:|
|-|取反符号|取反运算|b=-a|
|++|自加一|先取值在加一，或先加一再取值|a++或++a|
|--|自减一|先取值再减一，或先减一再取值|a--或--a|
```java
package basic_grammar;

public class Operator {
    public static void main(String[] args){
        int a = 10;
        System.out.println(-a);
        int b = a++;
        System.out.println(b);
        int c = ++b;
        System.out.println(c);
        int d = c--;
        System.out.println(d);
        int e = --d;
        System.out.println(e);
    }
}
```
```
-10
10
11
11
10 
```
### 二元运算符　　
> 　二元运算符包括：+，- , * , / 和 %。这些对数值类型都是有效的。  

|运算符|名称|说明|例子|
|:------:|:------:|:-------:|:------:|
|+|加|求和，字符串相连|a+b|
|-|减|求差|a-b|
|*|乘|求积|a*b|
|/|除|求商|a/b|
|%|取余|求余|a%b|
```java
package basic_grammar;

public class Binary {
    public static void main(String[] args){
        char charnum = 'A';
        int intresult = charnum + 1 ;
        System.out.println(intresult);
        intresult = intresult - 1;
        System.out.println(intresult);
        intresult = intresult * 2 ;
        System.out.println(intresult);
        intresult = intresult / 2 ;
        System.out.println(intresult);
        intresult = intresult + 8 ;
        intresult = intresult % 7 ;
        System.out.println(intresult);

        double doubleresult = 10.0;
        System.out.println(doubleresult);

        doubleresult = doubleresult + 1 ;
        System.out.println(doubleresult);
        
        doubleresult = doubleresult -1 ;
        System.out.println(doubleresult);

        doubleresult = doubleresult * 2 ;
        System.out.println(doubleresult);

        doubleresult = doubleresult / 2 ;
        System.out.println(doubleresult);
    }
}
```
```
66
65
130
65
3
10.0
11.0
10.0
20.0
10.0
```
### 算术赋值运算符
|运算符|名称|例子|
|:------:|:------:|:-----:|
|+=|加赋值|a+=b|
|-=|减赋值|a-=b|
|*=|乘赋值|a*=b|
|/=|除赋值|a/=b|
|%=|取余赋值|a%=b|
```java
package basic_grammar;

public class arithmetic {
    public static void main(String[] args){
        int a = 2;
        int b = 3;
        a += b;   //a = a+b
        System.out.println(a);
        a += b + 2 ; //a = a +b +2
        System.out.println(a);
        a -= b;  //a = a-b
        System.out.println(a);
        a *= b;   //a = a*b
        System.out.println(a);
        a /= b ;   //a = a/b
        System.out.println(a);
        a %= b ;    //a = a % b
        System.out.println(a);
    }
}
```
```
5
10
7
21
7
1
```
### 关系运算符
>  关系运算符的结果是布尔类型，True和False。关系运算符有6种：==,<=,>=,!=,>,<.
### 逻辑运算符
>   逻辑运算符是布尔类型进行运算，所以结果也是布尔类型。  
|运算符|名称|说明|例子|
|:------:|:------:|:-------:|:------:|
|!|非|a 为 true 时，值为 false，a 为 false 时，值为 true|a!b|
|&|与|ab 全为 true 时，计算结果为 true，否则为 false|a&b|
| \| |或|ab 全为 false 时，计算结果为 false，否则为 true|a\|b |
| \&&|短路与|ab 全为 true 时，计算结果为 true，否则为 false。&&与&区别：如果 a 为 false，则不计算b|a && b|
|\|\||短路或|ab 全为 false 时，计算结果为 false，否则为 true。\|\|与\|区别：如果 a 为 true，则不计算 b|a\|\|b|

##  控制语句
### if语句
```java
package basic_grammar;

public class java_if {
    public static void main(String[] args){
        int source = 95;
        if (source >= 90){
            System.out.println("A");
        }else  if (source >= 80){
            System.out.println("B");
        }else  if (source >= 70) {
            System.out.println("C");
        }else {
            System.out.println("E");
        }
//        if (source <60) {
//            System.out.println("不及格，加油哦！");
//        }else {
//            System.out.println("真棒，及格。");
//        }
//        if (source>= 85){
//            System.out.println("你真优秀！");
//        }
//        if (source<60){
//            System.out.println("您需要继续加油哦！");
//        }
//        if ((source>60) && (source<=85)) {
//            System.out.println("不错哦，继续努力！");
//        }
    }
}
```
### switch语句
```java
package basic_grammar;

public class Swatch {
    public static void main(String[] args){
        int testsorce = 75;
        char grade;
        switch (testsorce/10){
            case 9:
                grade = '优';
                break;
            case 8:
                grade = '良';
                break;
            case 7:
            case 6:
                grade = '中';
                break;
                default:
                    grade = '差';
        }
        System.out.println("Grade="+grade);
    }
}
```
```
Grade=中
```
### while
```java
package basic_grammar;

public class java_while {
    public static void main(String[] args){
        int i = 0;
        while (i*i<100){
            System.out.println(i);
            i ++;
        }
        System.out.println(i);
    }
}
```
```
0
1
2
3
4
5
6
7
8
9
10
```
### do-while
>  do-while语句使用和while相似，不过do-while是事后判断循环的。
```java
package basic_grammar;

public class java_do_while {
    public static void main(String[] args){
        int i = 0;
        do {
            i++;
            System.out.println(i);
        }while (i*i<100);
    }
}
```
```
1
2
3
4
5
6
7
8
9
10
```