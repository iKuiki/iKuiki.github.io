---
layout: post
title: 在Evervim中安装wakatime插件
date: '2017-10-26 08:15:53'
tags:
- vim
- ubuntu
---

在evervim中，插件由evervim来管理，用户bundle文件为**~/.EverVim.bundles**,我们只需要将我们需要的插件**wakatime/vim-wakatime**写入这个文件即可

``` bash
echo Plug \'wakatime/vim-wakatime\' >> ~/.EverVim.bundles

# vim Plug Update
vim :PluginUpdate
```

更新：
可能是由于evervim更新，现在还需要在.Evervim.vimrc中添加如下语句才可以读取额外的bundles文件

``` vimrc
let g:override_evervim_bundles = 1
```