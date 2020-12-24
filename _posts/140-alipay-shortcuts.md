---
title: 用快捷指令快速打开支付码、扫一扫或支付宝指定小程序
date: 2020-12-23 13:26:57
tags: ['折腾']
---

首先说说使用快捷指令打开支付码、扫一扫等功能的好处，因为在ios14下，支付宝自带的小组件就有这个功能。

1. 小组件上的功能是固定的，无法自定义。
2. 小组件样式不一定符合用户审美（至少不符合我的审美，不然我也不会写这篇文章）。
3. 小组件占用屏幕空间大，降低了屏幕的使用效率。

这是支付宝小组件的效果：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-23-IMG_7334.jpg?imageMogr2/thumbnail/!40p)

只是使用快捷指令的效果，图上的付款码，扫一扫和叮嗒出行都是支付宝的功能快捷指令化：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-23-IMG_7335.jpg?imageMogr2/thumbnail/!40p)

使用效果如下：

{% dplayer "url=https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-23-140-alipay-shortcuts.mp4" "pic=https://photo-album-1251959693.cos.ap-nanjing.myqcloud.com/video-poster.png"  "volume=0" "loop=no" "autoplay=false" %}

可以看出使用效果上并没有太大的差别，都是点击图标都直接进入扫码界面，没有多余的步骤。

## 实现过程

实现这个效果很简单

1. 新建一个快捷指令
2. 打开指定 url
3. 保存并添加到桌面

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-23-IMG_7336.png?imageMogr2/thumbnail/!40p)

如何将快捷指令发送到桌面可以参考[桌面美化](https://evolly.one/2020/12/17/138-ios-desktop/)这篇文章。

上面途中展示的扫一扫的链接，我这里还有一些其他的功能链接，有需要自取。

* 健康码: alipays://platformapi/startapp?appId=2021001139676873

* 微信扫码: weixin://scanqrcode

* 支付宝扫一扫: alipayqr://platformapi/startapp?saId=10000007

* 支付宝付款码: alipayqr://platformapi/startapp?saId=20000056

* 支付宝收款码: alipayqr://platformapi/startapp?saId=20000123

* 支付宝乘车码: alipayqr://platformapi/startapp?saId=200011235

* 支付宝查快递: alipays://platformapi/startapp?appId=20000754

* 蚂蚁庄园: alipays://platformapi/startapp?appId=66666674

* 蚂蚁森林： alipays://platformapi/startapp?appId=60000002

上面这些都是现成的，但仔细观察可以发现上面这些链接中，其实不一样的只是地址中的 appId不同，其他都是一样的，所以是不是只要有应用的 appId，就可以通过这种方式实现快速打开应用？

的确可以，比如我自己做了一个叮嗒出行的快捷方式，这是一个支付宝内嵌的小程序，支付宝也支持添加桌面书签，但这种方式会在打开过程中做一次跳转，体验非常差。

下面说一下提取程序中的 appId 的方式：

1. 在支付宝中打开要添加的程序，点击程序右上角的三个点，在下方弹出功能菜单。

   ![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-23-IMG_7340-1.png?imageMogr2/thumbnail/!40p)

2. 点击下方的【添加到桌面】这时小程序会在浏览器打开一个页面，上面会有添加到桌面的教程。

   ![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-23-IMG_7341.jpg?imageMogr2/thumbnail/!40p)

3. 复制浏览器地址

4. 从地址中提取 appId

这时我复制出来的地址：

`https://render.alipay.com/p/s/shortcut/index?appId=2018110161946659&appName=叮嗒出行&appIcon=https://appstoreisvpic.alipayobjects.com/prod/f38acc92-21a5-48be-9a66-8a69474b672a.png@120w.png`

地址中的 `appId=2018110161946659` 这串 appId 就是我们要找的。

找到这串 id 后，我们把 id 跟前面的的那串地址拼到一起 `alipays://platformapi/startapp?appId=2018110161946659`，然后在快捷指令中新建一个指令就完成了。

看一下原来的打开方式跟用快捷指令打开的区别：

{% dplayer "url=https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-23-140-alipay-shortcuts2.mp4" "pic=https://photo-album-1251959693.cos.ap-nanjing.myqcloud.com/video-poster.png"  "volume=0" "loop=no" "autoplay=false" %}

微信如果可以拿到 appId 也可以使用这种方式操作。