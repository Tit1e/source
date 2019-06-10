---
title: Mac 创建 .htaccess 文件
date: 2018-05-07 20:41:53
tags: ['学习']
---
在正常情况下，Mac 不允许用户创建以 `.` 开头的为文件或文件夹，如果创建，你将得到下图中的提示信息。
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144331.png)
<!--more-->
但是有时候我们又必须要创建，比如用 Apache 的服务器要重写路径，`.htaccess`文件必不可少。接下来就来说说如何创建以 `.` 开头的为文件或文件夹：
先打开终端，输入
```bash
defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder
```
然后你会发现 Finder 重启了，而且可能目录中多了很多半透明的文件夹，这些都是隐藏文件或文件夹，平时是受系统保护的。现在就可以创建以 `.` 开头的文件或文件夹了，修改完成后，只要运行
```bash
defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder
```
就可以恢复原状。