---
title: 记一次XCode项目工程文件冲突
date: 2017-05-23 10:31:00
tags: iOS开发
---

XCode的项目工程文件.pbxproj的节点排列没有固定次序并且UUID在不同的开发小伙伴的电脑上也会不一样，所以当多人同时修改项目工程文件时，必然会造成冲突，如果强制pull下来，则会导致工程打不开的问题，扎心了老铁。。。

解决方案：

1. 目前我们开发团队初期人数只有三个，所以我们目前采取的方法是只要有人修改过工程文件，就立马commit，然后再群里吼一下，呼叫大家进行更新。。。简单粗暴有木有

2. 后期开发团队发展到两位数的话，这种方式的沟通成本太高，工作效率太低，所以祭出大杀器**[xUnique](https://github.com/truebit/xUnique)**先放个链接，后面继续研究。。。

3. xUnique都做了什么？

   >- 替换所有UUID为项目内永久不变的MD5 digest
   >- 删除所有多余的节点（一般是合并的时候疏忽导致的）
   >- 用[Python重写](http://link.zhihu.com/?target=https%3A//github.com/truebit/xUnique/blob/master/xUnique.py%23L161)了我修改过的的[sort-Xcode-project-file](http://link.zhihu.com/?target=https%3A//github.com/truebit/webkit/commits/master/Tools/Scripts/sort-Xcode-project-file)的排序功能，修改版相较原版增加了以下功能：
   >  1. 对PBXFileReference和PBXBuildFile区块的排序
   >  2. 使用脚本后如果内容没有变化不会生成新文件，避免一次不必要的commit

4. ​

