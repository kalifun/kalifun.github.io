---
title: Python面试题-基础篇
date: 2019-03-11 15:44:00
categories: Python
tags:
  - 面试
  - python
---
# Python面试题-基础篇
## 文件操作
1.有一个jsonline格式的文件file.txt大小约为10K
``` python
def get_lines():
    with open('file.txt','rb') as f:
        return f.readlines()

if __name__ == '__main__':
    for e in get_lines():
        process(e) # 处理每一行数据
```
 现在要处理一个大小为10G的文件，但是内存只有4G，如果在只修改get_lines 函数而其他代码保持不变的情况下，应该如何实现？需要考虑的问题都有那些？
``` python
def get_lines():
    with open('file.txt','rb') as f:
        for i in f:
            yield i
```
因为内存只有4G，无法一次性读取10G文件，需要分批读取内容。每次读取内容的同时需要记录读取的位置，不然每次readline时，又是从第一行开始读取。
>  [yield](yield.md)生成器
>   [mmap](mmap.md)系统内存映射函数接口
```python
from mmap import mmap

def get_lines(filename):
    with open(filename,'r+') as f:
        m = mmap(f.fileno(),0)
        tmp = 0
        for i,char in enumerate(m):
            if char == "\n":
                yield m[tmp:i+1].decode()
                tmp = tmp+1
if __name__ == "__main__":
    for i in get_lines("mmap_test.txt"):
        print i
```
```
def get_lines():

ef get_lines():
    with open('file.txt','rb') as f:

f get_lines():
    with open('file.txt','rb') as f:
        for i in f:
```
#### mmap_test.txt
```txt
def get_lines():
    with open('file.txt','rb') as f:
        for i in f:
            yield i
```
 似乎使用mmap并不是特别好使，这样确实可以记录读取到行。似乎对tab比较多的并友好。
### 2.补充缺失的代码
``` python
import os
sPath = os.getcwd()
print sPath
def print_directory_contents(sPath):
    for all_data in os.listdir(sPath):
        files_path = os.path.join(sPath,all_data)
        if os.path.isdir(files_path):
            print_directory_contents(files_path)
        else:
            print files_path
```
### 3.输入日期， 判断这一天是这一年的第几天？
``` python
# -*- coding:utf-8 -*-
import datetime
def dayofyear():
    year = raw_input("请输入年份: ")
    month = raw_input("请输入月份: ")
    day = raw_input("请输入天: ")
    date1 = datetime.date(year=int(year),month=int(month),day=int(day))
    date2 = datetime.date(year=int(year),month=1,day=1)
    print (date1-date2).days+1
dayofyear()
```
### 4.打乱一个排好序的list对象alist？
``` python
import random
alist = [1,2,3,4,5]
random.shuffle(alist)
print alist
```
### 5.现有字典 d= {'a':24,'g':52,'i':12,'k':33}请按value值进行排序?
```python
d= {'a':24,'g':52,'i':12,'k':33}
a = d.items()
print a
f = lambda x:x[1]
print f(a)
b = sorted(d.items(),key=lambda x:x[1])
print b
```
```bash
[('a', 24), ('i', 12), ('k', 33), ('g', 52)]
('i', 12)
[('i', 12), ('a', 24), ('k', 33), ('g', 52)]
```
  lambda是一个隐函数，是固定写法，不要写成别的单词；x表示列表中的一个元素，在这里，表示一个元组，x只是临时起的一个名字，你可以使用任意的名字；x[0]表示元组里的第一个元素，当然第二个元素就是x[1]