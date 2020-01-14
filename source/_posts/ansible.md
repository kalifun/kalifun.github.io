---
title : 安装ansible
date: 2018-09-10
categories: 运维
tags:
  - ansible
---
# ANSIBLE
## centos 安装 Minimal版本无法获取IP解决
``` bash
vi /etc/sysconfig/network-scripts/ifcfg-ens33
ONBOOT=yes  (修改内容)
sudo reboot
```
## CENTOS 安装 ansible
``` bash
yum update
yum install ansible 
```
## Ubuntu 安装 ansible
``` bash
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get  update
sudo apt-get install ansible
将会报错说无法安装
```
## Ubuntu 安装 ansible报错解决
``` bash
添加源
sudo vi /etc/apt/sources.list
```
```  java
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```
## ansible 配置
###  配置linux主机ssh无密码访问
一,生成密钥
``` bash
ssh-keygen
cd   .ssh
id_rsa 为私钥  id-rsa.pub为公钥
```
二,下发公钥
``` bash
ssh-copy-id     [-i [identity_file]]    [-p port]    [user]@hostname
```
