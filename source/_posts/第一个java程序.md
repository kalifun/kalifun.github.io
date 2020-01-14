---
title:   第一个java程序
date: 2019-01-29 16:51:00
categories: Java
tags:
  - springboot
---
# 第一个java程序
```java 
package hello;

public class helloword {
    public static void  main(String[] args) {
        System.out.print("Hello World.");
    }
}
```
#### 注意
- 只能一个类声明为公有的（public）
- 文件名称必须与公有类名完全一致，包括字母大小写·。
- public static  void main(String[] args) 只能定义在公有类中

# 编译程序
#### 编译需要到程序根目录下利用javac来编译
```bash 
E:\java_sty\src\hello>ls
helloword.java

E:\java_sty\src\hello>javac helloword.java

E:\java_sty\src\hello>ls
helloword.class  helloword.java
```
### 此时目录下生产了一个helloworld.class类文件。
### 若定义的包不是在这个目录下，需要添加-d 参数。将会自动创建文件夹及文件。
``` java
package create1.create2;

public class hello2 {
    public static  void  main(String[] args){
        System.out.print("Hello world!");
    }
}
```
``` sh
E:\java_sty\src\hello2>ls
hello2.java

E:\java_sty\src\hello2>javac hello2.java

E:\java_sty\src\hello2>ls
hello2.class  hello2.java

E:\java_sty\src\hello2>javac -d . hello2.java

E:\java_sty\src\hello2>ls
create1  hello2.class  hello2.java

E:\java_sty\src\hello2>cd create1

E:\java_sty\src\hello2\create1>cd create2

E:\java_sty\src\hello2\create1\create2>ls
hello2.class
```
# 运行程序
```
java xxxx.xxx
```