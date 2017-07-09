---
title: ios不同屏幕分辨率下的设计规范
date: 2017-02-01 18:14:24
tags: iOS开发
---

1. iOS自诞生以来的各种尺寸屏幕的分辨率一览

   <!--more -->

   ![屏幕分辨率一览](https://raw.githubusercontent.com/JackPanda8/ImageHosting/master/iphone%E5%88%86%E8%BE%A8%E7%8E%87%E7%BB%88%E6%9E%81%E6%8C%87%E5%8D%97.jpg)
   ​        

   ​	可以按照ppi将所有屏幕分辨率分成@1x-163ppi，@2x—326ppi和@3x—401ppi一共三种。

   ​        iOS开发的基本单位是pt(point)，在非Retina屏幕下，@1x的屏幕对应的1pt=1px，@2x和@3x下的一个pt则对应2px与3px。现在大多都是Retina屏幕，所以，**@1x的屏幕对应的1pt=2px，@2x和@3x下的一个pt则对应4px与6px。**

   ---

2. 在使用xcode的storyboard进行UI绘制时，所有的数字单位是pt，而且是@1xppi比例下的1pt=2px，尽管像下图所示的storyboard可以选择以iPhone5se，iPhone6，iPhone7plus等各种不同的设备去查看app的显示效果，但是实际设计时，仍旧按照@1x下进行，在app运行时autolayout能自动进行绘制布局。

      ​

      ![Xcode可以选择不同的设备查看显示效果](https://raw.githubusercontent.com/JackPanda8/ImageHosting/master/xcode-storyboard2017%E5%B9%B4%E6%B5%81%E8%A1%8C%E5%B1%8F%E5%B9%95%E5%B0%BA%E5%AF%B8%E4%B8%8B%E7%9A%84%E6%95%88%E6%9E%9C%E5%9B%BE.png)

      ​

      所以iOS程序员在按照设计图进行设计时候，应当和美工沟通清楚该设计图是基于多少ppi的，这样才能确定1pt等于几个px（一点有几个像素），因为只说分辨率不讲ppi的手机厂商都是耍流氓。

      ​	下图是iOS不同分辨率下的导航栏等的设计规范。

      ![iOS不同分辨率下的导航栏等的设计规范](https://raw.githubusercontent.com/JackPanda8/ImageHosting/master/ios%E4%B8%8D%E5%90%8C%E5%B1%8F%E5%B9%95%E5%88%86%E8%BE%A8%E7%8E%87%E4%B8%8B%E7%9A%84%E8%AE%BE%E8%AE%A1%E8%A7%84%E8%8C%83.jpg)

---

3. 在美工切图的时候，注意按照@1x，@2和@3x的大小进行设计，现在@1x的手机几乎已经灭绝，所以只切@2x和@3x的即可。