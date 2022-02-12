---
title: GIT仓库的使用
date: 2019/7/31 9:10:00
tags: 
    - 学习笔记
    - 仓库
    - GIT
categories:
  - 软件相关
  - GIT
---
# GIT仓库的使用

## 配置基本信息  
- `git config --global user.name "用户名"`  
- `git config --global user.email "邮箱"`

## 克隆分支
- `git clone xxxxxx.git`

## 同步分支 
- `git clone xxxxxx.git - b 分支名`

## 上传提交信息
- `git commit  -m  "提交信息"`

## 将本地的库链接到仓库 
- `git remote add origin xxxxx.git`

## 查看代码的修改状态 
- `git status`

## 红色或绿色部分字体是工程内的发生修改的状态标识 
- `modified` 代表文件和上一版本相比，有过修改  
- `new file`  代表文件是新增加的  
- `deleted`   代表文件被删除了，提交成功后，文件将从repository中删除  
- `untracked file` 一般都是新增加的文件夹

## 查看代码的修改内容  
- `git diff <filename>`

## 添加一个文件 
- `git add <filename>`

## 删除一个不需要的文件 
- `git rm <filename>`


## 增加全部需要上传的文件  
- `git add --all`
- `git add -A`
- `git add .`

## 如果发现有文件漏提或注释有误，使用`amend`修正 
- `git commit --amend`  
    注意：使用commit命令只是将修改提交到本地仓库

## 同步到服务器前先需要将服务器代码同步到本地 
- `git pull`  
    如果执行失败，就按照提示还原有冲突的文件，然后再次尝试同步  
    `git checkout -- <有冲突的文件路径>`

## 同步到服务器  
- `git push origin  <本地分支名>`  
    如果执行失败，一般是没有将服务器代码同步到本地导致的，先执行上面的`git pull`命令。
