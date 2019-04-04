---
title: git克隆远程仓库进行修改提交
date: 2016-10-04
tags: ['elementary OS','学习笔记','git']
---
之前云课堂大作业在windows下开发，现在因为使用EOS比较多，所以想把项目克隆下来然后修改提交，但是由于没接触过git命令行，windows下是使用客户端进行提交的，所以到了ubuntu下对着这对命令行是一脸懵逼，网上找了半天教程总算是能达到我想要的要求了。
<!--more-->
```bash
#cd至目标文件夹
git init
git clone https://github.com/Tit1e/homework.git
#cd进入仓库文件夹
git checkout -b gh-pages origin/gh-pages
#查看修改了哪些文件，可以不执行
git status
git add .
#修改说明
git commit
#第一次提交的时候执行，之后直接直接git push
git config --global push.default simple
//提交
git push
```