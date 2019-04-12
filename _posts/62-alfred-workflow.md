---
title: Mac 使用 Alfred 执行终端命令
date: 2019-04-04 21:05:45
tags: [mac,折腾笔记]
---
<!-- more -->
Mac 上有个叫 Alfred 的软件，堪称神器。想了解的可以去网上搜搜，一大把，这软件从来不缺人夸。

其实我平时用到这个软件的功能才一小部分，但是已经给我带来了极大的便利，现在我已经无法习惯没有它的电脑。

之前用的一些搜索，计算器，打开软件这些用途其实 Mac 自带的聚焦也可以做到，但是 Alfred 特有的 Workflow 工作流聚焦可没有。Workflow 能做很多事情，我还是贴个少数派的 Workflow 专区 [链接](<https://sspai.com/tag/Alfred>) 吧，里面有很多关于它的介绍及使用技巧。

我今天要说的是用它来执行终端命令，因为工作中终端是必不可少的东西，其他比如还有 hexo 博客的部署什么的也是通过命令行，但是一般情况下终端都需要一连串的 cd 进入目录，然后执行命令，当次数多了就开始觉得好烦，Workflow 不是正好可以执行脚本么，于是就去淘宝买了个授权，不知道是官方给的批量授权还是申请来的测试授权码，反正还是花了我 100+ 大洋，之前一直用的免费版，无法使用 Workflow。

### 不是必要的准备工作

Alfred 默认用的是 Mac 自带的终端，如如果使用的 iTerm 可以根据下图设置：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-04-Xnip2019-04-04_21-41-25.png)

输入的代码为

```
on alfred_script(q)
    tell application "iTerm"
        set _length to count window
    if _length = 0 then
        create window with default profile
    end if
    set aa to (get miniaturized of current window)
    if aa then
        set miniaturized of current window to false
    end if
    set bb to (get visible of current window)
    if bb is false then
        set visible of current window to true
    end if
    set cc to frontmost
    if cc is false then
        activate
    end if
        (*if _length = 0 then*)
            set theResult to current tab of current window
        (*else
            set theResult to (create tab with default profile) of current window
        end if*)
        write session of theResult text q
end tell
end alfred_script
```

这样 Alfred 在执行命令是就会调用 iTerm。

### 编写脚本

先添加一个初试模板：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-04-Xnip2019-04-04_21-44-57.png)

填写 Workflow 的名称，也可以将图片拖到右侧框内，即可当作图标：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-04-Xnip2019-04-04_21-46-51.png)

双击左侧的模块设置触发关键词：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-04-134948.png)

在左侧输入框输入触发的关键词，右侧的第一个选项是还可以输入其他信息，并且在执行脚本的时候可以通过 `{query}` 获取到输入内容，第二个我没了解过，第三个就是输入完关键字 回车直接触发。我以部署博客为例编写两个 Workflow。第一个是新建一篇文章，第二个是一件部署。这个例子就写生成新文章的。因为生成新文章需要输入文件名（当然不填其实也有默认的名字），因此需要选择第一项。下方的 Title 是 必填项。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-04-135110.png)

之后编辑右边的模块：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-04-135912.png)

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-04-140100.png)

输入你想执行的命令并保存就可以了。我这里的命令行执行的分别是，先 cd 至博客目录，然后执行 新建文章的命令，然后打开 Typora，我的 Typora 目录就是文章目录。最后退出终端。因为我 iTerm 设置使用快捷键从顶部呼出，但 Workflow 执行的时候会打开一下终端的窗口，因此执行的时候并不后影响我原有的窗口，所以我用了 exit 命令，执行完毕直接退出终端，不然的话每次执行都会有一个终端窗口遗留，会很乱。

执行时只要呼出 Alfred  然后输入之前设置的关键字，再输入文件名就可以了。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2019-04-04-140726.png)

部署的话，也差不多，就是在设置关键字的时候选择 No Argument 项就可以了，然后在脚本部分写：

```
cd ~/Public/Myblog && hexo g && hexo d && exit
```

 完。