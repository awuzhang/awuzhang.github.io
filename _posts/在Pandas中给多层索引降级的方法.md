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

方法一：
``` python
gp3 = gp1.copy(deep=True)
gp3.columns = ["_".join(x) for x in gp3.columns.ravel()]   
gp3
```
方法一有个问题，如果column是数字  "_".join(x) 会报错

方法二：
``` python
ptable = df.pivot_table(...)
ptable.columns = ['_'.join(tuple(map(str, t))) for t in ptable.columns.values]
ptable = ptable.reset_index()
```
