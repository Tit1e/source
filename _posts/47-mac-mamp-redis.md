---
title: Mac 下为 MAMP (Pro) 安装 redis 扩展
date: 2018-05-12 15:04:42
tags: [折腾', 'Mac']
---
Mac 用户一般开本地服务器环境的时候用得比较多的就是 [MAMP](https://www.mamp.info/en/) 这款软件吧，指定目录，一键开启，甚至可以什么都不用配置就可以将服务跑起来。不过前阵子开发的时候，由于公司项目用了 [redis](https://redis.io/) ，但 MAMP 本身的 php 是不带有 redis 插件的，为了装这个东西，我折腾了好几个小时，所以在这里记录一下。
<!--more-->
### 下载 & 解压 && 编译 PHP 源码
**教程中将以 php 5.5.30 版本作为例子，实际操作中请将版本号做相应替换**

为方便下载，我在这放上下载地址，我这里的地址是 5.5.30 的 php 版本，如果需要其他版本，只需将地址中的版本号进行相应替换。
下载地址： [http://us3.php.net/get/php-5.5.30.tar.gz/from/a/mirror](http://us3.php.net/get/php-5.5.30.tar.gz/from/a/mirror)

下载完成后，将源码解压至 `/applications/MAMP/bin/php/php5.5.30/include/php`

cd 至源码目录，并执行：
```
cd /applications/MAMP/bin/php/php5.5.30/include/php

./configure
```
当前目录下继续操作：
```
git clone https://github.com/nicolasff/phpredis.git

cd phpredis
```
接下来就是进行编译，但为了确保期间，建议在执行下列操作前先执行，除非你很确定你已经安装 `autoconf`
通过 [Homebrew](https://brew.sh/index_zh-cn) 安装

`brew install autoconf`

下面进行编译
注：此时的目录应该为`/applications/MAMP/bin/php/php5.5.30/include/php/phpredis`

执行下列命令：
```
/Applications/MAMP/bin/php/php5.5.30/bin/phpize

./configure --with-php-config=/Applications/MAMP/bin/php/php5.5.30/bin/php-config

make
```
所有命令执行完成之后，会在当前目录下生成一个 `modules` 文件夹，里面有 `redis.so`，将这个文件复制至 `/Applications/MAMP/bin/php/php5.5.30/lib/php/extensions/no-debug-non-zts-XXXXXXXX`（最后的文件夹名每个用户有所差别）。

最后，修改 `/Applications/MAMP/bin/php/php5.3.30/conf/php.ini`,在文件的 545 行加上 `extension=redis.so`，保存关闭，重启服务器。

到此，redis 扩展安装完毕。

### 安装 redis

安装 redis 就很简单了，一句命令行搞定：
```
brew install redis
```
