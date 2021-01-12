---
title: express 静态资源代理
date: 2020-12-29 20:35:23
tags: ['学习']
---

前阵子在搭建我的[摄影网站](https://album.animalcrossing.life/)，等我跑起了后端服务，要布前端页面的时候，突然想到我还需要布个前端的服务，但实际前端只是需要访问一个 index.html 文件，单独跑项目又没有必要，于是我网上查了，因为我觉得这种情况一定有人遇到过，并且肯定会有成熟的解决方案。因为我用的是 express，于是我带着 express 的关键字搜索了一下，果然，express 就提供这种服务，称为 express 静态资源代理。

使用也很简单，无非就是在原有的代码上增加一行：

```js
const path = require('path');
app.use(express.static(path.join(__dirname, 'web')));
```

web 是我放前端文件的目录，并且我写这行代码的所在文件与 web 目录都在统一层级，并且都是根目录，所以我路径就使用了__dirname。然后重启一下服务，代理就生效了，直接访问[https://album.animalcrossing.life](https://album.animalcrossing.life)访问到的就是我部署的前端 index.html 文件了。

