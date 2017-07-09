---
title: OC的元类与父类以及isa指针关系图
date: 2017-03-18 22:23:26
tags: iOS开发
---

在Objective-C中，metaclass代表类对象（类也是一种对象，因为含有Class isa指针）的类。

![](https://github.com/JackPanda8/ImageHosting/blob/master/iOS%E7%B1%BB%E5%AF%B9%E8%B1%A1%E7%9A%84%E7%B1%BB-%E5%85%83%E7%B1%BB.png?raw=true)

>所有的元类都使用根元类（继承体系中处于顶端的类的元类）作为他们的类。这就意味着所有NSObject的子类（大多数类）的元类都会以NSObject的元类作为他们的类
>
>根据这个规则，所有的元类使用根元类作为他们的类，根元类的元类则就是它自己。也就是说基类的元类的isa指针指向他自己。
>
>引用自—[Objective-C 中的元类（meta class）是什么？](http://ios.jobbole.com/81657/)