---
title: Github绑定自定义二级域名
date: 2016-10-05
tags: ['折腾', '学习']
---
网上有很多利用 github pages 搭建个人博客网站并绑定域名的教程，但是有时候也许你想再开一个 github pages 放自己做的 demo ，但是如果不绑定二级域名，这个项目的网址默认是`tit1e.xyz/XXX/XXX.html`，这是我自己的网页 demo 的默认网址形式，这样一来会 url 会比较难看，那怎么把一个二级域名将这个网站的网址解析为 demo.tit1e.xyz 呢？网上查了一些资料，自己已经摸索成功了。下面贴上方法。

我用的是阿里云，所以下面就一阿里云的为例子。
<!--more-->
首先进入 github 的项目主页，切换到你项目文件所在的分支，我的项目是一个网页，所以项目在gh-pages上，然后点击`Create new file`新建一个文件。
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-15-d3407294-8b08-11e6-929d-bc15c3716c5e.jpeg)
名字命名为`CNAME`，内容填上自定义的裸二级域名，注意不要加`http/https`协议。保存。
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-15-d37be4fa-8b08-11e6-8106-bf099bfcd374.jpeg)
然后进入阿里云的控制台，点击已解析的域名进入域名控制台，可以看到你之前解析的域名，点击添加解析，记录类型选择 CNAME，主机记录选择之前新建的CNAME文件中填写的域名的前缀，如我之前填的是`demo.tit1e.xyz`，那么在主机记录只需填demo，记录值则填你的主机IP，然后确认，等待几分钟就可以访问了。时长根据运营商不同也会不一样。
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-15-d3a77728-8b08-11e6-8b0d-068bef2a488f.jpeg)