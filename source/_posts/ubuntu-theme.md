---
title: ubuntu使用
date: 2019-12-24 17:05:21
tags: 
    - Theme
    - Linux
    - Ubuntu
    - 日志
---
# Ubuntu使用


### 切换阿里云软件源: 
编辑： `sudo vim /etc/apt/sources.list`  
- 将默认的：http://archive.ubuntu.com/
- 替换为： http://mirrors.aliyun.com

> 配置如下:
```
    deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

    deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

    deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

    deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

    deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
    deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

参考 [Ubuntu 镜像](https://developer.aliyun.com/mirror/ubuntu?spm=a2c6h.13651102.0.0.53322f70EzruKZ)


### 谷歌拼音输入法:
> `sudo apt install fcitx fcitx-googlepinyin`    

设置输入法: 
`设置>区域与语言>管理已安装的语言>键盘输入法系统>fcitx`  
注销电脑配置重新登录, 在应用程序中找到`fcitx配置`或搜索配置谷歌输入法 

### 主题美化工具
安装：`sudo apt install gnome-tweak-tool gnome-shell-extensions`   
shell: [ChromeOS shell theme](https://www.gnome-look.org/p/1333760/)  
鼠标指针: [Bibata](https://www.gnome-look.org/p/1197198/)  
dock栏： [dash-to-dock](https://extensions.gnome.org/extension/307/dash-to-dock/)