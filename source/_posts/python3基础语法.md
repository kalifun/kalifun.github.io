---
title: Python3.7 の 基础语法
date: 2019-06-19 09:15:00
categories: Python
tags:
  - python
---

# Python3.7 の 基础语法

> 之前一直是使用python2.7，也没有去尝试使用3的版本。今天看到python2.7将维护到2020年。因此打算把3.7基础过一遍。

**我将会记录我印象中和2.7不同的。我2.7基础也不是很好，所以有的内容也可能会重复。**

------



### print

**python2.7**

```python
print "Hello World"
```

**python3.7**

```python
print ("Hello World")
这个应该算是3的特性，并不是指3.7衍生出来的。
```



------

### 字符串

**三重引号： “”“ ……”“”，‘’‘ …… ’‘’。字符串中的回车换行会自动包含到字符串中，如果不想包含，在行尾加入一个\即可。**

```python
print("""\
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
""")
```

```bash
$ python 1.py
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
```

```python
print("""
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
""")
```

```bash
$ python 1.py

Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
```

**相邻的两个或者多个字符串字面值将会自动连接。这个特性我不知道2.7是否有。**

```python
>>> 'p' 'y' 'thon'
'python'
```

**索引，我记得2.7就有这个特性。但是貌似我使用的不多。**

```python
>>> n = "python"
>>> n[1]
'y'
>>> n[-1]
'n'
```

### 列表

**列表可以进行拼接**

```python
>>> a = [1,4,6,8,9]
>>> a + [2,0]
[1, 4, 6, 8, 9, 2, 0]
```

**append()方法添加新元素**

```python
>>> a.append(5*6)
>>> a
[1, 4, 6, 8, 9, 30]
```

**嵌套列表，这个在数据处理上就方便很多啦。**

```python
>>> a
[1, 4, 6, 8, 9, 30]
>>> b = ["a","b"]
>>> b
['a', 'b']
>>> c = [a,b]
>>> c
[[1, 4, 6, 8, 9, 30], ['a', 'b']]
```

### 关键字参数

#### *args 和 **kwargs

首先我们需要关注在这个*上面，其实你可以理解它是有两部分的。

- 打包
- 拆包

```python
def argument(*arg):
    print(arg)
    print(type(arg))

argument(1,2,3,4,56)
```

```bash
$ python arg.py
(1, 2, 3, 4, 56)
<class 'tuple'>
```

∗把函数argument接受到的多个参数1,2,3,4,56，打包成了元组(1,2,3,4,56)，赋值给了形参arg。

根据上面我们就更好理解**kwargs了。

```python
def keywords(**kwargs):
    for kw in kwargs:
        print(kw,kwargs[kw])
    print(type(kwargs))

keywords(a=1,b=2,c=3)
```

```bash
$ python kwargs.py
a 1
b 2
c 3
<class 'dict'>
```

```python
def keywords(a,b,c):
    print(a,b,c)
keywords(**{"a":1,"b":2,"c":3})
```

```bash
$ python kwargs.py
1 2 3
```

**这样就是把字典拆分赋值给形参了。**

```python
def cheeseshop(kind,*argument,**keywords):
    print("-- Do you have any", kind, "?")
    print("-- I'm sorry, we're all out of", kind)
    for arg in argument:
        print(arg)
    print("-" * 40)
    for kw in keywords:
        print(kw,":",keywords[kw])

cheeseshop("Limburger", "It's very runny, sir.",
           "It's really very, VERY runny, sir.",
           shopkeeper="Michael Palin",
           client="John Cleese",
           sketch="Cheese Shop Sketch")
```

```bash
$ python moreval.py
-- Do you have any Limburger ?
-- I'm sorry, we're all out of Limburger
It's very runny, sir.
It's really very, VERY runny, sir.
----------------------------------------
shopkeeper : Michael Palin
client : John Cleese
sketch : Cheese Shop Sketch
```

### Lambda表达式

**可以用lambda关键字来创建一个小的匿名函数。Lambda函数可以在需要的函数对象的任何地方使用。**

```python
>>> add = lambda x,y : x+y
>>> add(1,2)
3
```

#### 应用在函数式编程中

**按照绝对值进行排序**

```python
>>> list = [5,1,3,-1,9]
>>> sorted(list,key=lambda x: abs(x))
[1, -1, 3, 5, 9]
```

#### 应用在闭包中

```python
>>> def get_y(a,b):
	return lambda x: a+b

>>> y1 = get_y(1,1)
>>> y1
<function get_y.<locals>.<lambda> at 0x02DB80C0>
>>> y1(1)
2
>>> y1(2)
2
```

### 函数标注

```python
def label(ham: str,eggs: str = 'eggs') -> str:
    print("Annotations:", label.__annotations__)
    print("Arguments:", ham, eggs)
    return ham + ' and ' + eggs

label('spam')
```

