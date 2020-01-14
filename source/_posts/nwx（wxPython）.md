---
title: nwx（wxPython）
date: 2019-01-19 17:35:00
categories: Python
tags:
  - python
---
# wx（wxPython）
## 前言
 使用了很多模块心血来潮觉得要写一些相关模块的使用文档。并想把常用的模块写成一个自己常用的类。这样只需要调用就行了，可以对自己写东西的时候节省时间。
## 安装
```python
pip install -U wxPython
```
## 创建一个简单程序
```python
# First things, first. Import the wxPython package.
import wx

# Next, create an application object.
app = wx.App()

# Then a frame.
frm = wx.Frame(None, title="Hello World")

# Show it.
frm.Show()

# Start the event loop.
app.MainLoop()
```
## GUI设计
>  wx.BoxSizer()
 这个可以设置你的box的比例。proportion比例设置。
```python
url_text = wx.StaticText(self.panel,label=u"路径：")
self.path_text = wx.TextCtrl(self.panel,wx.TE_MULTILINE)
openfile_text = wx.Button(self.panel,label=u"打开")
openfile_text.Bind(wx.EVT_BUTTON,self.readfile)
self.content_text = wx.TextCtrl(self.panel,style =wx.TE_MULTILINE)
box = wx.BoxSizer()
box.Add(url_text,proportion = 0.5,flag = wx.ALIGN_CENTER|wx.ALIGN_RIGHT,border=3)
box.Add(self.path_text,proportion = 3,flag = wx.EXPAND|wx.ALL,border=3)
box.Add(openfile_text,proportion = 1,flag = wx.EXPAND|wx.ALL,border=3)
```
### 设置了0.5|3|1
[![](https://image.kalifun.top/upload/1902/270a32000255164c.png)](https://image.kalifun.top/upload/1902/270a32000255164c.png)
```python
v_box = wx.BoxSizer(wx.VERTICAL)
v_box.Add(box,proportion = 0.5,flag=wx.EXPAND|wx.ALL,border=3)
v_box.Add(self.content_text,proportion = 5,flag=wx.EXPAND|wx.ALL,border=3)
```
### 上下分为两个部分，上下分为0.5|5
 设置了 0.5|5
[![](https://image.kalifun.top/upload/1902/741a19ade9085a02.png)](https://image.kalifun.top/upload/1902/741a19ade9085a02.png)
## 创建事件
>  创建事件基本发生在button上。创建事件关键词bind
```python
openfile_text.Bind(wx.EVT_BUTTON,self.readfile)
def readfile(self,event):
```
## 完全代码
```python
# -*- coding:utf-8 -*-
import wx
import os

class OpenfileTool(wx.Frame):
    def __init__(self,parent,title):
        wx.Frame.__init__(self, parent, title=title,size=(500,400))
        self.panel = wx.Panel(self)
        self.content()

 

    def content(self):
        url_text = wx.StaticText(self.panel,label=u"路径：")
        self.path_text = wx.TextCtrl(self.panel,wx.TE_MULTILINE)
        openfile_text = wx.Button(self.panel,label=u"打开")
        openfile_text.Bind(wx.EVT_BUTTON,self.readfile)
        self.content_text = wx.TextCtrl(self.panel,style =wx.TE_MULTILINE)
        box = wx.BoxSizer()
        box.Add(url_text,proportion = 0.5,flag = wx.ALIGN_CENTER|wx.ALIGN_RIGHT,border=3)
        box.Add(self.path_text,proportion = 3,flag = wx.EXPAND|wx.ALL,border=3)
        box.Add(openfile_text,proportion = 1,flag = wx.EXPAND|wx.ALL,border=3)

        v_box = wx.BoxSizer(wx.VERTICAL)
        v_box.Add(box,proportion = 0.5,flag=wx.EXPAND|wx.ALL,border=3)
        v_box.Add(self.content_text,proportion = 5,flag=wx.EXPAND|wx.ALL,border=3)
        self.panel.SetSizer(v_box)

    def readfile(self,event):
        with wx.FileDialog(self, "Open XYZ file", wildcard="txt files (*.txt)|*.txt",style=wx.FD_OPEN | wx.FD_FILE_MUST_EXIST) as fileDialog:

            if fileDialog.ShowModal() == wx.ID_CANCEL:
                return     
            # the user changed their mind

            # Proceed loading the file chosen by the user
            pathname = fileDialog.GetPath()
            self.path_text.SetValue(pathname)
            try:
                with open(pathname, 'r') as file:
                    # datas = file.readlines()
                    # for data in datas:
                    #     self.loadfile(data)
                    self.content_text.SetValue(file.read())
            except IOError:
                wx.LogError("Cannot open file '%s'." % newfile)
    # def loadfile(self,data):
    #     self.content_text.SetValue(data)
if __name__ == "__main__":
    app = wx.App(True)
    frame = OpenfileTool(None,"OpenfileTool")
    frame.Show()
    app.MainLoop()
```