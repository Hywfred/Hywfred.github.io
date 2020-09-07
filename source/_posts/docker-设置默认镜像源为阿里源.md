---
title: docker设置默认镜像源为阿里源
tags: docker
categories: docker
keywords: "docker,镜像源,阿里源,国内源"
description: 将docker默认地址镜像源设置为阿里云加速地址镜像源
top_img: 'https://s1.ax1x.com/2020/09/06/wmBtpV.jpg'
cover: 'https://s1.ax1x.com/2020/09/06/wmBCOe.png'
abbrlink: fc4278eb
date: 2020-09-06 10:04:28
---

`docker` 默认的镜像源在国外，所以我们安装完 `docker` 拉取镜像的时候速度会非常慢，而且经常拉取失败。这里介绍一下将 `docker` 默认镜像地址设置为阿里的镜像加速源的方法。如果想设置为国内其他镜像源的地址，也可参照方法进行相应地址的更改即可。  
几个比较常用的国内镜像源地址如下：
1. `docker` 官方中国区：`https://registry.docker-cn.com`
2. 网易：`http://hub-mirror.c.163.com`
3. `USTC`：`http://docker.mirrors.ustc.edu.cn`

下面介绍更改为阿里源的方法。  
## 获取阿里镜像源的地址  
* 登录[阿里云](https://cn.aliyun.com/)(`https://cn.aliyun.com/`)  
  ![](https://s1.ax1x.com/2020/09/01/dxLLFI.jpg)  
* 点击进入控制台  
  ![](https://s1.ax1x.com/2020/09/01/dxLoOe.jpg)  
* 点击左上角的图标，选择`产品与服务`，然后在搜索框内输入关键字`容器镜像服务`，出现搜索结果后选择`容器镜像服务`  
  ![](https://s1.ax1x.com/2020/09/01/dxLbTA.jpg)  
* 选择最下面的镜像加速器  
  ![](https://s1.ax1x.com/2020/09/01/dxLHwd.jpg)  
* 此时出现的部分就是如何将 `docker` 的默认镜像地址修改为阿里云加速地址的方法。按照红框内的方式操作即可（参考第二部分内容）。  
  ![](https://s1.ax1x.com/2020/09/01/dxL7eH.jpg)  



## 将 `docker` 默认地址更改为阿里镜像源  

* 首先确定 `/etc/docker` 目录存在，如果不存在则创建  
```shell
sudo mkdir -p /etc/docker   
```
* 在 `/etc/docker` 目录下增加文件 `daemon.json`，并将其内容改为加速地址  
```shell
sudo tee /etc/docker/daemon.json <<-'EOF'
{
"registry-mirrors": ["https://4wpmqmc8.mirror.aliyuncs.com"]
}
EOF
```
* 重载配置使之生效  
```shell
sudo systemctl daemon-reload
```
* 重启 `docker` 加载配置  
```shell
sudo systemctl restart docker
```

如果登录的是 `root` 用户，则将命令的 `sudo` 去掉执行。

以上就是将 `docker` 默认镜像地址修改为阿里云加速地址的方法。修改完成之后可以测试一下拉取速度是不是比之前快多了？
``` shell
docker pull ubuntu
```
