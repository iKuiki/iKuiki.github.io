---
layout: post
title: git优雅删除子模块
date: '2017-02-17 04:30:00'
tags:
- git
---

在git中用git submodule add添加子模块后，并不容易完整的删干净，经过摸索，通过以下命令删除子模块比较优雅与干净：

``` bash
# 逆初始化模块，其中{MOD_NAME}为模块目录，执行后可发现模块目录被清空
git submodule deinit {MOD_NAME}
# 删除.gitmodules中记录的模块信息（--cached选项清除.git/modules中的缓存）
git rm --cached {MOD_NAME}
# 此时.gitmodules中的记录未修改，需要手动删除其中相关的行
# 提交更改到代码库，可观察到'.gitmodules'内容发生变更
git commit -am "Remove a submodule."
```
