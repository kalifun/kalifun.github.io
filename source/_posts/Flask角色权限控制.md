---
title: Flask角色权限控制
date: 2019-11-18 09:15:00
categories: Python
tags:
  - python
  - flask
---

# Flask角色权限控制

## 前言

目前flask的组件并不能完美的解决角色权限控制问题。大部分组件都是check是否登录，每次请求检查session？

这并不是我想要的啊！我很喜欢前后端分离的理念，我不喜欢使用Python 模板语言，我觉得很臃肿。前端我还是很推荐使用vue，react等。后端很推荐REST架构设计，这样感觉思路清晰很多。

## 为啥选择Flask

我只能用简单易用来形容这个框架。Django = Flask + 插件

Django对于熟悉整个框架的人来说还是很方便的，因为他超多功能已经内置了，不需要像Flask装各种插件来满足自己的使用。但是Flask学习成本比较低不需要去学习一个庞大的系统。

## 认证方式

> 认证方式：Basic Auth，Token，OAuth等

**Basic Auth：**

​		通过用户名密码来验证，会不会太麻烦了，针对每个资源都要被要求认证（每次都要输入账号密码），我是脑子瓦特了？这时候有人会说我可以用session，cookie来维持会话啊，这样登录一次不就好了。这个已经涉及到另一种认证方式了。

​		客户端将输入的用户名密码用Base64进行编码后，采用非加密的明文方式传送给服务器。但是这种方式是比较可逆的。数据易抓取导致账号被盗取利用。**

**Token Auth：**

​		当你登录成功，服务端将会返回一个Token给你，当你每次请求时Header头携带Token进行资源请求验证，在这个过程已经不再涉及用户名密码，session，cookie了，做到无状态请求。且支持跨域访问等。

​		我个人还是很推荐使用Token认证的，而且我觉得也是简单方便。

**OAuth：**

​		这个认证一般第三方认证用的会比较多，你只需要用同一个账号密码，就能在各个网站进行访问，而免去了在每个网站都进行注册的繁琐过程。 

​		这个在公司内网采用的最为  ，只需要认证一次就可以请求公司内网的各种资源。

我谈及的并不深入，只是个人的看法和认知，想要更加深入各位大佬可以寻找文章进行学习。

## 认证流程

![](https://image.kalifun.top/upload/1908/76c35ea76c5d0b6f.png)

这个是大概的一个描述，接下我将讲一下我是如何实现角色权限控制的。

## 角色权限控制

> 这个并没有正对单个权限进行验证,这个也是不足地方。

假设我们有普通户，管理员，超级管理员。我们对每个接口进行验证角色是否有请求权限。

### 装饰器functools

```
from functools import wraps
def Authen(rolename):
	def check(func):
	@wraps(func)
        def checkrole(*args,**kwargs):
            if "admin" in rolename:
                func(*args,**kwargs)
            else:
                return False
    	return checkrole
    return check
```

```
import Authen

@Authen(["admin"])
def index():
	print("admin 存在")

@Authen(['user'])
def index():
	print("admin 不存在")
```

这个示例不知道是否能想到我的做法呢？

利用装饰器来充当我们的拦截器，当每次路由访问的时候进行角色判断。

```
@admin_api.route('/deluser',methods=['GET','POST'])
@cross_origin()
@Authentication(["admin"])
def deleteuser():
    username = request.json.get("username")
    if request.method == 'POST':
        user = User.query_filter_by(username = username).first()
        if user.role_id == 3:
            user.deluser()
            return jsonify({"message":"删除成功"})
        else:
            return jsonify({"message":"删除失败"}),403
```

拦截器我就不举例了，因为和上面示例差不多，到时候我再出一篇Token生成，ToKen验证，角色控制结合在一起的文章，也许这样更能理解。