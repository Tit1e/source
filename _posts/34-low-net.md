---
title: 陋 网
date: 2017-10-18 22:28:16
tags: ['学习']
---
### 前言
前阵子把今年四五月份时候写的陋网重写了. 之前用的是 vue.js, vue-resource 和 jQuery, 现在想想就算是那时候, jQuery也是完全没必要引入的.后来不知道为何网站在 PC 端列表无法渲染了, 原因是 PC 端获取到的 data 编程字符串格式了, 而我之前开发的时候明明是能正常获取的, 而且移动端是能正确渲染列表的, 这个原因我至今没有想明白. 

现在会了 vue-cli 之后, 就想着把网站重写了, 做成 SPA 单页应用, 这样才更像个 App.于是就有了这个项目. 
<!-- more -->
### 项目地址: 
[https://github.com/Tit1e/simpleWeb](https://github.com/Tit1e/simpleWeb)

### 项目描述
* 采用 vue-cli 开发
* vue-router 进行页面路由控制
* 采用 sass CSS 预处理, 对网站整体风格进行统一控制
* localstorage HTML5本地存储
* ThinkPHP 后台框架
* 前后端分离

### 项目结构
* `/数据库结构.sql` - 数据库结构,其实实际只用到了2张表而已
* `/program` - vue 项目目录
* `/static` - vue 打包后的静态资源目录
* `/Application/Home/View/Index/` - 打包后的 index.html 文件目录


** 下篇文章将总结开发过程中遇到的问题. ** 