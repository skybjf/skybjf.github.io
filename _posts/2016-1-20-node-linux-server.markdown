---
layout:     post
title:      "Windows中虚拟机配置linux"
subtitle:   "node开发环境配置"
date:       2015-10-16
author:     "小久哥"
header-img: "http://7xp21g.com1.z0.glb.clouddn.com/img%2Farticle%2Ftpbg%2Fnode1.jpg"
tags:
    - node
---

> windows系统中安装虚拟机，然后在虚拟机中配置与node相关的环境。

***

### 步骤
* 安装虚拟机，推荐oracle的
* linux系统，我使用的最新的centos7
* 在linux中安装相应的软件 epel-release,nodejs 

### 详细教程
现在是凭借记忆开始写了，有些遗漏的地方，请留言吧。

一、 安装虚拟机，关于虚拟机的选择，到底是vmware还是oracle的，我建议是oracle的。为什么？因为我在尝试的时候vm的虚拟机没有识别出来我下载的centos7系统镜像，具体什么原因我不知道，为了省时间我安装了oracle的。

二、 安装centos系统，有个基础知识说一下，你虚拟机系统位数要参照你的物理机。换句话说，就是你不可能在32位windows系统的虚拟机里安装64位linux系统；关于系统版本选择，我知道centos是做服务器最合适的系统，到底是用centos6，还是7，看你个人爱好，需要注意的是不同版本之间可能会存在有些指令不一样的情况。(我选的7，本文都以centos7为例子)

三、 安装的时候，注意以下几个地方：选择英文版的； `SOFTWARE SELECTION` 中选择 左边 `Basic Web Server` 右边 `Development Tools` ； `INSTALLATION DESTINATION` 中尽管已经选择了安装的磁盘，点掉重新勾选；注意设置root的密码；安装好了之后重启就可以了。重启之后首先要设置网卡为默认打开 ` vi /etc/sysconfig/network-scripts/ifcfg-enp0s3` 最后一项设置为yes；然后重启虚拟机网络 `systemctl restart network` 就可以联网了

四、 通过xshell链接虚拟机，虚拟的网络设置中选择 “网路地址转换NAT” 下面的 高级 中 端口转发，主机端口与子系统端口配置一下，以22为例的话可以都配制成22端口。在xshell中的配置，主机就是 `127.0.0.1` 端口 `22` 用户名是 `root` 密码是刚刚你在安装系统时候设置的管理员密码

五、 `yum install epel-release` `yum install nodejs` `yum install mongodb-server` `yum install mongodb` `yum install redis` 其他的根据个人爱好安装就可以了

### 补充 关于如何把文件同步到你的虚拟机服务器中
我使用的是sublime，所以安装了sftp的插件，然后和设置xshell差不多，记得设置保存上传就行。或者你下载一个ftp客户端软件，每次修改完之后手动同步到虚拟机中。
