---
title: 线程局部存储(TLS)
toc: true
date: 2022-02-12 20:44:14
tags:
categories:
    - Windows
    - PE
---

## 什么是线程局部存储?
[线程局部存储 (TLS) 是一种存储持续期（storage duration），对象的存储是在线程开始时分配，线程结束时回收，每个线程有该对象自己的实例。这种对象的链接性（linkage）可以是静态的也可是外部的。](https://zh.wikipedia.org/wiki/%E7%BA%BF%E7%A8%8B%E5%B1%80%E9%83%A8%E5%AD%98%E5%82%A8)
在`Windows`上`TLS`的存储结构为:
![](/images/tls/tls.png)

## 静态TLS
静态`TLS`需要使用 `__declspace(thread) 变量` 进行标识, 当编译器进行打包时会将它存储到特定的节中(`.tls`)它将会变成一个`tls`模板, 在程序进行线程创建时将会开辟一个足够大的空间将`tls模板`数据复制进去(`tls副本`), 所以每个线程访问与修改它时互不干扰.


## PE加壳带静态TLS如何修复?
这个问题我也是折腾了很久, 在网上找了很久的资料基本上都是讲的`TLS Callback`,反调试相关的, 然后看了`<<Windows PE权威指南>>`虽然有说静态TL是如何存储的,但是没有例子,只是说了下 不过已经知道`TLS`是存储在哪儿的 那不就好办了? 书上说的是
`“当创建线程时，加载器通过将线程环境块（TEB）的地址放入FS寄存器来传递线程的TLS数组地址。距TEB开头0x2C的位置处的字段ThreadLocalStoragePointer指向TLS数组”` 书上这里说的是32位的, 64位的可以自己去查 [TEB相关字段偏移](https://www.geoffchappell.com/studies/windows/km/ntoskrnl/inc/api/pebteb/teb/index.htm) 也就是说只要我将 `TLS`模板中的数据先写到一块内存中, 然后将数据首地址写入这个数组中就好了?
跟着书上说的我去试了下, 发现有大坑!!!, 然后我仔细想了想了`TEB`在每个线程都是不一样的, 所以我加进去的只是`TLS副本`也就是说我只给我当前获取到`TEB`的那个线程加进去了, 其他的还是没有然后这样会导致我的程序在开新线程的时候获取不到`TLS`数据... 离谱吧, 所以在程序中不开线程访问是莫问题滴... 但是对于简单的程序还好, 要是遇到网络相关操作的一个线程?? 所以这个算只是解决了半个问题??.. 现在大概是知道问题所在, 我修改的只是`TLS`副本, 所以我只需要修改加载器的`TLS`模板数据就可以了? (也就是说将加载器`TLS`数据模板替换为我主程序的模板数据).  但是我怎么知道加壳程序`TLS`模板存储位置呢.. 带着这个问题又去查资料, 果然在`Github`找到一个外国佬写的[pe_loader](https://github.com/polycone/pe-loader)而且它支持`静态TLS`的加载, 所以我去看了他的代码 [tls_support.cpp](https://github.com/polycone/pe-loader/blob/master/loader/src/loader/tls_support.cpp)

![](/images/tls/github_tls_001.png)
![](/images/tls/github_tls_002.png)

他获取这个模板数据首地址是硬搜的, 先拿到`TLS`表然后去内存匹配
芜湖~ 这不就是我想要的? 好像他的代码挺复杂的, 不过已经知道办法了只需要撸代码了


## 代码
代码就不贴了, 我写的代码垃圾.. 就截个图吧 
![](/images/tls/github_tls_003.png)



## 参考资料
> - [线程局部存储](https://zh.wikipedia.org/wiki/%E7%BA%BF%E7%A8%8B%E5%B1%80%E9%83%A8%E5%AD%98%E5%82%A8)
> - [线程局部存储结构图](https://docs.microsoft.com/zh-cn/windows/win32/procthread/thread-local-storage)
> - [找首地址](https://github.com/polycone/pe-loader/blob/master/loader/src/loader/tls_support.cpp)
