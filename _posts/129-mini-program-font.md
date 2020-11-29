---
title: uni-app 小程序设置自定义字体
date: 2020-04-27 22:21:56
tags: ['小程序']
---

最近在开发动森的一款小程序，到目前为止踩了不少坑，今天记录一下小程序引入自定义字体的方式。后续会陆续更新其他踩坑记录。

首先，小程序是支持使用自定义字体的，使用 [wx.loadFontFace()](https://developers.weixin.qq.com/miniprogram/dev/api/ui/font/wx.loadFontFace.html) 就可以引入字体，但是使用这个 api 有一些限制：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-04-27-143333.png)

需要特别注意的点有：

第一点：字体格式，保险起见使用 ttf 格式，如果不是 ttf 格式，可以在 https://transfonter.org/ 在线转一下。

第二点：字体链接必须是 **https**，意味着不支持引入本地字体。

第三点：链接必须**同源**或者开启了 **cors** 支持。

其他几点注意点对使用有影响，但没以上三点重要。

我将字体上传到了腾讯的对象存储上，因此我要在存储桶的设置中对 servicewechat.com 开启 cors 支持：

1、登录腾讯云，进入对象存储控制台

2、点击**【存储桶列表】**菜单

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-04-27-144352.png)

3、点击上传了字体的存储桶，进入存储桶详情

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-04-27-144625.png)

4、点击左侧**【基础配置】**菜单

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-04-27-144749.png)

5、找到**【跨域访问CORS设置】**卡片，点击添加规则，照下图填写表单并保存，过一会儿就会生效。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-04-27-144838.png)

官方文档上说如果需要全局支持字体的话 `wx.loadFontFace` 需要在 `app.js` 中调用并且设置 `global` 为 `true`。

并且使用 `global` 属性对调试基础库版本有要求，需要 2.10.0 以上，否则字体只会在调用这个 api 的页面生效。![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-04-27-150114.png)

还有注意中提到的第四点也注意一下，可能会有个报错，但是字体实际已经生效，这个报错可以忽略。



但是我使用的是 `uni-app`，我尝试写在 `main.js` 入口文件中，但是并没有生效，不过 `uni-app` 支持 vue 的 mixins，创建一个font.js：

```javascript
// @/mixins/font.js

export default {
	onLoad() {
		uni.loadFontFace({
			family: 'DFYuanW9',
			source: 'url("https://xxxxxxx.ttf")',
		})
	}
}
```

在 `main.js` 中：

```javascript
import mixin from '@/mixins/font.js'

Vue.mixin(mixin)
```

并在全局 css 中使用字体：

```css
page{
  font-family: 'DFYuanW9'
}
```



这样每个页面就都会生效了。

下面是字体使用效果：

不使用字体：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-04-27-IMG_2437.PNG)

使用字体：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-04-27-IMG_2436.PNG)