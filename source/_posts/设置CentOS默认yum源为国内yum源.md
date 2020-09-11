---
title: 设置CentOS默认yum源为国内源
tags:
  - CentOS
  - yum
categories:
  - 技术分享
  - 操作系统
  - CentOS
keywords: "CentOS,yum源,国内源"
description: 使用config文件管理ssh配置，实现多个密钥管理。
top_img: 'https://s1.ax1x.com/2020/09/11/wUAaNt.jpg'
cover: 'https://s1.ax1x.com/2020/09/11/wUAUAI.jpg'
abbrlink: 881d08f0
date: 2020-09-11 10:08:24
---

## 前言  
`CentOS` 默认的 `yum` 源不是国内源，导致 `yum` 在安装某些软件的时候速度不是很理想，我们可以选择将默认源设置为国内源。
国内可用的镜像站点是网易和阿里镜像，我们可以先 `ping` 一下这两个站点，选择速度较快的一个进行设置。
```shell
[root@localhost hexo]# ping 163.com
PING 163.com (123.58.180.8) 56(84) bytes of data.
64 bytes from 123.58.180.8 (123.58.180.8): icmp_seq=1 ttl=54 time=40.8 ms
64 bytes from 123.58.180.8 (123.58.180.8): icmp_seq=2 ttl=54 time=40.9 ms
64 bytes from 123.58.180.8 (123.58.180.8): icmp_seq=3 ttl=54 time=40.7 ms
64 bytes from 123.58.180.8 (123.58.180.8): icmp_seq=4 ttl=54 time=40.7 ms
```


```shell
[root@localhost hexo]# ping aliyun.com
PING aliyun.com (106.11.248.144) 56(84) bytes of data.
64 bytes from 106.11.248.144 (106.11.248.144): icmp_seq=1 ttl=37 time=30.6 ms
64 bytes from 106.11.248.144 (106.11.248.144): icmp_seq=2 ttl=37 time=30.6 ms
64 bytes from 106.11.248.144 (106.11.248.144): icmp_seq=3 ttl=37 time=30.5 ms
64 bytes from 106.11.248.144 (106.11.248.144): icmp_seq=4 ttl=37 time=30.5 ms
```
我这里阿里源的速度更快，所以以设置阿里源为例。

## 设置步骤  
* 备份默认 `yum` 源  
```shell
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
```
* 根据 `CentOS` 的版本下载相应的阿里云的镜像源文件，注意此时如果没有安装 `wget` 的话，需要先安装（`yum -y install wget`）。 
  * CentOS5  
```shell
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-5.repo
```
  * CentOS6  
```shell
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
```
  * CentOS7  
```shell
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```
* 清空 `yum` 缓存并生成 `cache` 文件
```shell
yum clean all
yum makecache
```
* 更新系统  
```shell
yum -y update
```
至此 `CentOS` 的默认 `yum` 源就更换为了阿里源。