---
title: element-ui el-table 表格高度自适应问题解决方案
date: 2020-04-14 21:14:40
tags: ['学习']
---

工作中由于公司业务需要，要求 table 做到整个 table 始终能在页面上完全显示， head 固定，body 超出滚动，由于公司使用的是 element-ui，el-table 组件提供了 height 属性，但是这个属性的值必须是具体的像素，因此，当页面的窗口大小发生变化时，想要表格高度自适应，一般的做法就是监听窗口大小是否改变，如果改变了就重新计算 table 的高度，然后重新设置。我之前的做法是这样的：

我们公司基本都是后台应用，结构差不多就是这个样子，菜单与顶栏的宽度与高度是固定的，而且菜单部分与表格高度的计算其实没有关系。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-04-14-134209.png)

因此我计算表格高度的时候，一般都是：

整个网页的高度 - 顶栏的高度 - 各种 margin，padding 的高度 = 表格高度

然后监听窗口的变化：

```javascript
window.addEventListener('resize', () => {
	//重新计算高度
})
```

直到后来我遇到了这个需求：

页面分为左右两部分，可拖动改变左侧部分的宽度，右侧还是自适应。乍一看这个左侧改变宽度跟右侧好像没什么关系，原来那套也适用啊，但是再仔细一想，左侧改变了有可能导致右侧筛选条件宽度不够而换行，一换行筛选条件的高度就改变了，这时候是需要重新计算高度的，但是上面监听的代码只监听窗口大小改变，而现在的情况窗口的大小并没有被改变，只是 div 的宽度发生了变化，后来我 google 了一圈，发现了 [css-element-queries](https://github.com/marcj/css-element-queries) 这个库中的 `ResizeSensor`  这个类可以实现对元素大小改变的监听，使用方法也很简单：

```javascript
/**
 * element 需要监听的元素
 */
new ResizeSensor(element, () => {
	//callback
  //重新计算高度
})
```

并且在这次调整的过程中，我想到了用 flex 属性来让右侧的筛选条件跟 table 自行分配空间，因为这类表格的页面结构基本是固定的，上面是筛选条件，下面是表格，于是我封装了一个  `<form-page>` 组件，结构就是大概 是这样：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-04-14-141421.png)

这个结构下，我就不用再去关心除了筛选条件的高度外我还需要减去多少的额外高度，我只要关心我的 table slot 区域有多少高度，我就把 table 设为多少高度就可以了。

当然内容展示区域的高度是需要计算过的，但是这个只要在全局样式中用 `calc` 计算一次高度就可以了。

下面是 demo 的演示效果：

<video src="https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-04-14-element-ui-table-auto-height.mp4" style="width: 100%" controls ></video>

[demo 源码](https://github.com/Tit1e/element-ui-table-auto-height)

**2020年8月27日更新：**

目前 2.13 版本的 table 已经支持height=“100%”，所以可以放弃上面的实现方式了。