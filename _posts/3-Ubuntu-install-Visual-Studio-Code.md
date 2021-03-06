---
title: Ubuntu下安装 Visual Studio Code
date: 2016-03-14
tags: ['折腾']
---
昨天在写eOS的使用体验的时候，无意间发现VS Code在linux可以用，于是写完博客后就立马去安装VSC，但是照着网上的教程，安装过程中还是遇到了不少的问题。所以我总结了下，希望能帮到有需要的人。

**我的系统是elementary OS，不过是基于Ubuntu的，所以安装方法相同**
<!--more-->
**Ubuntu Make安装Visual Studio Code**
首先使用PPA方式安装ubuntu-make
```bash
sudo add-apt-repository ppa:ubuntu-desktop/ubuntu-make
sudo apt-get update
sudo apt-get install ubuntu-make
umake web visual-studio-code
```
第四步安装过程中会让你确认两次，第一次是让你确认安装目录，你可以随意更改，第二次的时候输入a，回车就行，之后就等待下载安装完成。如果运气好的话，安装按成后啓动器里应该会出现VSC的图标。
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-15-8984851a-e90e-11e5-8979-cc6a31b36e00.png)
但如果你的脸也和我一样黑，没出现的话，没关系，进入`/home/tit1e/.local/share/applications/`（系统有区别路径也可能不一样，找不到可以直接搜文件夹），你会发现这里是有图标的，你可以试着运行它，我发现我无法运行。用编辑器打开它，你会看到如下代码：
```
	[Desktop Entry]
	Version=1.0
	Type=Application
	Name=Visual Studio Code
	Icon=/home/tit1e/visual-studio-code/resources/app/resources/linux/code.png
	Exec=/home/tit1e/visual-studio-code/Code //把Code改成你安装目录下这个文件的名字，我目录下的名字是code，文件名是分大小写的哦
	Comment=Visual Studio focused on modern web and cloud
	Categories=Development;IDE;
	Terminal=false
```
这时候你再试着运行试试，你会发现可以打开了。

至于啓动器的图标无法显示，你把这个图标剪切到其他目录下，在剪切回来就会显示了。以上是我的安装方法。只是这样安装的VSC不出现在右键运行方式的列表里，所以打开文档只能通过VSC内部的open打开。

**下载压缩包安装Visual Studio Code**

首先去[官网](https://code.visualstudio.com/)下载VSC的zip压缩包，下载下来后解压到你想安装的目录。

然后在啓动器目录下创建visualstudiocode.desktop文件，名字可以随意，就是啓动其中显示的名字。

用编辑器打开这个文件，添加如下代码
```
	[Desktop Entry]
	Version=1.0
	Type=Application
	Name=Visual Studio Code
	Icon=/home/tit1e/visual-studio-code/resources/app/resources/linux/code.png  //图标路径，在解压的文件夹下的/resources/app/resources/linux/目录下
	Exec=/home/tit1e/visual-studio-code/Code  //解压目录下code文件的路径
	Comment=Visual Studio focused on modern web and cloud
	Categories=Development;IDE;
	Terminal=false
```
接下来保存退出文件，文件夹中就多了一个VSC图标了，如果图标不显示，同样剪切到其他目录再剪切回来就OK。

请尽情使用吧：)

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-15-9a1c261c-e90e-11e5-8b88-e1ca351ca37c.png)
