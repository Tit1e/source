---
title: vue-cli 使用 ECharts 水球图
date: 2017-12-17 20:14:50
tags: ['学习', 'vue']
---
公司有个平台有改版的打算，UI 出了首页的设计稿，我看了下，大致构思了一下，打算用 vue 和 Element-UI 来写。前阵子刚忙完，这几天比较空，我就看着 UI 稿在想实现方式，其中一个是水球图。这篇先记录一下 vue-cli 中生成水球图的方法。
<!--more-->
首先`npm`包下载必不可少：
```
//安装 ECharts
npm install echarts
//安装 水球图插件
npm install echarts-liquidfill
```
随后在`.vue`文件中引入
```javascript
import echarts from "echarts";
import "echarts-liquidfill/src/liquidFill.js";
```
之后就该怎么用就怎么用。可以参考 [官方例子](http://gallery.echartsjs.com/editor.html?c=liquidfill-basic) ，也可以参考 echarts-liquidfill 的 [github](https://github.com/ecomfe/echarts-liquidfill) 文档。
