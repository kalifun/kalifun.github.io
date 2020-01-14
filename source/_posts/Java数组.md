---
title: Java数组
date: 2019-01-30 16:51:00
categories: Java
tags:
  - springboot
---
# 数组
## 一维数组
### 数组初始化
#### 静态数组初始化
>  静态数组初始化是将元素放入大括号中。
```java
int intarray;
intarray = {1,2,3};
```
#### 动态数组初始化
>   动态数组初始化是由new运算符分配长度的内存空间。
```
int array;
array = new int[4];
array[0] = 1;
array[1] = 2;
array[2] = 3;
array[3] = 4; 
```
### 数组合并
>   数组的长度是不可变化的，想要将两个数组合并。需要创建一个新的数组，新数组的长度是两个数组长度的和。然后将两个数组的内容导入新的数组中。
```java
package basic_grammar;

public class merge {
    public static void main(String[] args){
        int array1[] = {1,2,3,4,5,6};
        int array2[] = {7,8,9};
        int array[] = new int[array1.length+array2.length];

        for (int i = 0;i<array.length;i++){
            if (i<array1.length){
                array[i] = array1[i];
            }else {
                array[i] = array2[i-array1.length];
            }
        }
        System.out.println("合并后：");
        for (int item : array){
            System.out.printf("%d",item);
        }
    }
}
```
```
合并后：
123456789
```
##  二维数组
### 二维数组声明
>  二维数组声明需要两个zhong'ku'y中括号
>  元素数据类型[][] 数组变量名;
>  元素数据类型 数组变量名[][];
>  元素数据类型[] 数组变量名[];
```bash
int[][] array1;
int array2[][];
int[] array3[];
```
### 二维数组初始化
#### 静态初始化
```bash
int array[][] = {{1,2,3},{4,5,6},{7,8,9}};
```
 上述是创建并初始化了3x3的二维数组。
