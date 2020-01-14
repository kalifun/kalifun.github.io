---
title : hexo-asset-image的苦恼
date: 2018-08-20 17:28:12
categories: 站点
tags:
  - hexo
---
# hexo-asset-image的苦恼
博客搞定很激动，写博客加上图片就更奈斯了对不对。  
可是很蛋疼的是，图片居然解析错了。我也很绝望啊！

## 心路历程
- 图片放到一个文件夹里面  
- 写完md后自己解析图片路径

## 百度结果
- 使用工具 hexo-asset-image

## hexo-asset-image
- 修改_config.yml (post_asset_folder:true)  
- 当我们执行 hexo n "title”  
- 在source/_posts下面会生成一个xxx.md和一个xxx文件夹
- xxx文件夹是我们md需要引入的图片目录

## 执行过程
``` bash
npm install hexo-asset-image --save (目前版本0.0.3)
vim _config.yml
hexo n "hexo-asset-image的苦恼"
```
举例：hexo-asset-image的苦恼/1.jpg  
我们markdown的时候需要引入图片1.jpg语法如下
``` bash
![图片1](1.jpg)
```
没看出就是这么简单，接下来我们生成静态文件了。
``` bash
hexo g
```
在public/year(2018)/month/day/hexo-asset-image的苦恼/  
这个目录下会有一个index.html,及1.jpg
我们查看一下index.html。
``` bash
<img src="/day/hexo-asset-image的苦恼/1.jpg">”
```
这个怎么解析？  
不是应该这么解析嘛？
``` bash
<img src="/year/month/day/hexo-asset-image的苦恼/1.jpg">
```
各种百度，兄弟怎么就没有一个给我解释的呢。  
去愁一愁代码吧，在node_modules下面有一个hexo-asset-image  
愁一愁index.js 纳尼，为毛是js。反正我是看不懂。
``` bash
if(config.post_asset_folder){
    var link = data.permalink;
        var beginPos = getPosition(link, '/', 3) + 1;
        // In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
        var endPos = link.lastIndexOf('/') + 1;
    link = link.substring(beginPos, endPos);
```
我看不懂，我想大概意思应该是这样吧~  
Permalink应该指的是_config.yml的吧。
``` bash
permalink: :year/:month/:day/:title/
```
我猜应该是这样的。可是我好奇你为啥是('/',3)
``` bash
var beginPos = getPosition(link, '/', 3) + 1;
```
我获得的路径是/day/hexo-asset-image的苦恼/1.jpg  
这个是从day开始分析的。  
难道是这样year(第一个/)month(第二个/)day(第三个/)  
所有就是从第三个/解析拼接的嘛。那我们修改成这样试一试。
``` bash
var beginPos = getPosition(link, '/', 1) + 1;
```
``` bash
hexo g
```
再去愁一愁index.html的结果是啥样子。  
``` bash
<img src="/year/month/day/hexo-asset-image的苦恼/1.jpg">
```
soga  原来是这样子啊。

# 使用hexo-asset-image总结
## 1.安装hexo-asset-image
``` bash
npm install hexo-asset-image --save
```
## 2.修改index.js
``` bash
路径 /node_moduls/hexo-asset-image/index.js
修改配置  var beginPos = getPosition(link, '/', 1) + 1;
```

## 3.生成静态文件
``` bash
hexo g
```
## 4.查看网页是否正常解析
``` bash
hexo s --debug
```
## 5.推到GitHub
``` bash
hexo ds
```
