---
title: 将本地某个文件夹备份或同步至群晖
date: 2021-01-11 20:33:28
tags: ['nas', '折腾']
---

在最近一次时间机器备份的时候，突然想到我电脑中的 Lightroom 的备份，用时间机器备份不现实，由于我有一个群晖，就想着用它应该可以实现备份，毕竟这不是本来就是群晖的一个卖点么？

经过一番还算顺利的折腾之后，功能实现了，但是我的 Lightroom目录没了，也就是之前调好的照片配置全没了，我只把之前的照片导出备份了一份，还是有点心痛，我之前整理可是花了好大力气。

下面是经过：

### 1. 建立备份空间，开启共性

首先是分配一个空间，这个使用群晖的肯定都知道，不然东西往哪存？

然后打开 **Synology Drive 管理控制台**，左侧切换到 **团队文件夹** 标签，选择你要开启共享的目录，然后点击上方的【启用】按钮。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-11-Xnip2021-01-10_17-04-37.png)

### 2. 下载安装 Synology Drive 客户端

接下来去群晖官方的[**下载中心**](https://www.synology.cn/zh-cn/support/download/DS218play#utilities)下载**与你群晖型号匹配的 Synology Drive Client 客户端**，下载后安装。

### 3. 设置

我用的的是 Mac，所以我用 Mac 版的客户端做讲解，Windows的版本应该也差不多。

运行软件，点击【立即开始】后会出现任务类型选择界面：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-11-Xnip2021-01-10_16-29-14.png)

以我的理解，这两种任务的区别在于：

* 同步任务是双向的，你在群晖添加或删除文件，会同步至你本地电脑，你本地电脑删除文件群晖上也会删除相应文件，就像各种在线网盘的共享文件夹。

* 而备份任务是单向的，群晖只会把你本地硬盘的文件同步至群晖，同步完后，你在群晖上删除文件也不会影响你本地的文件。

我选择的是同步任务。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-11-Xnip2021-01-10_16-29-45.png)

软件会要求你输入地址跟账号密码，因为我自己只打算定期在家里（还不是群晖官方的速度太慢了）备份下照片，所以我用的局域网地址。点击地址右侧的放大镜会自动搜索局域网内的设备。当然你也可以输入 `quickconnect.cn` 域名的地址，白群晖应该每人都有一个，但速度感人。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-11-Xnip2021-01-10_16-29-57.png)

我就不用 quickconnect 了，点击【现在不要】。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-11-Xnip2021-01-10_16-30-07.png)

这个警告可能是因为我用的局域网地址，局域网地址是 http 的，而我输账号密码的地方又勾选了【启用 SSL 数据传输加密】导致的，点击【仍然继续】。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-11-Xnip2021-01-10_16-30-17.png)

然后就到了配置同步文件夹的步骤，鼠标放上去在右侧会显示编辑按钮，分别选择本地与群晖上要同步的目录。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-11-Xnip2021-01-10_16-31-06.png)

选择本地文件夹的时候有个要注意的点，就是那个复选框的项，如果勾选了【创建空 SynologyDrive 文件夹】，那么按我图上的配置来说，最后它会在我的 Lightroom Photos 文件夹下再创建一个 SynologyDrive 文件夹，放到这个文件夹中的文件才会被同步，而我想要的是同步 Lightroom Photos 文件夹中的文件，所以我在选择的时候需要把这个勾去掉。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-11-Xnip2021-01-10_16-31-28.png)

这个好像是共同协作用的，我也没怎么研究，感觉用不到，就选择了【稍后再说】，点击【完成】。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-11-Xnip2021-01-10_16-31-46.png)

然后任务列表就出现了刚添加的任务，稍等一会儿就会发现在软件正在同步了。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2021-01-11-132134.png)
