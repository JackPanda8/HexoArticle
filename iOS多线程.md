---
title: iOS多线程
date: 2017-02-04 19:45:39
tags: iOS开发
---

* ios多线程四种方法
  1. Pthreads 支持多系统的通用多线程API

  2. NSThread ios将POSIX封装好的面相对象的多线程类

  3. GCD  Grand Central Dispatch 大法好

  4. NSOperation & NSOperationQueue ios对GCD的封装

   区别直接上图

  ![四种多线程方法](https://raw.githubusercontent.com/JackPanda8/ImageHosting/master/fourmethod.png)

<!--more-->
* GCD

  1. GCD简介

  >GCD 全称是Grand Central Dispatch，可译为“超级厉害的中枢调度器”，GCD 是苹果公司为多核的并行运算提出的解决方案， GCD会自动利用更多的 CPU 内核（比如双核、四核）来开启线程执行任务，GCD 会自动管理线程的生命周期（创建线程、调度任务、销毁线程），不需要我们程序员手动管理内存。
  >
  >![](https://raw.githubusercontent.com/JackPanda8/ImageHosting/master/gcd.png)

  2. 使用

     * 获取主队列

       主队列是全局串行队列，用于控制UI。

       ```objective-c
       dispatch_queue_t main = dispatch_get_main_queue();
       ```

     * 获取全局并行队列

       系统提供的全局并发队列，一般耗时操作放在全局并行队列执行。

       ```objective-c
       dispatch_queue_t global = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
       ```

     * 自己创建串行和并行队列

       ```objective-c
       //串行队列
       dispatch_queue_t queue1 = dispatch_queue_create("JackPanda.serial", DISPATCH_QUEUE_SERIAL);
       //并行队列
       dispatch_queue_t queue2 = dispatch_queue_create("JackPanda.concurrence", DISPATCH_QUEUE_CONCURRENCE);
       ```

     * 创建同步和异步任务

       ```objective-c
       //同步任务
       dispatch_sync(main, ^{  
       	NSLog(@"同步任务 在 串行队列 中执行")；
       });
       dispatch_sync(global, ^{
         	NSLog(@"同步任务 在 并行队列 中执行")；
       });
       //异步任务
       dispatch_async(queue1, ^{
         	NSLog(@"异步任务 在 串行队列 中执行")；
       });
       dispatch_async(queue2, ^{
         	NSLog(@"异步任务 在 并行行队列 中执行")；
       });
       ```

       其实多线程的重点就在同步异步任务—dispatch_sync和dispatch_async，串行和并行都是FIFO取出来，也都是分配cpu时间片，只不过串行队列的任务不会出现交叉使用时间片，一定是按照先后顺序接受分配cpu。

     * 从其它线程回到主线程

       ```objective-c
       dispatch_async(dispatch_get_main_queue(), ^{
       	//self.label.text  = xxx;
       });
       ```

       ​

  3. Tips

     * 其实并不是绝对的同步任务就一定在当前线程，异步任务就一定会在新开的线程执行。同步(sync)任务当从其它线程向主线程派发时，就会在主线程执行，当异步(async)任务从主线程向主线程派发时，也会在主线程执行，并不会新开线程。同步和异步主要区别是：**是否会阻塞当前线程。**
     * 无论串行还是并发队列,任务启动顺序都是按照 FIFO 的,只是并发队列允许同一时间有多个任务执行都在执行.


* NSOperation &NSOperationQueue

  NSOperation是抽象类，一般使用NSInvokeOperation和NSBlockOperation或者它们的子类创建任务，

  >步骤简述
  >
  >1. 先将需要执行的操作封装到一个NSOperation的子类对象中
  >2. 然后将NSOperation对象添加到NSOperationQueue中
  >3. 系统会自动将NSOperationQueue中的NSOperation取出来
  >4. 将取出的NSOperation封装的操作放到一条新线程中执行

  未完待续。。。