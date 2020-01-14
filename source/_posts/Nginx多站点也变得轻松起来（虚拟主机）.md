---
title : Nginx多站点也变得轻松起来（虚拟主机）
date: 2018-11-27 17:28:12
categories: Nginx
tags :
  - nginx
---
# Nginx多站点也变得轻松起来（虚拟主机）
## 1.[Nginx](http://nginx.org/en/)
>Nginx是[WEB服务器](https://baike.baidu.com/item/WEB%E6%9C%8D%E5%8A%A1%E5%99%A8/8390210?fr=aladdin)，常见的还有[Apache](http://www.apache.org/).这里我就不过多去说两个的优缺点，如果不的站点计划使用Nginx希望可以对你有帮助。

## 2.环境介绍

- 系统：Linux
-  Web服务器：Nginx
-  数据库 ：[MariaDB](https://mariadb.org/)
- 脚本语言：PHP

你可以每个程序下载安装，然后配置连接好。还有一种方案就是一键安装环境。

[LNMP](https://lnmp.org/)，[OneinStack](https://oneinstack.com/)这样你无需烦恼如何去配置环境，一键帮你搞定。如果是企业级建议手动安装每一个程序。如果你不是安装lnmp环境的话，你可以选择OneinStack,它有很多组合选择。

## 3.配置
### 3.1首先查看nginx.conf文件。在最后几行你会看到
``` nginx
include  host/*.conf;
```
### 3.2如果未能找到你可以在最后面添加
``` nginx
include host/*.conf;
```
并在nginx.conf文件目录下创建host这个目录。

### 3.3创建虚拟主机配置文件
 ### xxx.conf
``` nginx
server
{
listen       80;    
server_name xxx.com;   //你的域名，二级域名，ip
index index.php index.html index.htm default.html default.htm default.php;   //默认寻找主页文件
root  /home/www/xxx;       //站点的目录地址
location ~ .*\.(php|php5)?$        
{
fastcgi_pass  unix:/tmp/php-cgi.sock;
fastcgi_index index.php;
include fastcgi.conf;     //
}
location /status {
stub_status on;
access_log   off;
}

location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
{
expires      30d;   //对于不怎么更新站点静态文件的话，可以把时间设大，让浏览器缓存降低对服务器流量带宽的压力。
}
location ~ .*\.(js|css)?$
{
expires      12h;
}

access_log off;    //这里你可以配置地址及文件名称，对网站访问日志感谢可以开启，说不定也能查问题。
#access_log logs/access.log main;   根据这个格式来修改。
}
```
在host目录下可以创建多个文件，看你自己需要什么。不管是一级域名还是二级域名，还是多个域名都可以。

后期我可能会在基础上更新一些相关网站策略，负责均衡，HTTPS文章。