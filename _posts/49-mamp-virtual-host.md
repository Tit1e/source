---
title: MAMP 配置虚拟主机
date: 2018-07-22 22:16:31
tags: ['学习', 'mac']
---
公司由于项目调整，在跑本地项目的时候需要指定域名，当然可以通过配置修改这个指定的域名，但是借此机会折腾一下也好。教程网上有很多，我重新整理一下一是加深记忆，二是日后如有需要方便查找，于是有了这篇文章。参考的文章链接在文末。

MAMP 我用的是免费版的，MAMP Pro 好像可以直接在界面设置，但是免费版也是可以实现的，只是设置过程没那么简单明了，但也不是非常难。
<!-- more -->
第一步：打开 `/Applications/MAMP/conf/apache/httpd.conf`

第二步：搜索 `httpd-vhosts`，定位至 `#Include /Applications/MAMP/conf/apache/extra/httpd-vhosts.conf`，删除前面的 `#`，保存。

第三步：打开 `/Applications/MAMP/conf/apache/extra/httpd-vhosts.conf`。这个文件中你可以看到被 `<VirtualHost *:80></VirtualHost>` 标签包裹的代码，我的是这样的，可能略微有些不同，但没关系。

```
<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host2.example.com
    DocumentRoot "/Users/xxxx/Public/Program/dmp"
    ServerName localhost
    ServerAlias localhost
    ErrorLog "logs/dummy-host2.example.com-error_log"
    CustomLog "logs/dummy-host2.example.com-access_log" common
</VirtualHost>
```

这就是我们需要修改的配置。默认应该有个 localhost 的配置（有点忘了）。

第四步：修改 `DocumentRoot` 为这是项目目录，修改 `ServerName` 为你想设定的域名，`ServerAlias` 可以为域名设定别别名，可以不设置。保存文件。

到此 MAMP 的配置结束了，我们还需要配置 hosts 文件，将 127.0.0.1 指向我们设定的域名，不然用域名还是无法访问到。
Mac 上推荐使用 [iHosts](https://itunes.apple.com/cn/app/ihosts-%E7%BC%96%E8%BE%91%E7%A5%9E%E5%99%A8/id1102004240?mt=12) 来设置 hosts，很实用的一个小工具，免费版的足够日常使用了。当然你也可以用其他方式，具体也可以搜索引擎搜索，网上教程一大堆。

在 hosts 文件中加入 `127.0.0.1  配置的域名`，保存。重启服务器，重连一下网络即可。

用 shadowsocks 的小伙伴要注意，要把代理模式设为 PAC自动模式，全局模式会导致域名无法访问，这个问题当初找了好久才发现。

[参考地址](https://blog.csdn.net/it_r00t/article/details/75254933)
