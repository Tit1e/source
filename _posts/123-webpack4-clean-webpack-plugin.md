---
title: 'TypeError: CleanWebpackPlugin is not a constructor'
date: 2020-02-27 21:23:27
tags: ['学习', 'webpack']
---

昨天在用 webpack 4.0 搭服务的时候，使用 clean-webpack-plugin 插件时遇上了一个报错：

`TypeError: CleanWebpackPlugin is not a constructor`

使用方法：

```js
const CleanWebpackPlugin = require('clean-webpack-plugin')
// 在 plugins 中
new CleanWebpackPlugin(['dist'])
```

这个插件是用于打包时清空输出目录下上一次的打包文件，dist 是要清空的目标文件夹名称，之前就是这么使用的，现在却不行了，初步猜测应该是版本问题导致的。

既然报 `CleanWebpackPlugin is not a constructor` ，那我就打印一下这默认导出的是个什么：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-02-27-134644.png)

从打印中可以看出 `CleanWebpackPlugin` 是个对象，对象内部还有个 `CleanWebpackPlugin` 是个 class 类，那解决方法显而易见了：

```js
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
// 后来在网上了解到在这个插件的 3.0.0 版本中，可以不传路径，默认就是 output 中设置的文件夹
new CleanWebpackPlugin()
```

