---
title:  记录一次学习C++时编译失败出现(multiple definition of xxx first defined here collect2)
date: 2020-11-03 23:49:32
tags:
- CPP
- 日志
---

# 记录一次学习C++时编译失败出现(multiple definition of xxx first defined here collect2)
![log](/images/cplusplus/log.png)
报错说我重复定义了这个重载函数, 网上查了半天,说是头文件没有写`#ifndef & #define`可是我明明写了, 还是不起作用,又说使用关键字`extern`, 可是我就是要定义在头文件里面,后来看了`c++`的全局重载函数,发现都写了`inline`修饰,于是试了试结果成功了

## 解决办法
使用`inline`修饰这个函数,使用它修饰需要考虑自己的代码是否符合`inline`


## `inline`相关
在 c/c++ 中，为了解决一些频繁调用的小函数大量消耗栈空间（栈内存）的问题，特别的引入了 inline 修饰符，表示为内联函数。  

**个人理解:**
我理解它其实就是和宏定义差不多,宏定义会在编译器编译时进行宏展开,而`inline`做的事情其实和它是差不多的,只不过`inline`会有一些使用限制.
然后我这个错误在还未链接时是没有错误的,在链接是就出现了重复定义,使用`inline`就正好解决了我遇到的问题,因为它在编译时就会把这个函数转换为内联函数,所以在链接时就不会出现重复定义问题


[参考:runoob.com](https://www.runoob.com/w3cnote/cpp-inline-usage.html)
