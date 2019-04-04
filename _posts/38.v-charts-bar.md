---
title: v-charts 实现柱状图渐变效果
date: 2017-12-17 21:03:48
tags: ['学习笔记', 'vue']
---
其实在考虑使用`echarts`之前我还考虑过使用饿了么的`v-charts`，但后来考虑到实际情况，还是使用了没有经过二次封装的`echarts`。虽然没有最终没有使用`v-charts`，但还是记录一下。

`v-charts`的使用方法[ 官方文档 ](https://elemefe.github.io/v-charts/#/)讲得已经比较详细了，但我的需求有个柱状图使用渐变色的需求，这里没有详细讲，但其实运行报错已经说得很清楚了。
首先安装`v-charts`
```
npm i v-charts -S
```
在`main.js`引入
```javascript
import VCharts from 'v-charts'
Vue.use(VCharts)
```
<!--more-->
如果这时候你去组件里写入以下代码并运行：
```html
<template>
  <ve-histogram :data="chartData" :settings="chartSettings"></ve-histogram>
</template>
```
```javascript
module.exports = {
  created: function() {
    this.chartData = {
      columns: ["日期", "成本", "利润", "占比", "其他"],
      rows: [
        { 日期: "1月1日", 成本: 1523, 利润: 1523, 占比: 0.12, 其他: 100 },
        { 日期: "1月2日", 成本: 1223, 利润: 1921, 占比: 0.345, 其他: 100 },
        { 日期: "1月3日", 成本: 2123, 利润: 5523, 占比: 0.7, 其他: 100 },
        { 日期: "1月4日", 成本: 4123, 利润: 6523, 占比: 0.31, 其他: 100 }
      ]
    };
    this.chartSettings = {
      metrics: ["成本"],
      itemStyle: {
        normal: {
          color: new echarts.graphic.LinearGradient(
            0, 0, 0, 1,
            [
              {offset: 0, color: '#83bff6'},
              {offset: 0.5, color: '#188df0'},
              {offset: 1, color: '#188df0'}
            ]
          )
        }
      }
    }
  }
}
```
控制台会报错："**echarts is not defined**"。所以解决方法很明显，只要引入`echarts`就行了。
```javascript
import echarts from 'echarts/lib/echarts'
```
