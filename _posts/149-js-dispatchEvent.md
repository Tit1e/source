---
title: js 使用 dispatchEvent 主动触发事件
date: 2021-01-13 21:26:33
tags: ['学习', 'js']
---

首先方放上 MDN 文档：

> MDN：[EventTarget.dispatchEvent](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/dispatchEvent)

事情是这样的，前几天老大提了个需求，公司业务系统上有个地方需要录入带圈的数字：①②③④⑤⑥⑦⑧⑨。但这这个这种数字对于计算机水平不一的录入人员来说，有的人根本不知道怎么输入，于是老大要我做个快捷的录入方式，在页面上放一排按钮，用户点击按钮就可以把对应数组插入到光标所在的位置。

在做这个需求的过程中我就发现了两个问题：

1. 直接改变 input、textarea 的 value 值，会造成 value 与 v-model 的值不同步。
2. el-input 的一个坑，就是改变 el-input 的 value 在视图上会更新，但实际的 v-model 绑定的值并不会更新。

后来发现只要在改变完值之后再手动改变一下输入框中的内容就没问题了，比如打个空格。

但是这个步骤肯定不能让用户来做，所以就需要用到 dispatchEvent 这个东西了。

使用很简单：

```js
var el = document.querySelector('#elInput')
var event = new InputEvent('input')
el.dispatchEvent(event)
```

查看 [demo](https://evolly.one/demos/149-dispatchEvent/)。