#### 动态初始化
>  new 元素数据类型[高维数组长度][低维数组长度]   高维数组就是最外层的数组，低维数组是每个元素的数组。
```java
package basic_grammar;

public class Two_dimensional_array {
    public static void main(String[] args){
        int[][] array = {
                {1,2,3},
                {4,5,6},
                {7,8,9}
        };
        double[][] doublearray = new double[3][3];
        System.out.println(array.length);
        for (int i = 0;i < array.length;i++){
            for (int j = 0;j < array[i].length;j++){
                doublearray[i][j] = Math.sqrt(array[i][j]);
            }
        }
        for (int i = 0;i < doublearray.length;i++){
            for (int j = 0;j < doublearray[i].length;j++){
                System.out.printf("[%d][%d] =  %f",i,j,doublearray[i][j]);
                System.out.print("\t");
            }
            System.out.println();
        }
    }
}
```
```
3
[0][0] =  1.000000	[0][1] =  1.414214	[0][2] =  1.732051	
[1][0] =  2.000000	[1][1] =  2.236068	[1][2] =  2.449490	
[2][0] =  2.645751	[2][1] =  2.828427	[2][2] =  3.000000 
```
### 不规则数组
#### 不规则数组初始化
```
int[][] array = new  int[4][];
array[0] = new int[2];
array[1] = new int[1];
array[2] = new int[5];
array[3] = new int[2];
```
```java
package basic_grammar;

public class irregular {
    public static void main(String[] args){
        int[][] array = new int[3][];
        array[0] = new int[2];
        array[1] = new int[3];
        array[2] = new int[2];
        for (int i = 0;i < array.length;i ++){
            for (int j = 0;j < array[i].length;j++){
                array[i][j] = i+j;
            }
        }
        for (int[] x : array){
            for (int y : x){
                System.out.print(y);
                System.out.print("\t");
            }
            System.out.println();
        }
    }
}
```
```
0	1	
1	2	3	
2	3	
```
## 字符串
### Java中的字符串
>  Java SE 提供了三个字符串类：String,StringBuffer和StringBuilder。String是不可变字符串，StringBuffer和StringBuilder是可变字符串。
### 字符串拼接
>  String虽然是不可变字符串，但是也可以进行拼接只是会产生一个新的对象。String字符串拼接可以使用+或者String的concat方法。
```
String s1 = "Hello";
String s2 = s1 + " ";
String s3 = s2 + "world!";
System.out.println(s3);

String s4 = "Hello";
String s5 = s4 + concat(" ")+concat("World!");
System.out.println(s5);
```
### 字符串查找
>  String类中提供了indexOf和lastIndexOf方法用于查找字符或字符串。，返回值是查找的字符或字符串的位置，-1表示未找到。
-  int indexOf(int ch)从前往后搜索字符ch，返回第一次查找到字符ch所在处的索引。
-  int indexOf(int ch,int fromIndex):从指定的索引开始从前往后搜索字符ch，返回第一次查找到的字符。
-  int indexOf(String str):从前往后搜索字符串str，返回第一次查找到字符串的索引位置。
-  int indexOf(String str,int fromIndex): 从指定的索引 开始从前往后查找字符串，返回第一次查找到字符串索引位置。
-  int lastIndexOf(int ch)：从后往前搜索字符串，返回第一次查找到字符所在处。
-  int lastIndexOf(int ch，int fromIndex)：从指定索引开始从后往前查找字符。返回第一次查找到字符索引所在位置。
-  int lastIndexOf(String str)：从后往前搜索字符串str，返回第一次 查找字符串所在位置。
-  int lastIndexOf(String str,  int fromIndex)：从指定索引开始从后往前搜索，返回第一次字符串所在处索引。
```java
package basic_grammar;

public class java_str {
    public static void main(String[] args){
        String sourceStr =  "There is a string accessing example.";
        int len = sourceStr.length();
        char ch = sourceStr.charAt(16);

        int firstchart1 = sourceStr.indexOf('r');
        int lastchart1 = sourceStr.lastIndexOf('r');
        int firststr1 = sourceStr.indexOf("ing");
        int laststr1 = sourceStr.lastIndexOf("ing");
        int firstchrat2 = sourceStr.indexOf('e',15);
        int lastchart2 = sourceStr.lastIndexOf('e',15);
        int firststr2 = sourceStr.indexOf("ing",5);
        int laststr2 = sourceStr.lastIndexOf("ing",5);


        System.out.println("原始数据："+sourceStr);
        System.out.println("字符串长度："+len);
        System.out.println("索引16的字符："+ch);
        System.out.println("字符r从前往后，所在索引位置："+firstchart1);
        System.out.println("字符r从后往前，所在索引位置："+lastchart1);
        System.out.println("字符串ing从前往后，所在索引位置："+firststr1);
        System.out.println("字符串ing从后往前，所在索引位置："+laststr1);
        System.out.println("索引从15开始，从前往后搜索e字符，所在索引位置："+firstchrat2);
        System.out.println("索引从15开始，从后往前搜索e字符，所在索引位置："+lastchart2);
        System.out.println("索引从5开始，从前往后搜索ing字符串，所在索引位置："+firststr2);
        System.out.println("索引从5开始，从后往前搜索ing字符串，所在索引位置："+laststr2);
    }
}
```
```
原始数据：There is a string accessing example.
字符串长度：36
索引16的字符：g
字符r从前往后，所在索引位置：3
字符r从后往前，所在索引位置：13
字符串ing从前往后，所在索引位置：14
字符串ing从后往前，所在索引位置：24
索引从15开始，从前往后搜索e字符，所在索引位置：21
索引从15开始，从后往前搜索e字符，所在索引位置：4
索引从5开始，从前往后搜索ing字符串，所在索引位置：14
索引从5开始，从后往前搜索ing字符串，所在索引位置：-1
```
### 字符串比较
> #### 字符串比较常见操作，包括比较大小，比较相等，比较前缀和后缀等
##### 比较相等
-  boolean equals(Object anObject):比较两个字符串内容是否相等。
-  boolean equalsIgnoreCase(String anotherString)：类似equals方法，只是忽略大小写。
##### 比较大小
-  int compareTo(String anotherString)：安装字典顺序比较大小。如果参数字符串等于此字符串，则返回值 0；如果此字符串小于字符串参数，则返回一个小于0 的值；如果此字符串大于字符串参数，则返回一个大于 0 的值。
-  int compareToIgnoreCase(String str)：类似compareTo，只是忽略大小写。
##### 比较前缀后缀
-  boolean endsWith(String suffix)：测试次字符串是否是指定的后缀结束。
-  boolean startsWith(String prefix)：测试字符串是否是指定的前缀开始。
```java
package basic_grammar;

public class Str_Comparison {
    public static void main(String[] args){
        String s1 = new String("Hello");
        String s2 = new String("Hello");
        System.out.println("s1 == s2 :"+(s1 == s2));
        System.out.println("s1.equals(s2):"+(s1.equals(s2)));

        String s3 = "HeLLo";
        System.out.println("s1.equalsIgnoreCase(s3):"+s1.equalsIgnoreCase(s3));

        String s4 = "java";
        String s5 = "Switch";
        System.out.println("s4.compareTo(s5):"+s4.compareTo(s5));
        System.out.println("s4.compareToIgnoreCase(s5):"+s4.compareToIgnoreCase(s5));

        String[] docForder = { "java.docx", "JavaBean.docx", "Objecitve-C.xlsx", "Swift.docx" };
        int doccount = 0 ;
        for (String doc : docForder){
//            去除前后空格
            doc = doc.intern();
//            比较后缀是否是.docx
            if (doc.endsWith(".docx")){
                doccount ++ ;
            }
        }
        System.out.println("查找到docx后缀的文件个数："+doccount);

        int javadocnumber = 0;
        for (String doc : docForder) {
            doc = doc.intern();
            doc = doc.toLowerCase();
            if (doc.startsWith("java")){
                javadocnumber ++ ;
            }
        }
        System.out.println("关于java相关文件个数："+javadocnumber);
    }
}
```
```
s1 == s2 :false
s1.equals(s2):true
s1.equalsIgnoreCase(s3):true
s4.compareTo(s5):23
s4.compareToIgnoreCase(s5):-9
查找到docx后缀的文件个数：3
关于java相关文件个数：2
```
### 字符串的截取
-  String substring(int beginIndex)：从指定索引 beginIndex 开始截取一直到字符串结束的子字符串。
-  String substring(int beginIndex, int endIndex)：从指定索引 beginIndex 开始截取直到索引 endIndex - 1 处的字符，注意包括索引为 beginIndex 处的字符，但不包括索引为 endIndex 处的字符。
```java
package basic_grammar;

public class Str_Intercept {
    public static void main(String[] args){
        String sourceStr = "There is a string accessing example.";
        String subStr1 = sourceStr.substring(28);
        String subStr2 = sourceStr.substring(11,17);
        System.out.printf("subStr1 = %s%n",subStr1);
        System.out.printf("subStr2 = %s%n",subStr2);

        System.out.println("-----使用split-----");
        String[] array = sourceStr.split(" ");
        for (String str : array){
            System.out.println(str);
        }
    }
}
```
```
subStr1 = example.
subStr2 = string
-----使用split-----
There
is
a
string
accessing
example.
```
## 可变字符串
>   可变字符串在追加、删除、修改、插入和拼接等操作不会产生新的对象。
###  StringBuffer 和 StringBuilder
>  StringBuffer 和 StringBuilder 具有完全相同的 API，即构造方法和方法等内容一样。
-  StringBuilder()：创建字符串内容是空的 StringBuilder 对象，初始容量默认为 16个字符。
-  StringBuilder(CharSequence seq)：指定 CharSequence 字符串创建 StringBuilder 对象。CharSequence 接口类型，它的实现类有：String、StringBuffer 和 StringBuilder等，所以参数 seq 可以是 String、StringBuffer 和 StringBuilder 等类型。
-  StringBuilder(int capacity)：创建字符串内容是空的 StringBuilder 对象，初始容量由参数 capacity 指定的。
-  StringBuilder(String str)：指定 String 字符串创建 StringBuilder 对象。
```java
package basic_grammar;

public class variable_str {
    public static void main(String[] args){
        StringBuilder sbuilder = new StringBuilder();
        System.out.println("包含字符串长度："+sbuilder.length());
        System.out.println("字符串缓冲区容量："+sbuilder.capacity());

        StringBuilder sbuilder1 = new StringBuilder("Hello");
        System.out.println("包含字符串长度："+sbuilder1.length());
        System.out.println("字符串缓冲区容量："+sbuilder1.capacity());

        StringBuilder sbuilder2 = new StringBuilder();
        for (int i = 0;i<17;i++){
            sbuilder2.append(8);
        }
        System.out.println("包含字符串长度："+sbuilder2.length());
        System.out.println("字符串缓冲区容量："+sbuilder2.capacity());
    }
}
```
```
包含字符串长度：0
字符串缓冲区容量：16
包含字符串长度：5
字符串缓冲区容量：21
包含字符串长度：17
字符串缓冲区容量：34
```
### 字符串追加
>  StringBuilder 在提供了很多修改字符串缓冲区的方法，追加、插入、删除和替换等.字符串追加方法append.
```java
package basic_grammar;

public class add_str {
    public static void main(String[] args){
        StringBuilder sbuilder1 = new StringBuilder("Hello");
        sbuilder1.append(" ").append("World");
        sbuilder1.append(".");
        System.out.println(sbuilder1);

        StringBuilder sbuilder2 = new StringBuilder();
        Object obj = null;
        sbuilder2.append(false).append('\t').append(obj);
        System.out.println(sbuilder2);

        StringBuilder sbuilder3 = new StringBuilder();
        for (int i =0;i<9;i++){
            sbuilder3.append(i);
        }
        System.out.println(sbuilder3);
    }
}
```
```
Hello World.
false	null
012345678
```
### 字符串插入，删除，替换
>  StringBuilder insert(int offset, String str)：在字符串缓冲区中索引为 offset 的字符位置之前插入 str，insert 有很多重载方法，可以插入任何类型数据。
>  StringBuffer delete(int start, int end)：在字符串缓冲区中删除子字符串，要删除的子字符串从指定索引 start 开始直到索引 end - 1 处的字符。start 和 end 两个参数与 substring(int beginIndex, int endIndex)方法中的两个参数含义一样。
>  StringBuffer replace(int start, int end, String str)字符串缓冲区中用 str 替换子字符串，子字符串从指定索引 start 开始直到索引 end - 1 处的字符。start 和 end 同delete(int start, int end)方法。
```java
package basic_grammar;

public class java_insert_del_update {
    public static void main(String[] args){
        String str1 = "Java C";
        StringBuilder stb = new StringBuilder(str1);

        stb.insert(4," C++");
        System.out.println(stb);

        stb.insert(stb.length()," Object-C");
        System.out.println(stb);

        stb.append(" and Switch");
        System.out.println(stb);

        stb.delete(11,23);
        System.out.println(stb);
    }
}
```
```
Java C++ C
Java C++ C Object-C
Java C++ C Object-C and Switch
Java C++ C  Switch
```
## 面向对象特性
>  封装性，继承性，多态性。
## 类
### 类声明
>  类的实现包括：类声明和类体。
```java
[public][abstract|final] class className [extends superclassName] [implements interfaceNameList] {
//类体
}
```
class是声明类的关键词，className是自定义的类名，class前面的修饰符public，abstract，final用来声明类，他们也可以省略。
### 成员变量
```java
class className {
[public | protected | private ] [static] [final] type variableName; //成员变量
}
```
#### type是成员变量数据类型，variableName是成员变量名称。
>  public、protected 和 private 修饰符用于封装成员变量。
>  static 修饰符用于声明静态变量，所以静态变量也称为“类变量”。
>  final 修饰符用于声明变量，该变量不能被修改。