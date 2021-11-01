---
title: 记录一次Flutter run时出现的错误
date: 2020-05-20 22:05:29
tags:
    - 日志
    - 踩坑
    - Flutter
---
## 使用Flutter run 出现错误:

![](/images/flutter-run.png)

## 错误原因:
镜像：`https://storage.googleapis.com/download.flutter.io` 访问不了

## 解决办法：

更改仓库地址为 `https://storage.flutter-io.cn/download.flutter.io`

```
allprojects {
    repositories {
        google()
        jcenter()
        maven {url "https://storage.flutter-io.cn/download.flutter.io"}
    }
}```
