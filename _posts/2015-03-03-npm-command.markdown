---
layout:     post
title:      "npm指令"
subtitle:   "node初期常用的一些npm指令"
date:       2015-03-03
author:     "小久哥"
header-img: "http://7xp21g.com1.z0.glb.clouddn.com/img%2Farticle%2Ftpbg%2Fnpm.jpg"
tags:
    - node
---

> 2016年6月19日第一次更新

***

### npm指令

* 初始化(会生成package.json)：`npm init`
* 安装模块：`npm install xxx`
* 安装模块(安装不上线的代码依赖，比如webpack)：`npm install xxx --save-dev`
* 安装模块(安装需要上线代码依赖，比如jq)：`npm install xxx --save`
* 安装模块(安装指定版本号)：`npm install xxx @版本号`
* 安装模块到全局：`npm install xxx -g`
* 安装`--save`的模块：`npm install xxx --production`
* 查看已安装的模块：`npm ls`
* 查看已安装到全局的模块：`npm ls -g`
* 卸载模块：`npm uninstall xxx (-g)`
* 清理缓存：`npm cache clean`
* npm官网注册：`npm adduser`
* npm官网登录：`npm login`
* 查看是否已经登录：`npm whoami`

***

#### 额外说明：

1. 本地安装和全局安装，两种安装方法安装的位置不一样，如果想方便一些的话可以都采用全局安装，没有什么问题
2. 虽然不是处女座……还是不喜欢有cache的存在，建议隔一段时间清理一下cache
3. `npm login|adduser|publish|...` 的源必须是 `https://registry.npmjs.org`

#### 更多有关npm的信息，请参考:

* [npmjs.com](http://npmjs.com/){:target="_blank"} (各种可以安装的插件都在里面)
* [npm官方文档](https://docs.npmjs.com/){:target="_blank"} 