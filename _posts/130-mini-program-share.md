---
title: uni-app 小程序给所有页面添加分享
date: 2020-04-28 22:59:39
tags: ['小程序']
---

小程序的转发按钮默认是关闭的，需要人为打开，为小程序更方便地传播，所以我需要给小程序加上这个功能。

我的目的只是方便传播小程序，需求跟其他小程序相比没那么复杂，分享后我只需固定进入小程序首页。

小程序在 Page 注册小程序页面时，有一个 [onShareAppMessage](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onShareAppMessage-Object-object) 函数，这个函数需要 `return` 一个`Object` ，这个对象中是聊天界面显示的小程序卡片的一些信息，标题，默认图片，页面路径。

原生小程序使用就是这样：

```javascript
Page({
  onShareAppMessage: function (res) {
    if (res.from === 'button') {
      // 来自页面内转发按钮
      console.log(res.target)
    }
    return {
      title: '自定义转发标题',
      path: '/page/user?id=123'
    }
  }
})
```

但是这个函数需要在每个页面都写一遍，就很麻烦。我用的是 uni-app，所以 mixins 又派上用场了：

```javascript
// @/mixins/share.js

export default {
	onShareAppMessage(res) {
		return {
			path: 'pages/tabBar/tools/tools',
		}
	}
}

```

在 `main.js` 中：

```javascript
import mixin from '@/mixins/share.js'

Vue.mixin(mixin)
```

这样，小程序中的所有页面都可以发起转发了，不过我转发的路径固定都是小程序首页，你也可以使用 [getCurrentPages()](https://developers.weixin.qq.com/miniprogram/dev/reference/api/getCurrentPages.html)  来获取当前页面栈，从而获取当前页面路径，这样就可以实现动态转发路径，转发当前页面。