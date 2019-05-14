---
layout: post
title: Golang http包req.Body只能读取一次的解决办法
date: '2017-09-13 03:40:39'
tags:
- golang
- code
---

今天参加实验楼的golang比赛，碰到个大坑，比赛要求把http请求的body取出后计算md5然后放入请求头中，然后我用一下代码读取body后，body就无法被再次读取了！无法，被，再次，读取！有毒！
``` bash
body, err := ioutil.ReadAll(req.Body)
```

经过20分钟调试，发现应该用如下方式来获取body，才不会导致body被抹掉：
``` bash
br, err := req.GetBody()
if err == nil {
	body, err := ioutil.ReadAll(br)
}
```

*事实上，从golang1.9开始，第一种代码也不会导致req.Body被抹掉了*