---
title:   Java面向对象
date: 2019-01-31 16:51:00
categories: Java
tags:
  - springboot
---
# 面向对象
### 方法重载
> #### MethodOverloading.java
```java
package basic_grammar;

public class MethodOverloading {
    void receive(int i){
        System.out.println("接收到一个int参数");
        System.out.println("i = "+i);
    }
    void receive(int x,int y){
        System.out.println("接收到两个int参数。");
        System.out.printf("x = %d,y = %d \n",x,y);
//        System.out.println();
    }

    int receive(double x,double y){
        System.out.println("接收到两个double参数。");
        System.out.printf("x = %f , y = %f \r",x,y);
        return  0;
    }
}
```
> ####  mo_class.java
```java

package basic_grammar;

public class mo_class {
    public static void main(String[] args){
        MethodOverloading mo = new MethodOverloading();

        mo.receive(1);
        mo.receive(2,3);
        mo.receive(2.0,3.3);
    }
}
```
```
接收到一个int参数
i = 1
接收到两个int参数。
x = 2,y = 3 
接收到两个double参数。
x = 2.000000 , y = 3.300000
```
## 封装性与访问控制
> #### Java 面向对象的封装性是通过对成员变量和方法进行访问控制实现的，访问控制分为 4 个等级：私有、默认、保护和公有
|控制等级|同一个类|同一个包|不同包的子类|不同包非子类|
|:------:|:------:|:------:|:------:|:------:|
|私有|yes| | | |
|默认|yes|yes| | |
|保护|yes|yes|yes| |
|公有|yes|yes|yes|yes|
### 私有级别
> #### 私有级别的关键词是private，私有级别的成员变量和方法只能在其所在的类中自由使用，不允许其他类自由访问。私有级别限制性最高。
> #### PrivateClass.java
```java
package basic_grammar;

public class PrivateClass {
    private int x ;
    public PrivateClass(){
        x = 5 ;
    }
    private void printX() {
        System.out.println("Values of x is "+x);
    }
}
```
> #### PC_class
```java
package basic_grammar;

public class PC_class {
    public static void main(String[] args){
        PrivateClass p;
        p = new PrivateClass();
        p.printX();
    }
}
```
```
Error:(7, 10) java: printX() 在 basic_grammar.PrivateClass 中是 private 访问控制
```
#### p.printX();是私有的所以无法调用。
### 默认级别
> #### 默认级别没有关键词，也就是没有访问修饰符，在不同包的类中是不允许被访问的。
> #### DefaultClass.java
```java
package basic_grammar;

public class DefaultClass {
    int x;
    public DefaultClass(){
        x = 100;
    }
    void printX(){
        System.out.println("x = "+x);
    }
}
```
> #### DC_class.java
```java
package basic_grammar;

public class DC_class {
    public static void main(String[] args){
        DefaultClass p;
        p = new DefaultClass();
        p.printX();
    }
}
```
```
x = 100
```
#### 若在不同包体去编译执行，将会报错。
### 公有级别
> #### 公有级别的关键词是 public，公有级别的成员常量和方法可以再任何场合被直接调用。
 > #### PublicClass.java
 ```java
  package basic_grammar;

public class PublicClass {
    public int x ;
    public PublicClass(){
        x = 100;
    }
    public void printX(){
        System.out.println("x = "+x);
    }
}
  ```
  > ####  PublicClass_class.java
  ```java
  package basic_grammar.basic_grammar;

import basic_grammar.PublicClass;

public class PublicClass_class {
    public static void main(String[] args){
        PublicClass p;
        p = new PublicClass();
        p.printX();
    }
}
  ```
  ```
  x = 100
  ```
  ### 保护级别
  > #### 保护级别的关键词protected，保护级别在同一包中完全与默认访问级别一样，但不同包中子类可继承父类中的protected的变量和方法。
  > #### ProtectedClass.java
