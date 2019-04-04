---
title: vue-cli 打包静态资源路径出错
date: 2018-05-03 19:53:26
tags: ['vue','vue-cli','学习笔记']
---

今天同事整理服务器的文件结构，把页面和资源文件移动了位置，由于我之前资源引用用的是绝对路径，所以我把 `index.html` 和 `static` 文件夹都放在根目录下，结果现在一移动就出现了问题，绝对路径不能用了，那只好把打包的绝对路径改成相对路径，结果是打包生成的 `js` 、 `css` 文件引用正常了，但是图片什么静态资源路径全部出错了，然后 Google 了好久，终于在 [这里](https://blog.csdn.net/a120120yyyy/article/details/78048838) 找到了解决办法，就是在 `build/utils.js` 文件中，找到`ExtractTextPlugin.extract`，加上 `publicPath: '../../'` 就能使静态资源正确引用。
<!--more-->
