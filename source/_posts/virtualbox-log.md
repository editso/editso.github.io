---
title: 解决VirtualBox无法启用嵌套分页
date: 2020-10-30 23:19:26
tags:
    - 日志
    - VirtualBox
---
# `VirtualBox` 无法启用嵌套分页
![问题截图](/public/images/virtualbox/nested.png)

## 解决方式

开启嵌套分页:

**`VBoxManage modifyvm "虚拟机名字" --nested-hw-virt on`**  

![修复](/public/images/virtualbox/nested-fixs.png)

