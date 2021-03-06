---
title: 在 Vue CLI 3 中配置 svg-sprite-loader
date: 2019-03-12 14:25:46
tags: ['vue', '学习']
---

### 安装

```bash
npm install svg-sprite-loader --save-dev
```

### 配置

在 Vue.config.js 中加入如下配置：

```javascript
const path = require('path')

function resolve(dir) {
    return path.join(__dirname, '.', dir)
}

module.exports = {
    chainWebpack: config => {
        // 删除默认配置中处理svg
        config.module.rules.delete('svg')
        config.module
            .rule('svg-sprite-loader')
            .test(/\.svg$/)
            .include
            .add(resolve('src/icons')) // 处理svg目录,根据实际情况修改
            .end()
            .use('svg-sprite-loader')
            .loader('svg-sprite-loader')
            .options({
            symbolId: '[name]'
        })
    }
}
```

当然其他的 webpack 配置也可以在 Vue.config.js 中配置，包括反向代理等等。具体可查看[这里](https://cli.vuejs.org/zh/guide/webpack.html)。

[参考文章](https://juejin.im/post/5bc93881f265da0aea69ae2e)