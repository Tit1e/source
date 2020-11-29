---
title: vue-cli3 直接将 markdown 转为 html
date: 2019-07-01 21:56:15
tags: ['vue', '学习']
---

vue-cli3 中直接将 `.md` 文件转为 html。我用到的是 webpack 的 [vue-markdown-loader](https://github.com/QingWei-Li/vue-markdown-loader)，这个 loader 可以直接将 `.md` 文件转换为 vue 的组件，然后可以直接在页面中使用，非常的方便。

### 使用方法

安装

```bash
npm i vue-markdown-loader -D
npm i  vue-loader vue-template-compiler -D
# 样式
npm i github-markdown-css -D
npm i highlight.js -D
```

配置

```js
// vue.config.js 如果没有就在项目根目录新建
module.exports = {
  chainWebpack: config => {
    config.module.rule('md')
      .test(/\.md/)
      .use('vue-loader')
      .loader('vue-loader')
      .end()
      .use('vue-markdown-loader')
      .loader('vue-markdown-loader/lib/markdown-compiler')
      .options({
        raw: true
      })
  }
}
```

使用

```vue
<template>
	<!-- class markdown-body 必须加，否则标签样式会出现问题 -->
  <div class="markdown-body">
    <markdown />
  </div>
</template>

<script>
// 引入 markdown 文件，引入后是一个组件，需要在 components 中注册
import markdown from '@/assets/ApiDocument.md'
// 代码高亮
import 'highlight.js/styles/github.css'
// 其他元素使用 github 的样式
import 'github-markdown-css'
export default {
  components: {
    markdown
  },
}
</script>
```

