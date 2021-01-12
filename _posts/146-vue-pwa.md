---
title: Vue 项目新增 PWA 支持初次尝试
date: 2021-01-10 19:21:09
tags: ['vue', '折腾']
---

前阵子制作了我的个人摄影网站 [Tit1e's Photo Studio](https://album.animalcrossing.life/)。由于是网站，因此每次访问的时候总是有些麻烦，虽然现在移动端已经可以把书签添加至桌面，但是打开的时候依旧是浏览器的界面，丑丑的工具栏跟地址栏依旧显示在界面上，而且把书签添加到桌面这种操作也太没技术含量了，于是我想到了 PWA。这玩意儿我一直在用，但是自己从来没实践过，于是正好趁着这个机会实践一番，项目本身很简单，拿来试手正好。

我这个项目改造很简单，添加 PAW 插件：

```bash
# 如果没装脚手架，那需要先安装一下 vue-cli
vue add pwa
```

但是我执行这一步的时候报错了，原因是因为 node 版本过低，依赖冲突。我原本使用的是 9.6 版本的 node，不行。又换了 11.8 的版本，依旧冲突。最后我直接使用了最新的 15.5.1 的版本。

执行完成后，你的项目中会多出一些文件。

首先修改 vue.config.js，在配置文件中新增下列配置项：

```js
module.exports = {
  pwa: {
    manifestOptions: {
      short_name: "Title's Album", // 名称缩写
      name: "Title's Album", // 全名
      start_url: "index.html", // 启动页面
      display: "standalone", // 启动过渡动画
      background_color: "#1f212b", // 背景颜色
      theme_color: "#1f212b" // 主题颜色
    },
    iconPaths: {
      favicon32: 'icon.png',
      favicon16: 'icon.png',
      appleTouchIcon: 'icon.png',
      maskIcon: 'icon.png',
      msTileImage: 'icon.png'
    }
  }
}
```

icon.png 就是之后添加到桌面后的图标，放置位置在 public 目录下。

然后在 `public/index.html` 的 `header` 中加入 `<meta name="theme-color" content="#1f212b">`。

然后打包部署就可以了。

然后打开浏览器，找到【添加到主屏幕】（我用的 iOS，安卓应该也有类似的功能）

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-10-IMG_7636.PNG?imageMogr2/thumbnail/!40p)

访问效果：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-10-IMG_7637.PNG?imageMogr2/thumbnail/!40p)

这只是一次 PWA 的简单尝试，实际项目中情况会比这个复杂得多，比如离线缓存，消息推送等。

既然用到了 PWA，我又想到了 Vue 的 SSR，服务器端渲染我也没有接触过，后面有机会我再把项目改造一番。