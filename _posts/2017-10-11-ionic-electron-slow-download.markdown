---
layout: post
title: ionic下载缓慢解决方法(顺带说说electron)
date: '2017-10-11 16:39:03'
tags:
- ionic
- code
---

最近帮妹子搭建ionic环境，过程中需要下载不少东西，下载时因为GFW的关系，速度特别慢，经过查询，发现可以通过一下方式解决
*注：本问方法适用于Unix环境，Windows环境不保证适用*
---
### 解决nvm下载速度慢的问题

nvm下载时可以通过设置环境变量*NVMNODEJSORGMIRROR*来解决，将一下代码写入.zshrc或.bashrc中，可以将nvm下载node的源设置为淘宝源
``` bash
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/dist
```

### 解决npm下载缓慢

在npm中下载时，因为用的时国外源，所以下载速度很慢，而国内淘宝镜像了一套完整的npm源，所以将npm的源设置为淘宝源可以获得较好的下载速度。
将npm的源设置为淘宝，有两个方法：一是直接安装cnpm，但是有时候有些代码、脚本中直接调用的是npm，所以还是用方法而：直接将npm的源设置为淘宝源更方便一些。
将淘宝源设置为npm默认源，可以使用以下命令完成
``` bash
npm config set register https://registry.npm.taobao.org
```

### 解决ionic build android时下载gradle缓慢的问题

解决ionic build android时下载gradle缓慢的问题，可以直接将该文件下载好，然后设置环境变量告诉ionic去你电脑本地路径下载gradle即可。假设你下载好的gradle文件路径为*$HOME/Downloads/gradle-2.14.1-all.zip*,则在你的.zshrc或.bashrc中添加一下文本即可：
``` bash
export CORDOVA_ANDROID_GRADLE_DISTRIBUTION_URL=file://$HOME/Downloads/gradle-2.14.1-all.zip
```

### 解决electron npm install时下载缓慢的问题

最近刚好摸到一个和ionic类似的技术：electron，也是基于js做原生app的技术，不过是桌面端的app，并且也有下载缓慢的问题，所以就在这一起说了。
electron项目在npm install阶段有个node install.js的步骤，这个install.js是electron内的install.js，不受npm register配置的影响，经过查询，发现也是可以通过npm配置解决的。以下命令将electron的下载源改为国内的淘宝源
```bash
npm config set electron_mirror https://npm.taobao.org/mirrors/electron/
```