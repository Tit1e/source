---
title: Mysql 报错：Data too long for column
date: 2019-07-05 15:07:41
tags: ['学习']
---

前阵子写了一个 [Js-Practice](https://github.com/Tit1e/Js-Practice) 项目，由于是纯前端页面，所以所有的题目都是写死在文件中的，导致题目更新后，代码很容易出现冲突，所以我就想着把题目存到数据库去，用 nodejs 写一个服务端，因为正好有一台空闲着的服务器，顺便学习一下 nodejs。

项目中数据库用的 Mysql，在使用过程中，本地都没什么问题，但是到了线上，就有问题了。在执行某条 sql 的时候，数据库抛出这样一个错误：

> Data too long for column 'XXXX' at row 1

网上找了一圈发现了解决方法：

> 首先连接数据库：
>
> mysql -h 127.0.0.1 -u root -p 123;
>
> 然后执行：
>
> SET @@global.sql_mode= '';
>
> 最后退出：
>
> quit

之后就可以正常运行 sql 了。

这个问题好像是因为 Mysql 使用严格模式导致的，严格模式下，Mysql 会截断过长的插入值，所以导致了报错，上面的命令就是关闭了 Mysql 的严格模式。

[参考](http://huanyouchen.github.io/2018/05/22/mysql-error-1406-Data-too-long-for-column/)