---
title: ios的Frame与Bounds
date: 2017-02-24 20:30:50
tags: iOS开发
---

### 理解ios view的frame与bounds

而者都是CGRect的，都是含有origin 和 size两个结构体属性

不同之处：frame是相对于父View而言的，而bounds则相对于自己的坐标系，所以bounds的origin一定是（0，0）。

![](https://github.com/JackPanda8/ImageHosting/blob/master/frame&bounds.jpg?raw=true)

