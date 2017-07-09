---
title: ios的UINavigationController
date: 2017-02-02 23:13:41
tags: iOS开发
---

### UINavigationController的结构层次

* 一张图说明一切

  <!--more-->

![结构图](https://raw.githubusercontent.com/JackPanda8/ImageHosting/master/%E4%B8%80%E5%BC%A0%E5%9B%BE%E8%AF%B4%E6%98%8E%E6%89%80%E6%9C%89.png)

>通俗地说就是，uinavigationController是个容器，里面可以装很多uiviewController。装这么多uiviewController让用户怎么控制它们呢，总得有个工具吧。这个工具就是uinavigationBar。一个容器就这么一个bar，相当于控制台吧。但是，管理那么多uiviewController，控制台上得按钮啊、标题啊，都千篇一律是不是看起来太无聊了。为了解决这个问题，uinavigationController为每个uiviewController生成一个uinavigationBarItem，通过这个uinavigationBarItem可以改变控制台“上面”得按钮和标题。如果你不自定义uinavigationBarItem，uinavigationController会使用默认的。

​	图片和总结来源于[博客地址](http://www.cnblogs.com/ygm900/p/3659619.html)

* 源码分析

  1. UINavigationController 

     它是UIViewController的子类，它管理着一个总的UINavigationBar、一个总的UIToolBar和UIViewController的栈容器。

     ![](http://images.cnitblog.com/i/471533/201404/112138383251339.png)

     ![navigaitoncontroller](https://raw.githubusercontent.com/JackPanda8/ImageHosting/master/UINavigationController.png)

     ```objective-c
     //AppDelegate.m 初始化
     RootViewController *rootViewController = [[RootViewController alloc] init];
     UINavigationController *navi = [[UINavigationController alloc] initWithRootViewController:rootViewController];
     self.window.rootViewController = navi;

     //AViewController 入栈
     AViewController* aVC = [[AViewController alloc] init];
     [self.navigationController pushViewController:aVC animated:NO];

     //BViewController 出栈
     [self.navigationController popViewControllerAnimated:YES];
     ```

  2. UINavigationBar 

     UINavigationBar只有一个，它永远只属于UINavigationController，是所有UIViewController公用的。所以，当你修改了UINavigationBar的背景图片或者颜色时，相当于修改了所有UIViewController的NavigationBar的背景图片或颜色。

     所以如果有在Storyboard使用NavigationBar控件，一定在注意该ViewController是否是出于NavigationController的视图栈中。如果是，则要调用

     ```objective-c
     self.navigationController.navigationBarHidden = YES;
     ```
     将其隐藏掉，以显示xib或者storyboard的navigationbar控件。

     ![navigationBar](https://raw.githubusercontent.com/JackPanda8/ImageHosting/master/navigationBar.png)

  3. UINavigationItem

     UINavigationController会为每一个入栈的UIViewController生成一个UINavigationItem. UIViewController通过修改UINavigationItem可以控制UINavigationBar上的按钮和标题等。

     ![navigationItem](https://raw.githubusercontent.com/JackPanda8/ImageHosting/master/navigationItem.png)

  4. UIViewController

     每一个视图栈中的ViewController都可以访问navigationController和navigationItem属性设置其导航栏。

     ![](https://raw.githubusercontent.com/JackPanda8/ImageHosting/master/viewcontroller.png)

