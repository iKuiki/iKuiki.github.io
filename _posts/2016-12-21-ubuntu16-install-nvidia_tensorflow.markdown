---
layout: post
title: 在Ubuntu16下安装基于NVIDIA显卡的TensorFlow
date: '2016-12-21 04:12:00'
tags:
- ubuntu
- nvidia
- tensorflow
---

要使用gpu加速tensor flow的运算，就必须安装gpu enable版本的tensor flow，而gpu enable tensor flow只支持nvidia的gpu，所以首先你需要一台有nvidia显卡的电脑，台式笔记本都可，并在其上安装Ubuntu16 x64桌面版，在设置->软件和更新->附加驱动中启用nvidia专有驱动

接下来安装相关软件
``` bash
# 升级系统中已存在的包
sudo apt-get update && sudo apt-get dist-upgrade
# 安装python以及相关依赖
sudo apt-get install \
    python-pip python-dev \
    python-numpy python-scipy python-matplotlib ipython ipython-notebook python-pandas python-sympy python-nose \
    python-imaging -y
# 安装nvidia cuda toolkit
sudo apt-get install nvidia-cuda-toolkit
```
然后需要到nvidia官网下载cudnn，Google cudnn，然后进去后直接点下载，必须要注册，然后填一堆问卷，最后你就可以找到v4的下载项了（如果不登录，只有v3以前的下载，坑！
下载好cudnn-7.0-linux-x64-v4.0-prod.tgz后，安装余下软件
``` bash
# 安装cudnn v4
cd path/to/cudnn
tar zxf cudnn-7.0-linux-x64-v4.0-prod.tgz
sudo chown -R root:root cuda
cd cuda
sudo cp -P include/cudnn.h /usr/include
sudo cp -P lib64/libcudnn* /usr/lib/x86_64-linux-gnu/
# 安装tensor flow
sudo pip install --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.8.0-cp27-none-linux_x86_64.whl
```

接下来，还要下载VGG19网络模型:[http://www.vlfeat.org/matconvnet/models/beta16/imagenet-vgg-verydeep-19.mat](http://www.vlfeat.org/matconvnet/models/beta16/imagenet-vgg-verydeep-19.mat)
MD5:8ee3263992981a1d26e73b3ca028a123

然后clone项目[https://github.com/harry19902002/image-style-transfor.git](https://github.com/harry19902002/image-style-transfor.git)
将VGG19网络模型与待合成的图片放进项目根目录
合成命令：
``` bash
python neural_style.py --content 原始图片文件名 --styles 风格图片文件名 --out 生成图片文件名
```

附加参数可以参考python neural_style.py -h

常用参数包括
- --iterations 修改迭代次数，默认1000
- --iterations 照片权重
- --style_weight 风格图片权重
- --learning_rate 学习步长
