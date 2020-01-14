---
title: Nginx配置HTTPS 
date: 2018-10-29 17:35:00
categories: Nginx
tags:
  - nginx
  - https
---
# Nginx配置HTTPS 
## 前言
我们站点搭建完协议都是走的HTTP1.1,不排除更低的1.0。
![http](http://image.kalifun.top/upload/1811/a739968397a59def.png)
如果我们没有开启HTTPS你可以看到我们的站点会显示不安全。
![nginx](http://image.kalifun.top/upload/1811/aa4a33294093a978.png)
![Security](http://image.kalifun.top/upload/1811/1dd7aa404bb92778.png)  
## HTTPS
> HTTPS其实是有两部分组成：HTTP + SSL / TLS，也就是在HTTP上又加了一层处理加密信息的模块。今年出了TLS1.3了。其中还有TLS1.2，感觉更不上世界的步伐啊。
### 1.获取证书SSL
>**acme.sh**实现了`acme`协议, 可以从Let’s Encrypt 生成免费的证书.

#### 1.1 安装acme.sh
``` bash
curl https://get.acme.sh | sh
```
 脚本将会自动执行，当你看到Install success!说明你安装成功。安装成功请把你的服务器的终端关闭。不然你输入相关命令是不生效的哦。  
#### 1.2生成证书
**acme.sh**实现了**acme**协议支持的所有验证协议. 一般有两种方式验证: http 和 dns 验证.
 这里就提及http验证，如果感兴趣你可以去官网查相关资料。
http 方式需要在你的网站根目录下放置一个文件, 来验证你的域名所有权,完成验证. 然后就可以生成证书了.
``` bash
    acme.sh  --issue  -d mydomain.com -d www.mydomain.com  --webroot  /home/wwwroot/mydomain.com/
```
如果你用的**apache**服务器,**acme.sh**还可以智能的从**apache**的配置中自动完成验证, 你不需要指定网站根目录:
```
    acme.sh --issue  -d mydomain.com   --apache
```
如果你用的**nginx**服务器, 或者反代,**acme.sh**还可以智能的从**nginx**的配置中自动完成验证, 你不需要指定网站根目录:
```
    acme.sh --issue  -d mydomain.com   --nginx
```
#### 1.3 copy/安装 证书
  ``` sh
        acme.sh  --installcert  -d  <domain>.com   \
            --key-file   /etc/nginx/ssl/<domain>.key \
            --fullchain-file /etc/nginx/ssl/fullchain.cer \
            --reloadcmd  " nginx -s reload"
  ```
 修正一下，请使用nginx -s reload.如果安装官方会报没有这个命令，除非你去init.d/nginx进行修改才可以。  
  ### nginx配置
  http默认是监听80端口，而https监听的是443端口。
  > domain.com.conf
  ``` nginx
  server {
    listen 443 ssl http2;
    server_name domain.com;
    index index.php index.html index.htm default.html default.htm default.php;
    if ($host != domain.com) {
        return 301 https://domain.com$request_uri;
    }
    root  /home/www/domain;

    ssl_certificate     /etc/nginx/ssl/fullchain.cer;
    ssl_certificate_key   /etc/nginx/ssl/domain.top.key;

    ssl_protocols TLSv1.2;
    ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';
    ssl_prefer_server_ciphers on;

    ssl_session_cache shared:SSL:50m;
    ssl_session_timeout 1d;
    ssl_session_tickets on;

    ssl_stapling on;
    ssl_stapling_verify on;

    resolver 119.29.29.29 8.8.8.8 valid=300s;
    resolver_timeout 10s;

    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload";

    location ~ .*\.(php|php5)?$
	{
	fastcgi_pass  unix:/tmp/php-cgi.sock;
	fastcgi_index index.php;
	include fastcgi.conf;
	}
    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|ico)$ {
      expires 30d;
    }
    location ~ .*\.(js|css)?$ {
      expires 15d;
    }
}
  ```
如果你的也是WordPress站点，那你等等吧。你网站绝对无法正常开启https。你会发现还是会报（你的连接不完全安全）。
 点击F12，查看Console。它的意思是你的网站已经是https了，但是有些连接还是http地址。好不容易搞定，我又要跑去后台，把以前是http的地址改成https。
还有一种偷懒的方法，那就是去数据库进行update。找到你站点的数据库，来到wp_postsde表。执行sql语句。
  ``` mysql
  1.  update wp_posts set post_content = replace(post_content,'http://','https://');
  ```
当你改完之后你刷新页面就可以看到效果了。
  ![https](https://image.kalifun.top/upload/1811/fa267fcc3336370a.png)
 看到这个是不是舒畅了，总比之前报红好。
  ![sll](https://image.kalifun.top/upload/1811/2c5d5dcd80ffbf5f.png)
最好推荐一个ssl的跑分网站[SSL Server Test](https://www.ssllabs.com/ssltest/index.html)
  ![SSL Server Test](https://image.kalifun.top/upload/1811/2b95e408e7fa59e1.png)
  