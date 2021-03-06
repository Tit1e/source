---
title: Elementary OS Loki 折腾笔记
date: 2016-09-29
tags: ['折腾']
---
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-15-7515b68a-8689-11e6-9ffe-9e14494271b0.jpeg)
前两天折腾黑苹果，想在自己的小黑上装个黑苹果玩玩，结果没想到折腾了差不多一天多毫无进展，连系统安装界面都没见到，更加不用说之后的安装驱动了。如果一直钻牛角尖，其实代价挺大的，毕竟我现在无业游民一个不是閒的没事干。于是我昨天傍晚的时候彻底放弃了黑苹果计划，还是以后工作了攒钱买个白苹果吧。
<!--more-->
但是我都打算折腾了不搞出点事情我又不甘心，所以我打算把Freya装成Loki，反正好久没有重装了，就折腾一下这个吧。于是轻车熟路刻U盘，装系统，一路下来都没什麽问题，结果在装chrome的时候看了Linux公社的教程，chrome没装上，源还出了问题，网上查了一下还有不少人中招的，查了半天也没查出什麽结果，索性就重装了，反正很快，然后问题就来了，我把备份博客的U盘给插了上去，开机USB啓动不了，而之系统装好时敲好拔U盘时除了一点小问题，我以爲是U盘里的系统被我弄坏了的缘故，又正好赶上要吃饭，我就重新刻了下U盘，吃完饭回来又重装了一遍，然后开始折腾其他的东西，后来打算装博客，安装hexo的时候又出了点问题，于是我想起之前有把命令行存下来，这才想起了备份的U盘，拿起U盘一插，发现容量怎麽少了这麽多，之前U盘里我备份了不少东西，已经快满了，结果现在一看才用了一个多G的空间，点开一看，卧槽爲什麽里面变成
elementary OS的系统了？！当时脑子一片空白，之前已经很小心，装系统前还特意看了备份U盘中的博客，感觉万无一失了才装的系统。

而事实证明:**生活处处有惊喜，这世界上也没有万无一失的事情。**

接下来就是疯狂百度google各种恢复blog的方法，然而并没有，源文件没了就是没了，github上只有public文件夹中的内容，源文件是不上穿的，这也是爲什麽hexo轻巧快速的原因，好在我的另一台电脑种还有几个月前的备份，庆幸幸好没有把这破笔记本扔了。看了一下丢失4篇文章。最后找来找去，把github上的文件打包下载下来，生成的html文件还是在的，所以里面内容也在，于是把html里的多馀标籤什麽都删除，只保留内容部分，再把文件存爲markdown格式，然后本地部署看了一下，还好生成的页面是正常的。折腾到1点总算是恢复了，终于鬆了一口气。

下面是我个人的elementary OS的一些基本配置：

## 安装elementary-tweaks
elementary OS下必备的美化软件
```bash
#更新软件源
sudo apt update

#安装add-apt-repository命令所在的软件包
sudo apt install software-properties-common

#使用add-apt-repository命令添加ppa
sudo add-apt-repository ppa:philip.scott/elementary-tweaks

#再次更新软件源
sudo apt update

#安装elementary-tweaks
sudo apt install elementary-tweaks
```
## 网易云音乐
中毒太深，无法自拔，好在网易良心，出了ubuntu客户端。**网易大法好！**
```bash
#官网下载对应的deb包
#打开终端，cd至deb包所在文件夹
sudo dpkg -i XXXXX.deb

sudo apt-get -f install

sudo dpkg -i XXXXX.deb
```
## 安装并配置fcitx输入法框架
习惯用搜狗，linux下的搜狗还是挺良心的
```bash
#打开终端，更新软件源
sudo apt update
#直接安装需要的输入法包，如fcitx-googlepinyin，fcitx-pinyin，fcitx-table-wubi等，其他基础软件包会自动装上。
sudo apt install fcitx-googlepinyin
#打开“系统设置”、“语言和区域”；点击“解锁”，输入密码；在左边“已安装的语言”中选择Chinese，然后点击“设置系统语言”
#打开“系统设置”、“键盘”、“布局”；把左边的布局都删掉，只保留“英语（美国）”
#重启
```
## 安装chrome
必备，没啥好说的，linux下基本也就chrome、firefox、chromium了
```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

sudo dpkg -i google-chrome-stable_current_amd64.deb

sudo apt-get -f install

sudo dpkg -i google-chrome-stable_current_amd64.deb
#PS、或者在自带的应用中心直接安装chromium
```
## node.js安装
[Hexo](https://hexo.io/)需要，其他有些软件也需要node，必装
```bash
sudo apt install nodejs-legacy
```
## 安装guake终端
刚发现的一款终端，用着挺方便
```bash
apt-get install guake
#然后在系统设置-->Tweaks-->Cerbere中设置开机启动
#呼出快捷键：F12，更多设置可在终端首选项中设置
```
## 安装7z
折腾过程中遇到需要解压7z格式压缩包，记录下
```bash
apt-get install p7zip
#解压
7z x filename.7z
```
## 安装字体管家
```
sudo apt-get install font-manager
```
## git克隆项目
```bash
#在目标文件夹git init
#cd进入项目文件夹
git branch –r显示所有分支

git checkout <目标分支>
```
## 安装sublime-text3以及中文支持
sublime-text3在ubuntu中不支持中文输入，但有解决办法
```bash
#安装sublime
sudo add-apt-repository ppa:webupd8team/sublime-text-3

sudo apt-get update

sudo apt-get install sublime-text-installer

#中文支持，注意需要使用fcitx
sudo apt-get update && sudo apt-get upgrade

git clone https://github.com/lyfeyaj/sublime-text-imfix.git

cd sublime-text-imfix && ./sublime-imfix
#安装完成后启动器中可能会出现两个图标，一个是自带的，一个是后面添加的，以前用该方法只能通过终端输入subl运行才能调用输入法，点击图标需要改图标文件配置后才能调用，现在已经被修复了，所以只要删除自带图标就可以了
#解决办法：
sudo rm /usr/share/applications/sublime_text.desktop
#Ps:新图标的名字为sublime-text.desktop，启动器现实的名字是相同的
```
以上大致就是我的elementary OS的基本配置，至于其他零零碎碎的配置我就不放上来了。