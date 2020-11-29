---
title: Vue 报 Failed to mount component 错误
date: 2020-09-28 22:00:34
tags: ['学习', 'vue']
---

开发过程中遇到了个奇怪的报错

> Failed to mount component: template or render function not defined.

看内容应该是模板引用出了问题，但是这个报错不会显示具体是哪行报错了。我 debug 了好久最终才发现问题。

在用 vue-cli 开发项目时，我会图省事在引用组件时经常省略`index.vue`，比如正常的路径是：

```
@/views/home/index.vue
```

而我经常写成

```
@/views/home
```

正常情况下这样写并没有问题，本来也支持这种写法，

当时当你的 home 目录下存在`index.js`时，它默认引用的是 js 文件而不是.vue文件，因此就会产生上面的报错，解决方法就是写上完整的文件路径。

这其实就是个很简单的问题，就是 debug 比较难。

