---
title: 使用Java客户端连接IMM系统时报错的解决方案
tags:
  - 解决方案
  - BMC
  - IMM
categories:
  - 技术分享
  - 解决方案
keywords: >-
  Java客户端,IMM,BMC,Video Viewer Control,remote control,Unable to launch the
  application,Server returned HTTP code: 405 for URL
description: >-
  使用Java客户端远程连接BMC时报错， Unable to launch the application，具体信息是Server returned
  HTTP code: 405 for URL，通常是第二次连接同一个服务器时发生。本文记录其解决方案。
top_img: 'https://s1.ax1x.com/2020/09/18/whCOaR.jpg'
cover: 'https://s1.ax1x.com/2020/09/18/w4z1mj.png'
abbrlink: 32ee7bef
date: 2020-09-17 06:23:16
---
[1. 问题描述](#1)  
[2. 原因分析](#2)  
[3. 解决方案](#3)  

<span id="1">

## 问题描述  
*注意，本文只针对 `IBM` 的 `IMM` 或 `IMM2` 系统，因为其他系统没有尝试过，理论上大同小异。*  

**对 `BMC` 不太熟悉的朋友可以参考知乎的[这个链接](https://www.zhihu.com/question/54716507)。简单来说， `BMC` 系统是独立于服务器操作系统的一个小型操作系统，提供了一种管理服务器的方式。**  

远程连接 `BMC` 系统时，`IMM` 系统提供了三种方式，如下图所示：  
![](https://s1.ax1x.com/2020/09/17/wW1Ivd.png)  
当使用 `Java Client` 方式连接时可能会报如下错误：  
![](https://s1.ax1x.com/2020/09/18/w4WPsg.png)  
> `Unable to launch the application`  

点击图中的 `Details`，显示如下图：  
![](https://s1.ax1x.com/2020/09/18/w4WKQU.png)  
> `java.io.IOException: server Returned HTTP response code: 405 for URL: https://XXX/443/designs/imm/aessrp/avctIBMViewer_V082817.jar`  

下面我们先做一下简要分析。

</span>

<span id="2">

## 简要分析  
其实，这里有点托大。本人并不是很确定该错误出现的具体原因。但通过报错产生的上下文推断，可以大致有个猜想：该报错绝大多数都是在连接同一台服务器时产生，并且是在已经连接过之后才会产生。所以推断，产生报错的原因是，当第二次连接的时候会使用某些第一次连接时产生的数据，但是连接时校验产生错误，所以不匹配而导致无法启动。  

*以上的推论实际上可以无视，大家只要使用下面的方式能够解决问题就可以了。*

</span>

<span id="3">

## 解决方案  
实际上以上报错只要清理一下 `Java` 控制面板的缓存及安装的文件和应用即可，具体方法如下：  
1. 按下 `Windows` 的 `Win` 键，输入 `control` 打开控制面板。  
2. 选择 `程序（program）`。  
![](https://s1.ax1x.com/2020/09/17/wW1TKA.png)  
3. 点击 `Java`，出现以下面板。  
![](https://s1.ax1x.com/2020/09/17/wW15gH.png)  
4. 点击 `settings`。  
![](https://s1.ax1x.com/2020/09/17/wW1RUK.png)  
5. 点击 `Delete Files...`  
![](https://s1.ax1x.com/2020/09/17/wW12E6.png)  
6. 选择图中两项，第三项可选可不选，点击 `OK` 即可。  

此时，再次使用 `Java Client` 方式连接即可成功。

</span>

