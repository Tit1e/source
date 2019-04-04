---
title: 编写一个全局 SVG 图标组件
date: 2019-03-27 12:30:06
tags: ['vue','vue-cli','学习笔记']
---

在现在网站开发中， icon 是必不可少的元素，它可以使页面更加美观，内容更加丰富，某些场景也能更加直观地表达含义。icon 也分好多种类型，以前是使用图片，后来有了字体图标，SVG。由于占用空间大以及使用不方便（比如不同终端需要不同尺寸的图片），用图片做图标的方式已经渐渐被淘汰了，取而代之的是字体图标以及 SVG，这两种都是矢量图形，光放大不失真这个特点就很让人心动，并且体积还比图片小。

字体图标顾名思义就是这个图标就是一个字体，只是它显示的是个图标。因此 CSS 能对字体设置的属性，对字体图标也同样适用。

缺点的话可能只是想使用这个图标库的某几个图标，但是却必须把整个图标库引入，造成了资源浪费。不过现在这个问题在 [阿里妈妈](<https://www.iconfont.cn/>) 图标管理平台得到了很好的解决，可以自己定义需要用到的图标，然后系统会帮你生成一个图标库，可以在线引入，也可以下载到本地，可以说是造福了广大前端工程师和设计师。

虽然阿里的这个平台已经做得很好了，但是就我而言，在实际项目开发中还是遇到了不方便的问题：

* 之前没考虑周全，或者后来来了新的需求，需要增加新图标，而要更新图标库是需要账号的，更新一个图标可能会涉及到根很多人沟通。
* 更新完图标库后还需去更新在线引用地址或更新本地文件。

SVG，以下是 [维基百科](<https://zh.wikipedia.org/wiki/%E5%8F%AF%E7%B8%AE%E6%94%BE%E5%90%91%E9%87%8F%E5%9C%96%E5%BD%A2>) 对它的描述：

> **可缩放矢量图形**（英语：**Scalable Vector Graphics**，**SVG**）是一种基于 [可扩展标记语言](https://zh.wikipedia.org/wiki/%E5%8F%AF%E6%89%A9%E5%B1%95%E6%A0%87%E8%AE%B0%E8%AF%AD%E8%A8%80) （[XML](https://zh.wikipedia.org/wiki/XML)），用于描述二维 [矢量图形](https://zh.wikipedia.org/wiki/%E7%9F%A2%E9%87%8F%E5%9B%BE%E5%BD%A2) 的图形格式。SVG由 [W3C](https://zh.wikipedia.org/wiki/W3C) 制定，是一个 [开放标准](https://zh.wikipedia.org/wiki/%E5%BC%80%E6%94%BE%E6%A0%87%E5%87%86)。

SVG 其实是 XML格式的文件，因此你可以直接使用编辑器打开 .svg 文件，并且可对它进行编辑。它的优点在于：

* 一个图标就是一个文件，灵活性高。
* 不同区域可以使用不同颜色，也就是说可以用它绘制彩色图标。
* 矢量图形，放大不失真。

SVG 文件的使用可以分为很多种，你可以把它当作图片直接在 `img` 标签或 CSS 的 `background-image` 属性中使用，也可使用 `svg` 标签中使用。但是这样使用会很麻烦，避免不了一串很长的地址，如果只要写一个标签，并传入一个图标名字就可直接使用，那就完美了。

我在  [vue-element-admin](<https://github.com/PanJiaChen/vue-element-admin>) 项目中发现了这种使用方式，因此把它提取了出来。

首先参考 [在 Vue CLI 3 中配置 svg-sprite-loader](<https://evolly.one/2019/03/12/53.vue-cli3-use-svg-sprite-loader/>) 在 webpack 添加 SVG 的 loader。并建好相应的目录，文章中我的目录建在 `src/icons`。

在 `icons` 文件夹下新建 `svg` 文件夹用来存放图标，在同级目录下新建 `index.js`。

然后我们去编写组件，名字就命名为 `SvgIcon`。

```vue
// SvgIcon.vue
<template>
  <svg :class="svgClass" aria-hidden="true" v-on="$listeners">
    <use :xlink:href="iconName" />
  </svg>
</template>

<script>
export default {
  name: 'SvgIcon',
  props: {
    iconClass: {
      type: String,
      required: true
    },
    className: {
      type: String,
      default: ''
    }
  },
  computed: {
    iconName() {
      return `#icon-${this.iconClass}`
    },
    svgClass() {
      if (this.className) {
        return 'svg-icon ' + this.className
      } else {
        return 'svg-icon'
      }
    }
  }
}
</script>

<style scoped>
.svg-icon {
  width: 1em;
  height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}
</style>

```

然后继续编写 `src/icons/index.js`

```javascript
// src/icons/index.js

import Vue from 'vue'
import SvgIcon from '@/components/SvgIcon'// svg组件

// 注册为全局组件
Vue.component('svg-icon', SvgIcon)

const req = require.context('./svg', false, /\.svg$/)
const requireAll = requireContext => requireContext.keys().map(requireContext)
requireAll(req)

```

最后，在 `main.js` 中引入

```javascript
// main.js

import '@/icons'
```

这样你就可以在系统任何地方使用 `<svg-icon icon-name="svg名称" />` 引入图标了，当你想增加图标时，只要往 `src/icons/svg` 中添加图标就可以了。

结合 [使用 Vue CLI3 编写组件库并发布至 npm](<https://evolly.one/2019/03/26/54.vuecl3-npm-publish/>)，你就可以拥有一个属于自己的图标库。你只要维护这个图标库，然后在其他项目中安装与更新它就可以了。

