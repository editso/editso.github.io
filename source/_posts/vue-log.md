---
title: 记录一次使用vue-cli4.0 创建项目时非常慢问题
date: 2019-12-19 11:07:51
tags:
    - 日志
    - 踩坑记
---
# 记录一次使用vue-cli4.0 创建项目时非常慢问题
使用`vue`时在创建项目遇到创建项目非常慢  
然后各种办法都不行, 最后问了我朋友,朋友告诉我是我的`npm node vue`版本过低  
当时想只需要更新`npm vue`版本就行后还是下载很慢,最后将`node`更新故问题解决

### 解决步骤:
- 更新 `npm`: `npm install npm@latest -g`  
- 安装 `n`: `npm install -g n --force`
- 更新 `node`: `sudo n stable`
- 下载最新 `vue`: `sudo npm install -g @vue/cli`  
更新好后重启终端问题解决!  

### 命令整合:
``` 
npm install npm@latest -g  
npm install -g n --force  
sudo n stable 
sudo npm install -g @vue/cli 
```