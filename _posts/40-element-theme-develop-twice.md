---
title: element-theme 配合 vue-cli 进行二次开发
date: 2017-12-20 15:38:03
tags: ['学习', 'vue']
---
公司新平台我打算使用`element-ui`和`vue-cli`来开发，既然用的是现成的ui，那么就涉及到ui的定制问题，虽然是一个后台项目，并且让设计参照`element-ui`的文档去作图，但是设计不可能照搬默认主题，不在组件功能上修改，但在主题配色上肯定会进行调整。虽然官方有 [在线主题生成工具](https://elementui.github.io/theme-chalk-preview/#/zh-CN) ，但这个工具只能修改一个整体的主题色，连按钮的颜色都无法定制，更不用说定制圆角阴影这些效果了，所以还是要自己改文件编译才能实现最大化定制。折腾过程中遇到了点问题，在此记录一下。

官方也提供了定制的 [文档](https://github.com/ElementUI/element-theme) ，那当然是相信官方，照着官方的教程来！
<!--more-->
#### 安装`element-theme`
```bash
#只在当前项目安装
npm i element-theme -D

#全局安装
sudo npm i element-theme -g
```
然而我第一步就报错了……ORZ。

我使用的是`npm`命令，但源用的是淘宝的源。但安装的时候报错：`not found python2`。于是我去找安装`python2`的方法，结果发现 Mac 是默认集成了`python`的，那我就郁闷了，试了半天，一直报错，最后突发奇想想用`cnpm`试试，结果竟然成功了。
```bash
#终于装上了
sudo cnpm i element-theme -g
```
好，继续。

#### 安装 `theme-chalk`
因为我一开始构思的定制流程是在其他目录下把样式写好编译后到现有项目中替换，所以我就建了一个空文件夹，然后执行命令：
```bash
npm i element-theme-chalk -D
```
结果当然是提醒我找不到`package.json`文件。后来我望着命令中的`-D`若有所思……，终于反应过来了，这是装在项目中的。

好，顺利安装，继续。

#### 初始化
```bash
#初始化
et -i
```
执行完后会在根目录下生成一个`element-variables.scss`的文件。然后你就可以在这个文件中修改主题色，按钮颜色，圆角，阴影等这些样式，修改好了之后，就可以编译了。
#### 编译
```bash
# 监听并编译
et --watch [--config variable file path] [--out theme path]

# 编译
et [--config variable file path] [--out theme path] [--minimize]
```
`et --watch`可以监听`element-variables.scss`文件。如果文件被修改了，那么就会自动进行编译。

`et --config 文件路径/element-variables.scss`这个命令是可以指定`element-variables.scss`的路径，因为这个文件不一定放在项目根目录下。

`et --out 目录`这个命令是指定编译后的文件输出目录，默认是输出在根目录下的`theme`文件夹中。

但这就会产生一个文件替换问题，每次编译后都需要手动替换文件，之后才能看效果。所以我直接把输出路径改成了项目引用的`element-ui`的路径：`node_modules/element-ui/lib/theme-chalk/`，这样配合`-w`就可以实现实时编译，再配合`npm run dev`就可以实时编译实时预览了。

如果还需要更深度的定制，则修改 `node_modules/element-theme-chalk/src/` 目录下的的`.scss`文件，然后执行编译后就能生效。注意直接修改`.scss`文件不会触发自动编译。
