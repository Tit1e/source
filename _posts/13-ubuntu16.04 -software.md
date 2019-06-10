---
title: ubuntu16.04网易云托盘图标显示出做及微信应用分享
date: 2016-10-06
tags: ['折腾']
---
今天无意间打开贴吧发现之前在elementary OS吧发的关于网易云托盘图标的求助帖有了回复,试了一下发现问题解决了,顺便还发现了一个第三方微信应用,虽然是基于网页版的,但是这个版本是有托盘的，所以拿出来分享给大家。

先说云音乐问题：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-15-badbf2f2-8c0a-11e6-8102-213021f310f1.jpeg)
这是我之前网易云的托盘图标，无法正常显示，贴吧也有不少吧友遇到和我一样的问题。
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-15-b6aa1920-8c0a-11e6-9952-e7c47b3240e5.jpeg)
这是修复后的托盘，小问题还是有，但至少使用上是没问题了，这样直接看也看不出什么，点击的时候有点小问题。
<!--more-->
下面贴上方法：
```bash
#编辑云音乐的.desktop文件（编辑工具根据实际情况而定）
sudo vi /usr/share/applications/netease-cloud-music.desktop
#在原来的Exec那行的前面加上
env XDG_CURRENT_DESKTOP=Unity
#比如：然后把原来的Exec那行：
Exec=netease-cloud-music %U
#修改为：
Exec=env XDG_CURRENT_DESKTOP=Unity netease-cloud-music %U
#保存关闭编辑器，再打开就会发现托盘能正常使用了。
```
下面说我分享的微信应用：
[Electronic WeChat](https://github.com/geeeeeeeeek/electronic-wechat) 是基于[Electron](https://github.com/electron/electron) 开发的应用，Electron应用我一直在用，但不知道微信也有，今天看到回复我云音乐的吧友回复我才知道这么个应用。

这是官方的图：
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-05-10-144711.png)
安装很简单：
```bash
git clone https://github.com/geeeeeeeeek/electronic-wechat.git
cd electronic-wechat
npm install && npm start(这一步如果出错就把clone下来的文件夹删除后从头执行一遍)
#也可以下载压缩包解压然后运行里面的程序，不过我用这种方法微信无法在托盘显示
```
更多Electronic小应用可以去 [这里](http://electron.atom.io/apps/) 下载，还是有不少有意思的应用。