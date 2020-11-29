---
title: webpack 报错 error：Support for the experimental syntax 'classProperties' isn't currently enable
date: 2020-09-01 09:29:52
tags: ['学习', 'webpack']
---

前阵子封装完波形插件在用 webpack 打包的时候，出现了异常：

```bash
error：Support for the experimental syntax 'classProperties' isn't currently enable
```

从这个报错中可以知道出现这个异常的原因是我封装的类中用了 Class 的实验性语法，虽然我已经装了 Babel，但我安装的 Babel 并不支持试验性语法的转译，所以需要单独安装转译试验性语法的插件。

```bash
npm i -D @babel/plugin-proposal-class-properties
```

另外还需要在 .babelrc 中配置：

```js
{
  "plugins": [
    ["@babel/plugin-proposal-class-properties"]
  ]
}
```

之后再进行编译就不会出现上面的报错。