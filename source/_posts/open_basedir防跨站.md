---
title: open_basedir防跨站
date: 2018-11-28 17:35:00
categories: Nginx
tags:
  - nginx
---
# open_basedir防跨站
 > 多站点时担忧自己因为某个站点因为存在漏洞导致整个网站被沦陷。这里介绍服务权限层的防御（open_basedir）。
在搭建网站时请不要直接给站点目录为777权限，及设置目录归属为服务用户，不要为root用户。
## 1.跨站
学过信息安全的可能听到跨站很快会想到CSRF,XSS这两个词，但是这里主要说的是不同虚拟主机进行跨站（从A站点目录到B站点目录）。列：当获取A站点的webshell其他站点目录也可以随意进入了。  
## 2.open_basedir设置
### 2.1 fastcgi.conf
我们可以查看nginx.conf目录下有一个fastcgi.conf配置文件。查看最后一行是否有下面代码。
``` nginx
fastcgi_param PHP_ADMIN_VALUE    "open_basedir=$document_root:/tmp/:/proc/";
```

如果没有可以添加在最后一行。
在上篇文章我提到虚拟主机配置中有include fastcgi.conf。如果这些都齐全了，重启服务就可以实现了。
``` nginx
$document_root 这个是我们设置虚拟主机配置文件的站点地址
列：
    {
listen       80;    
server_name xxx.com;   //你的域名，二级域名，ip
index index.php index.html index.htm default.html default.htm default.php;   //默认寻找主页文件
root  /home/www/xxx;    $document_root
}
``` 

### 2.2 php.ini
先确认自己php版本是否是5.3.3+，如果是5.3.3+的话可以直接在php.ini后面添加语句。
``` php
[HOST=www.asite.com]    域名
open_basedir=/home/wwwroot/asite/:/tmp/     一定需要在asite后面加入/，不然可以遍历
[PATH=/home/wwwroot/asite]
open_basedir=/home/wwwroot/asite/:/tmp/
```

请在填写open_basedir地址时以/结束目录。如果你是site1,site2这样来设置自己站点目录名称将可以绕过。  
## 3.总结
open_basedir这个并不能完全可靠，可能会被绕过。可以给每个站点单独开php-fpm,这样一个站点被沦陷也无法被利用。
