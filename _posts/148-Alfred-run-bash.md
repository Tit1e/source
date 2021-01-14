---
title: Alfred 运行终端命令升级版
date: 2021-01-12 19:59:09
tags: ['折腾', 'mac']
---

之前我写过一篇有关 Alfred 运行命令行的文章：[Mac 使用 Alfred 执行终端命令](https://evolly.one/2019/04/04/62-alfred-workflow/)，用之前的方法日常使用也并没有问题。但是我心里一直有一个不爽的地方，就是按之前的方法执行命令，执行期间会先打开一个终端窗口，然后执行完了终端窗口才会关闭，当然关闭也是自己写的命令，否则执行完窗口会留在桌面。不过有窗口有一个好处就是如果报错了，错误会直接显示在窗口上，如果后台执行，我就不知道到底是为什么执行失败了。但是我依旧还是想要后台运行。

今天在装一个 workflow 的时候我突然发现它可以通过 bash 运行 `.py` 文件，而且它是静默运行的，这我就奇了怪了，能运行 `.py` 那肯定也能运行 `.sh` 啊，不然没道理。然后经过我一番折腾，终于成功静默运行了终端命令。

初始创建命令的步骤我就不再复述，有需要可以去看我之前的文章，里面已经作了说明：[Mac 使用 Alfred 执行终端命令](https://evolly.one/2019/04/04/62-alfred-workflow/)。

创建完之后界面上应该是这个样子，**注意红色方框处**

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-12-122812.png)

按之前的方式，我会在这个方框处右键把它替换成 Terminal Command 模式，而现在，我们不需要改变它，就用 Run Script 模式。

接下来有两种来运行命令：

### 1. 直接运行命令

双击 Run Script 可直接编写命令。不过编写命令前需要做一个准备工作，也是最关键的一个步骤，否则命令是无法执行的。

打开终端，在终端中运行 `echo $PATH`，复制打印出来的一长串字符。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-12-123702.png)

然后开始编辑脚本：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-12-132102.png)

```bash
#!/bin/bash
PATH="替换成你复制出来的字符串"
#在这里你就可以开始编写自己的脚本了
#如果需要传参，那顶部中间的下拉框就选择【width input as {query}】
cd ~/Document/xxx && echo '{query}'
```

保存，然后使用热键处罚就会执行了。

### 2. 编写 .sh脚本文件，然后通过运行脚本文件执行命令

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-12-132151.png)

两种方式没太大区别，，这种方式主要要注意的有两点：

1. .sh 文件的执行权限
2. 参数的传递获取方式

```bash
#!/bin/bash
PATH="替换成你复制出来的字符串"
terminal-notifier -message "$1" -title "Message"
```

上面脚本的作用是会弹出一个通知，标题是 Message，内容是 `$1`，这个 `$1` 就是上面图中运行时传入的 `{query}`。不了解的可以看这里的资料：[Shell 传递参数](https://www.runoob.com/linux/linux-shell-passing-arguments.html)。

在编辑器新建一个文件，输入好脚本，先保存至你能找到的地方。然后打开 workflow 所在文件夹位置。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-12-132256.png)

将保存的脚本文件复制到文件夹中

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-12-130247.png)

然后打开终端，输入 `chmod 777 `，再将脚本文件拖入终端，命令中会自动填入路径，enter。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-12-130345.png)

最终效果：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-12-%E5%B1%8F%E5%B9%95%E5%BD%95%E5%88%B6%202021-01-12%2021.07.56.mov.gif)

## 2021 年 1 月 13 日更新

我在工作中经常用 workflow 做一些项目的打包更新操作，然后我就优化了一下流程，其实主要是记录下 shell 中对 git 状态的一些判断：

#### 判断本地的分支版本跟远程跟踪的版本是否一致

```shell
#!/bin/bash
PATH="替换成你复制出来的字符串"

LOCAL=$(git log 你本地的分支名 -n 1 --pretty=format:"%H")
REMOTE=$(git log remotes/origin/你要比较的分支名 -n 1 --pretty=format:"%H")

if [ $LOCAL = $REMOTE ]
then
	# 一致时候的操作
else
  # 不一致时候的操作
fi
```

#### 判断是否有未暂存的文件

```shell
#!/bin/bash
PATH="替换成你复制出来的字符串"

if [[ -z $(git status -s) ]]
then
	# 文件都已暂存时候的操作
else
	# 有未暂存文件时候的操作
fi
```

#### shell 命令分隔符的区别

直接在linux命令行中可以依次执行多个命令，多个命令间可采用 `;`、`&&` 和 `||` 分割，三个分隔符作用不同：

1. `;` 分割符：前后命令间没有必然的联系，前一个执行结束后、再执行第二个，没有逻辑关联；

2. `&&` 分隔符：前后命令有逻辑关联，后面的命令是否执行取决于前面的命令是否执行成功，前者执行成功，才会执行后面的命令。

3. `||` 分隔符：前后命令有逻辑关联，与 `&&` 相反，前面的命令执行失败后才能执行后面的命令。