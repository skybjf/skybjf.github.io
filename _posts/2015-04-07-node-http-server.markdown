---
layout:     post
title:      "Node简易http-server"
subtitle:   "基于node.js搭建本地简易http-server"
date:       2015-04-07
author:     "小久哥"
header-img: "http://7xp21g.com1.z0.glb.clouddn.com/img%2Farticle%2Ftpbg%2Fhttp-server.jpg"
tags:
    - node
---

> 应用node.js的插件，搭建一个简易的http-server

***

简单来说就是基于node.js的插件“http-server”来快速搭建一个简易的本地服务器。因为要用真机测试移动端代码，没有上线的测试，只好搭建一个临时的本地服务器，手机通过ip地址访问我电脑里的文件来测试移动端。

至于搭建本地服务器的方法有很多，python似乎会快些
```
python -m SimpleHTTPServer 8000
```
写进一个bat文件，放在根目录下，双击运行就可以了(前提是安装了python，印象中2版和3版启动httpserver的指令不一样)。不足的地方就是每次都要把这个文件移动对应的目录下启动才可以，相比组合使用sublime的插件直接开启node的http-server不太方便了。


下面开始讲解步骤~~

一、安装node.js [node的官网](https://nodejs.org/){:target="_blank"} ,下载安装对应的安装包就可以了。
安装完成之后，检查一下是否安装成功。在cmd里输入 node -v，会出现一个版本号算是成功。

![img](/img/article/insert/20150407/1.png)

二、npm install http-server 安装一个叫“http-server”的插件，等待它安装完成就可以了。

三、启动，cmd找到对应的根目录，输入“http-server”，然后就可以使用了。

![img](/img/article/insert/20150407/2.png)


