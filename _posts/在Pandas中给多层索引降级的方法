---
layout:     post
title:      pandas 多层索引降级
subtitle:   留存显示
date:       2018-11-27
author:     awuzhang
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - pandas
    - python
    
---


每次透视图生成后都不知道怎么处理才好，直到网上找到这个方法，赶紧记下来，原文(https://www.jb51.net/article/150975.htm)


``` python
gp3 = gp1.copy(deep=True)
gp3.columns = ["_".join(x) for x in gp3.columns.ravel()]
gp3
```