```java
package basic_grammar;

public class ProtectedClass {
    protected int x;
    public ProtectedClass(){
        x = 100;
    }
    protected void printX(){
        System.out.println("value of is "+x);
    }
}
```
> ####  ProtectedClass_class.java
```java
package basic_grammar;

public class ProtectedClass_class {
    public static void main(String[] args){
        ProtectedClass p;
        p = new ProtectedClass();
        p.printX();
    }
}
```
#### 同一个包下保护级别和默认级别没有区别，不同包下也无法直接访问printX。需要在不同包内继承ProtectedClass类。
```java
package basic_grammar.basic_grammar;

import basic_grammar.ProtectedClass;

public class ProtectedClass_class {
    public class SubClass extends ProtectedClass{
        void diskplay(){
            printX();
            System.out.println(x);
        }
    }
}
```
```
value of is 100
```
#### 不同包中SubClass从ProtectedClass类中继承了printX()方法和x实例变量。
> ####  访问成员有两种方式：一种是调用，通过类或对象调用成员，如p.printX().另一种是继承，子类继承父类的成员变量和方法。

### 静态变量和静态方法
> #### Account.java
```java
package basic_grammar;

public class Account {
    double mount = 0.0;
    String owner ;
    static double interestRate = 0.0668;
    public static double interestBy(double amt){
        return interestRate * amt;
    }
    public String messageWith(double amt){
        double interest = Account.interestBy(amt);
        StringBuilder sb = new StringBuilder();
        sb.append(owner).append("的利息是").append(interest);
        return sb.toString();
    }
}
```
> #### Account_class
```java
package basic_grammar;

public class Account_class {
    public static void main(String[] args){
        System.out.println(Account.interestRate);
        System.out.println(Account.interestBy(1000));
        Account myaccount = new Account();
        myaccount.mount =  1000000;
        myaccount.owner =  "Tony";
        System.out.println(myaccount.messageWith(1000));
        System.out.println(myaccount.interestRate);
    }
}
```
```
0.0668
66.8
Tony的利息是66.8
0.0668
```
##  对象
> #### 实例化可生产对象，实例方法就是对象方法 ，实例变量就是对象属性。一个对象的周期包括三个阶段：创建，使用和销毁。
### 创建对象
> #### 创建对象包括两个步骤：声明和实例化。
#### 声明
> ##### 声明对象与声明普通变量没有什么区别。
```
type objectName;
```
##### type是引用类型，即类，接口和数组。
```
String  name;
```
##### 该语句声明了字符串类型对象name。可以声明并不为对象分配内存空间，而只是分配一个引用。
#### 实例化
> ##### 实例化分为两个阶段：为对象分配内存空间和初始化对象。首先使用new运算符为对象分配内存空间，然后再调用构造方法初始化对象。
```java
String name ;
name = new String("Hello World");
```
### 空对象
> #### 一个引用变量没有通过new分配内存空间，这个对象就是空对象，java使用null表示空对象。
```
String name = null;
name = "Hello World";
```
> #### 引用变量默认值是 null。当试图调用一个空对象的实例变量或实例方法时，会抛出空指针异常 NullPointerException
### 构造方法
> #### 构造方法名必须与类名相同。
> #### 构造方法没有任何返回值。
> #### 构造方法只能与new结合使用。
```java
package basic_grammar;

public class Rectangle {
    int width;
    int height;
    int area;
    public Rectangle(int w,int h){
        width = w ;
        height = h ;
        area = getArea(w,h);
    }
}
```
### 默认构造方法
> #### 默认构造方法的方法体内无任何语句，也就不能够初始化成员变量了。
### 构造方法重载
> #### 在一个类中可以多个构造方法，他们具有相同的名字，参数列表不同，所以他们之间一定是重载关系。
```java
package basic_grammar;

import java.util.Date;

public class Person {
    private String name ;
    private int age ;
    private Date birthday ;
    public Person(String n,int a,Date b){
        name = n;
        age = a ;
        birthday = b ;
    }
    public  Person(String n ,int a){
        name = n ;
        age = a ;
    }
    public Person(String n, Date b){
        name = n ;
        age = 30 ;
        birthday = b ;
    }
    public  Person(String n){
        name = n ;
        age = 30;
    }
    public String  getinfo(){
        StringBuilder sb = new StringBuilder();
        sb.append("名字：").append(name).append("\n");
        sb.append("年龄：").append(age).append("\n");
        sb.append("出生日：").append(birthday).append("\n");
        return  sb.toString();
    }
}
```
### 构造方法封装
> #### 构造方法可以封装，访问级别和普通方法一样。
### this关键词
> #### this指向对象本身，一个类可以通过this来获得一个代表它自身的对象变量。
> #### [ ]调用实例变量。
> #### [ ]调用实例方法。
> #### [ ]调用其他构造方法。
