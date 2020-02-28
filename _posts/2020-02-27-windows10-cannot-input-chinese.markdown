---
layout: post
title: Win10无法输入中文
date: '2020-02-27 12:05:00'
tags:
- openwrt
---

# Win10无法输入中文

在win10上我已经不止一次碰到无法输入中文的问题了，经过多番查阅，网上流传的大多是重启输入法的计划任务等操作，不堪大用。

后来发现在微软官方论坛有这个问题的反馈并且有微软工程师回复解决方案，尝试过后方案可行。方案如下（仅摘取我认为有效的部分）

在管理员权限的PowerShell或者CMD执行如下命令：

``` powershell
DISM /Online /Cleanup-image /Scanhealth
DISM /Online /Cleanup-image /Restorehealth
Sfc /scannow
```

执行完成后重启，即找到中文输入法了。

如果还不行，微软工程师还提供了一个究极解决方案：无损用户数据情况下重装系统。
通过下载如下工具[Win10 MediaCreationTool](https://www.microsoft.com/zh-cn/software-download/windows10)后无损重装一次win10，就能修复系统文件损坏导致的bug。
