---
title: 阻止表单只有一个 input 时回车自动提交
date: 2020-03-08 15:13:44
tags: ['学习']
---

之前在开发过程中遇到过这个问题，就是当一个表单中只有一个 input 时，enter 会直接提交表单。当初我用的是 Element-UI  的表单组件，本来我以为是 Element-UI的问题，后来查了后发现是原生表单就有这个特性。我当时是使用异步请求的数据，因此并不需要原生的提交，所以只有一个 input 时得想办法阻止原生的提交功能。

Element-UI 因为是封装过的，可以用它自己的方法来解决：

```html
<!-- 只要在表单元素上加入 @submit.native.prevent -->
<el-form @submit.native.prevent>
</el-form>
```

原生表单的组织方式我也查了一下，在这总结一下：

方法一：

在表单中再加入一个隐藏的 input

```html
<form>
	<input value=""></input>
	<input value="" style="display: none;"></input>
</form>
```

方法二：

表单提交时阻止提交

```html
<form onsubmit="return false;">
	<input value=""></input>
</form>
```

方法三：

监听 input  `keydown` 事件

```html
<form>
	<input onkeydown="if(event.keyCode === 13) { return false }"></input>
</form>
```