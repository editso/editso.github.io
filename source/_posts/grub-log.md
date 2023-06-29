---
title: GRUB双系统找不到Windows问题
date: 2021-03-21 00:55:37
tags: 
- GRUB
- Manjaro
- Windows
- Linux
categories:
  - 问题记录
  - Grub
---

# `GRUB`双系统找不到`Windows`问题
问题截图  
![GRUB](/images/grub/01.png)

## 解决办法
1.编辑: `/etc/default/grub`  
2.添加: `GRUB_DISABLE_OS_PROBER=false`  
3.执行: `sudo update-grub`  
![GRUB](/images/grub/02.png)


参考: [记一次 Grub 找回 Win10 启动项的过程](https://dreamanddead.github.io/post/grub-find-win-back/)










