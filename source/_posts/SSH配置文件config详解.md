---
title: 使用config文件管理ssh配置
tags: ssh
categories: ssh
keywords: 'ssh,config,配置,国内源'
description: 使用config文件管理ssh配置，实现多个密钥管理。
top_img: 'https://s1.ax1x.com/2020/09/07/wKGGnA.png'
cover: 'https://s1.ax1x.com/2020/09/07/wKGJ0I.png'
abbrlink: 1c5df34e
date: 2020-09-07 10:08:54
---
[1. 前言](#1)
[2. 生成密钥](#2)
[3. 通过案例配置 `config`](#3)

<span id="1">

## 前言  
本文假设读者已经了解 `ssh` 的基本原理并掌握其基本用法；所以重点说明如何用 `config` 文件来管理 `ssh` 的用户配置。  

当我们使用 `ssh` 登录其他主机时，可能会有以下几种需求：  
1. 我们会通过 `SSH` 从自己的 `github` 仓库上拉取某个项目的代码，或者提交代码更新；
2. 我们有自己的服务器，需要通过 `SSH` 远程登录或者免密登录；
3. 在公司的服务器上进行远程登录访问，或者在公司搭建的私有仓库上拉取或者提交代码；
4. ...等等  
   
总而言之，当我们想对多个不同的主机使用不同的密钥，或者对同一个主机使用不同的密钥，再或者对不同的主机使用同一个密钥。我们都可以使用 `config` 文件进行配置。我们 `ssh` 可以用命令行进行选项控制，也可以使用 `/etc/ssh/sshd_config` 文件进行 `ssh` 的系统配置，而 `config` 文件则是用来对 `ssh` 进行用户相应的配置。下面我们来看一下如何使用 `config` 文件进行配置。在此之前，先熟悉一下如何生成密钥。  
</span>

<span id="2">

## 生成密钥  
* 打开终端，进入 `~/.ssh/` 目录下，执行以下命令：
```shell
ssh-keygen -t rsa -C "hitwhy@gmail.com"
```
`-t` 参数指定生成密钥的类型（算法），也即 `type`，这里是 `rsa`。
`-C` 参数指定密钥注释，一般写邮箱即可。
* 运行命令后，出现以下提示：  
```shell
[root@localhost .ssh]# ssh-keygen -t rsa -C "hitwhy@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
```
* 输入文件名称，建议输入一个含义明确的名字。这里假设有一台主机名为 `host` 的主机，故输入 `id_rsa_host`，回车提交：
```shell
[root@localhost .ssh]# ssh-keygen -t rsa -C "hitwhy@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): id_rsa_host
Enter passphrase (empty for no passphrase):
```
* 接下来提示输入密码，这里建议直接回车，不设置密码。当然也可以选择设置，只是设置密码后每次输入都要填写密码，徒增麻烦。这里直接按两次回车：
```shell
[root@localhost .ssh]# ssh-keygen -t rsa -C "hitwhy@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): id_rsa_host
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa_host.
Your public key has been saved in id_rsa_host.pub.
The key fingerprint is:
SHA256:vkEK4/zLJ29JJLmMHunC8VlvqiL8EUJl85kkkjPhTzw hitwhy@gmail.com
The key's randomart image is:
+---[RSA 2048]----+
| oo= .           |
|.+= = o          |
| ooE + .         |
|. o . o .        |
| . oo+ +S        |
|  oo=o++.        |
|.. *o+.oo.       |
|..+ *o. *o       |
| ..+..=Oo        |
+----[SHA256]-----+
```
* 此时密钥就生成了。在 `~/.ssh` 目录下会生成两个文件，`id_rsa_host` 和 `id_rsa_host.pub`，即私钥和公钥。  
生成密钥之后就可以进行相关的配置来登录其他主机。
一般的做法是，将我们刚才生成的公钥添加到目标主机相应用户的 `.ssh` 目录下的 `authorized_keys` 文件中，这里注意该文件的访问权限应当设置为 `600`。当我们访问该主机时，我们的数据会用本机的私钥(`id_rsa_host`)进行加密，目标主机接收后再用 `authorized_keys` 文件中相应的公钥进行解密，从而实现数据的加密传输；反之亦然。可以使用命令 `ssh-copy-id -i ~/.ssh/私钥名称 远程用户名@远程主机地址` 将公钥添加到目标主机。
如果不将公钥添加到 `authorized_keys` 文件中，那么每次远程登录的时候都需要输入密码确认，所以也就不是免密登录了。
</span>

<span id="3">

## 通过案例配置 `config`  
首先在本机的 `~/.ssh` 目录下创建 `config` 文件。下面先说一下该文件中用的几个参数及其含义：

| 参数           | 含义                             |
| -------------- | -------------------------------- |
| `Host`         | 关键字，别名，下面案例中详细解释 |
| `HostName`     | 目标主机 `IP` 地址或域名         |
| `Port`         | `ssh` 连接端口                   |
| `User`         | 登录该主机的用户                 |
| `IdentityFile` | 登录该主机使用的密钥文件         |

下面看两个例子，假设主机 `B` 的地址为 `www.B.com`，主机 `C` 的地址为 `www.C.com`。
1. 本机 `A` 使用不同密钥访问主机 `B` 和主机 `C`。  
在主机 `A` 的 `~/.ssh` 目录下生成两对密钥，假设分别为 `id_rsa_B`、`id_rsa_B.pub` 和 `id_rsa_C`、`id_rsa_C.pub`，分别对应于登录主机 `B` 和主机 `C` 的密钥文件。本机 `config` 文件的内容如下： 
```shell
Host B
     HostName www.B.com
     Port 22
     User B
     IdentityFile ~/.ssh/id_rsa_B

Host C
     HostName www.C.com
     Port 6789
     User C
     IdentityFile ~/.ssh/id_rsa_C
```
当我们登录主机 `B` 的时候，在终端使用以下命令：
```shell
ssh B
```
然后输入密码即可，如果将公钥 `id_rsa_B.pub` 存储到目标主机的话就可以实现免密登录。这里 `B` 就是主机 `www.B.com` 的对应的 `ssh` 配置的别名，`ssh B` 在这里等同于 `ssh -p 22 -i ~/.ssh/id_rsa_B B@www.B.com`。使用 `config` 配置一次，以后访问就可以直接使用 `ssh B` 登录，是不是很简洁呢？由于 `ssh` 默认使用的就是 `22` 端口，所以上面 `Port 22` 这一行可以省略。但是，如果像主机 `C` 一样，将 `ssh` 的默认端口改变的话（这里是`6789`），那就不能省略了。此外，如果本机用户名与远程登录的用户名相同的话，那么 `User B` 这一行也是可以省略的。最后一点，就是我们也可以将每一行中间的空格换成 `=`，例如 `Port=22`，效果是一样的。
2. 本机 `A` 使用相同密钥访问主机 `B` 和主机 `C`。  
同样，我们还是以上面的两对密钥 `id_rsa_B`、`id_rsa_B.pub` 和 `id_rsa_C`、`id_rsa_C.pub` 为例。本机 `config` 文件内容如下： 
```shell
Host github.com gitee.com
     HostName %h
     Port 22
     User A
     IdentityFile ~/.ssh/id_rsa_git
```
`%h` 代表远程主机名，上面的例子中也就是代表 `Host` 中的两个主机名。当我们访问 `github` 时，`ssh` 就会匹配上面的配置，`%h` 就会匹配 `github.com`，使用上面的信息进行访问。
配置完成之后我们可以测试一下是否成功：
```shell
ssh -T git@github.com
```

</span>   