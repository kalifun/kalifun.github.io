---
title : hexo加入评论功能(来必力)
date: 2018-11-28 17:28:12
categories: 站点
tags :
  - hexo
  - 来必力
---
# hexo加入评论功能(来必力)
![laibili](http://image.kalifun.top/upload/1811/e1473e15e5c7d305.png)
## 1.登录注册
 [来必力](https://www.livere.com/)，如何登录注册我就不冗余了，随便搞个邮箱什么的就可以了。  
## 2.来必力配置
 来到代码管理选择一般网站里面有一个data-uid
 再来到你主题目录下有一个_config.yml，进行搜索查找关键词livere_uid
```
livere_uid : xxxxxxxxxxxxx
```
### 然后提交代码到github就实现功能了。
```
hexo clean && hexo g && hexo d
```
 上面是主题支持来必力功能的。
如果不支持，那百度找找吧，我也没测试过，就不给教程了。
还有这个是一个韩国人的东西，如果抵制的话，那就弃坑选择其他的评论插件吧。
