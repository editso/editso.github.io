---
title:  记一次C内存溢出问题
date: 2021-03-21 01:30:49
tags:
- Linux
- C
- GCC
categories:
  - 问题记录
  - C相关
---

#  记一次`C`内存溢出问题
## 问题代码
```
#include <stdio.h>

void func(){
        char buff[1024 * 1024 * 10];
        printf("hello func\n");
}

int main(int argc, char **argv){
        func();
        return 0;
}

```
编译后运行直接段错误,查了半天硬是没查出问题, 后突然想到是不是栈空间满了,查了下百度果不其然
在我的Linux下使用`ulimit -s`查出我的系统栈空间大小为`8kb`而我开辟了`1MB`的空间所以出现段错误

## 解决办法
将 `buff`变量加上`static`关键字或缩小`buff`大小即可解决问题,还可以将`buff`作为全局变量


