---
title: alfred 使用 workflows 快速进行图片压缩
date: 2019-04-12 21:29:00
tags: ['mac 技巧']
---

图片压缩是我平时经常用到的操作，一开始是去 [tinypng](<https://tinypng.com/>) 使用网页版压缩，后来在逛 Github 时发现了 [TinyPNG4Mac](<https://github.com/kyleduo/TinyPNG4Mac>) 这个小工具，相比之前的网页操作已经方便了不少，今天在逛 Twitter 时又发现了一个 alfred 的 workflows 工作流，可以在本地快速进行图片压缩。下载地址在 [image Compressor](<https://github.com/BlackwinMin/alfred-gallery/tree/master/image%20Compressor>)。

### 安装

使用这个工具的时候需要先安装以下三个项目：

```bash
# 压缩 png
brew install pngquant
# 压缩 jpg
brew install jpegoptim
# 压缩 gif
brew install gifsicle
```

[文件下载地址](<https://github.com/BlackwinMin/alfred-gallery/blob/master/image%20Compressor/image%20Compressor.alfredworkflow>)

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-12-Xnip2019-04-12_22-24-26.png)

下载完成后打开下载好的文件，点击 import 将工作流导入就安装完成。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-12-Xnip2019-04-12_21-41-31.png)

### 使用

我由于之前没使用 workflows 处理过文件，都是使用热键类型的 workflows，因此我不知道怎么选择文件进行操作，在一通瞎按外加 一通 Google 后终于了解了。一开始我连如何执行这个压缩命令都不知道。

* 对单个文件进行操作

  * 选择一个文件后按 `control` 出现操作选项，执行 `image Compressor`

  ![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-12-Xnip2019-04-12_21-51-34.png)

* 在 Alfred 中选择多个文件后进行操作

  * 选择一个文件后按 `option` + `↑` 可以将文件加入缓冲区，可以将多个文件加入缓冲区， `option` + `←`可以删除最近添加的文件， `option` + `→` 可以对这些选中的文件进行批量操作。

* 在 Finder 中选好文件直接使用 Alfred 操作文件

  * 对于压缩图片来说这应该是最好的操作方式：在 Finder 中批量选中图片后，按 `option` + `commend` + `\` 调出操作列表进行操作。如果无法调出可能是快捷键被修改了，可以从下图出查看具体快捷键。

    ![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-12-Xnip2019-04-12_21-46-08.png)

在找的过程中我还发现一个使用 TinyPNG 压缩的 workflows [Compress with TinyPNG](<http://www.packal.org/workflow/tinypng>)，应该与 TinyPNGMac 用的是同一个接口吧。使用方式同 image Compressor。

 注：使用的时候需要去 [这里](<https://tinypng.com/developers>) 注册一个 API Key，然后在 Alfred 执行 `tinypng_config` ，输入 API Key 后回车。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-12-Xnip2019-04-12_22-17-07.png)

压缩效果有点差异，但相差不多，但是压缩速度差非常多。TinyPNG 还会出现丢图现象，下图右侧其实应该有三张图片，但是压缩只为了我两种张，而且速度非常慢，从修改日期可以看出两张图的创建时间相差了一分钟。image Compressor 由于使用的是本地的工具库，因此几张图压缩只要几秒就可完成。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-12-Xnip2019-04-12_22-21-59.png)