```bash
$ python label.py
Annotations: {'ham': <class 'str'>, 'eggs': <class 'str'>, 'return': <class 'str'>}
Arguments: spam eggs
```

**标注函数一字典的形式存放在函数的\__annotations__属性中,并不会影响函数的任何其他部分。**

**形参标注的定义方式是在形参名称后面加冒号，后面跟个表达式，该表达式会被请求为标注的值。返回值标注的定义形式是加上一个组合符号->，后面跟一个表达式，该标注位于形参列表和表示def语句结束的冒号之间。**

### 列表的更多特性

- **list.append(x)**

  **在列表的末尾添加一个元素。**

  ```python
  >>> a = [1,2]
  >>> a.append(3)
  >>> a
  [1, 2, 3]
  ```



- **list.extend(iterable)**

  **使用可迭代对象中的所有元素来扩展列表。**

  ```python
  >>> aList = [123, 'xyz', 'zara', 'abc', 123];
  >>> bList = [2009, 'manni']
  >>> aList.extend(bList)
  >>> print(aList)
  [123, 'xyz', 'zara', 'abc', 123, 2009, 'manni']
  ```



- **list.insert(i,x)**

  **在给定的位置插入一个元素。第一个参数是要插入的元素的索引。**

  ```python
  >>> aList.insert(0,"first")
  >>> aList
  ['first', 123, 'xyz', 'zara', 'abc', 123, 2009, 'manni']
  ```

  

- **lsit.remove(x)**

  **移除列表第一个值为x的元素。如果没有这样的元素，则抛出ValueError异常。**

  ```python
  >>> aList.remove("x")
  Traceback (most recent call last):
    File "<pyshell#78>", line 1, in <module>
      aList.remove("x")
  ValueError: list.remove(x): x not in list
  >>> aList.remove(123)
  >>> aList
  ['first', 'xyz', 'zara', 'abc', 123, 2009, 'manni']
  ```

  

- **list.pop([i])**

  **删除列表中给定位置的元素并返回它。如果没有给定位置，pop()将会删除并返回列表中的最后一个元素。**

  ```python
  >>> aList
  ['first', 'xyz', 'zara', 'abc', 123, 2009, 'manni']
  >>> aList.pop(1)
  'xyz'
  >>> aList.pop()
  'manni'
  >>> aList
  ['first', 'zara', 'abc', 123, 2009]
  ```

  

- **list.clear()**

  **删除列表中所有的元素。**

  

- **list.index(start,end)**

  **返回列表中第一个值为x的元素的从零开始索引。如果没有这样的元素将会抛出异常。**

  **可选参数start和end是切片符合，用于将搜索限制为列表的特定子序列。返回的所有是相对于整个序列的开始计算的，而不是start参数。**

  ```python
  >>> aList
  ['first', 'zara', 'abc', 123, 2009]
  >>> aList.index('first')
  0
  ```

  

- **list.count(x)**

  **返回元素x在列表中出现的次数**

  

- **list.sort(key=None,reverse=False)**

  **对列表中的元素进行排序**

  

- **list.reverse()**

  **反转列表中的元素**

  

- **list.copy()**

  **返回列表的一个浅拷贝。**

### 包

```python
sound/                          Top-level package
      __init__.py               Initialize the sound package
      formats/                  Subpackage for file format conversions
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  Subpackage for sound effects
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  Subpackage for filters
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
```

**你会发现上面的目录下都有一个一样的文件，\__init__.py**

**因为必须要有这个文件才能让python将包含该文件的目录当做包。**

```python
import sound.effects.echo
```

**我们必须要把全名导入，不然无法使用这个包。这样会不会觉得很麻烦，每次都要一个一个的导入，多了就很费时间。**

当我们想要导入effect目录下所有的包怎么办？

我们将effect目录下的\__init__.py文件进行修改

```python
__all__ = ["echo","surround","reverse"]
```

当我们需要导入包时只需要：

```python
from sound.effects import * 
```

### 输入输出

#### 格式化字符串文字

> [格式化字符串字面值](https://docs.python.org/zh-cn/3/reference/lexical_analysis.html#f-strings) （常简称为 f-字符串）能让你在字符串前加上 `f` 和 `F` 并将表达式写成 `{expression}` 来在字符串中包含 Python 表达式的值。

```python
>>> import math
>>> print(f'The value of pi is approximately {math.pi:.3f}.')
The value of pi is approximately 3.142.
```

**在‘：’后传递一个整数可以让该字段成为最小字符宽度。**

```python
>>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
>>> for name,phone in table.items():
	print(f'{name:10} ==> {phone:10}')

	
Sjoerd     ==>       4127
Jack       ==>       4098
Dcab       ==>       7678
```

**其他的修饰符可用于在格式转化之前转化值。'!a'应用ascii(),'!s'应用str(),还有'!r'应用repr()**



