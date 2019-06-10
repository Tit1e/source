---
title: hexo rss 报错解决方案
date: 2019-03-30 09:24:37
tags: ['折腾']
---

这几天一直在折腾博客主题，原来用的 [hexo-theme-apollo](<https://github.com/pinggod/hexo-theme-apollo>) 去看了下作者已经停止维护了，apollo 主题本身已经是一个比较简洁的主题了，配色来源于 [Vue](<https://cn.vuejs.org/>) 的官网，但看久了还是觉得审美疲劳，我有个关注的博主 [ky0n](<https://ky0n.xyz/>) 的博客主题我挺喜欢的，非常简洁，所以我就想照着他的主题修改一个，但改的过程中发现，他主题里面的 class 命名，dom结构跟 apollo 的非常相似，所以我猜他可能也是在 apollo 基础上改的。

改样式过程很顺利，但这次我想把 RSS 功能给放出来，这就出现问题了，RSS 页面出现了错误：

```
This page contains the following errors:
error on line 264 at column 238: PCDATA invalid Char value 8
Below is a rendering of the page up to the first error.
```

这个问题其实是由于文章中的隐藏字符 `^H` 导致的，这个字符在 VS Code 跟 Typora 下并不会显示，但在 vim 下会显示。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-03-30-%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-03-28%2023.45.15.png)

### 处理方式

用编辑器打开 `/public/atom.xml`，定位到报错的行，看看报错是在哪篇文章中，然后用 vim 打开这篇文章，查看有没有 删掉文章里的 `^H` 字符，当然保险起见你可以过一遍所有文章，然后再执行 `hexo g && hexo d` 部署到线上，就不会有错误信息了。



[参考文章](<https://www.leejia.me/code/id_rss_vscode_bug.html>)