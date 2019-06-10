---
title: 使用 mpvue 开发小程序过程中遇到的问题及总结
date: 2019-05-01 15:48:18
tags: ['学习', '小程序']
---

最近在使用 [mpvue](https://github.com/Meituan-Dianping/mpvue) 开发一个小程序，在这过程中遇到了不少问题，因为是第一次开发，遇到的问题可能比较基础，在这里做一个记录。

* 如果你底部导航栏有三个页面，在这三个页面中的 created 中的代码会在进入小程序的时候就被执行，如果你想切到对应页面后才执行相应的方法，需要把代码写在 mounted 钩子中。

* 虽然官方是说 mpvue 支持 slot，但是我在实际使用的过程中，会出现奇奇怪怪的问题，不建议使用。

* Vuex 可以使用，并且支持程度比较好，但是默认你无法在代码中使用 `this.$store` 来获取 store，每次你要使用 store 的时候都需要在页面中引入 store，`import store from '/path/to/sotre.js'`，然后就可以正常使用了，但是我们可以通过将 store 绑定至 Vue 的原型上，不是就可以使用 `this.$store` 了吗？

  ```javascript
  main.js
  import store from '/path/to/store.js'
  Vue.prototype.$store = store
  ```

* 对应页面的 `main.json` 中加入 `"enablePullDownRefresh": true` 配置项可开启下拉刷新，然后在 `onPullDownRefresh` 钩子中可加入刷新的操作代码，不过你需要使用  `wx.stopPullDownRefresh()` 来解除下拉后的 loading 动画。

* `onReachBottom` 钩子在页面滚动到底部时出发，可用于实现到达底部时自动加载列表。

* `vidoe` 、`canvas` 、`map`、`textarea` 这些由客户端创建的原生组件在小程序中层级永远的最高的，所以在正常情况下这些元素永远是置顶的，`cover-view`、`cover-image` 这些标签可以解决这个问题，但是它们有很大的局限性，`cover-view` 只能嵌套 `cover-view` 标签或 `button` 标签，并且并不是所有 css 样式对这些标签有效，所以这些标签还是无法满足比较复杂的组件。项目中由于这个问题的存在，我将筛选条件进行的调整，最后采用点击筛选侧滑出筛选项，并在显示筛选项的时候隐藏 echarts 图表，点击搜索后再重新展示图表来解决了这个问题。