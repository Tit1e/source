---
title: 从零开始：Ubuntu 部署 Node 环境
date: 2018-12-14 20:29:48
tags: ['折腾']
---
最近注册了一个亚马逊的 AWS 账号，有一年的免费体验时间，于是打算这次机会来折腾折腾服务器方面的东西，顺便学学 Node。折腾过程中遇到了挺多问题，我都会一一记录下来。

这次记录的是我在 Ubuntu 下安装了 mysql 时，没提醒我输入密码，Google 了一圈解决，在这里做个记录。

<!-- more -->

……

……

……

本来这篇文章叫做“Ubuntu 安装 mysql 未提示输入密码”，但在我想去服务器上执行命令验证下命令是否正确的时候，发现 AWS 连不上了……然后我折腾了一个多小时，放弃了原来的服务器，重新新建了一个实例，又从头到尾折腾了一遍，所以这篇文章也正式更名为“从零开始：Ubuntu 部署 Node 环境”。

### 基本说明 ###
- 服务器：[Amazon Web Services](https://aws.amazon.com/cn/) EC2（新用户免费体验 1 年）
- 服务器系统：Ubuntu 16.04.5 LTS
- 操作环境：mac OS Mojave 10.14.2
- 终端：iterm2

### 过程 ###

#### 一、创建实例 ####
1、登录控制台，开始创建
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-14-Xnip2018-12-14_23-35-09.png)
2、选择系统（免费用户只能选择带有“符合条件的免费套餐”字样的项）
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-14-Xnip2018-12-14_23-37-31.png)
3、选择实例类型
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-14-Xnip2018-12-14_23-37-57.png)
4、启动
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-14-Xnip2018-12-14_23-38-26.png)
5、创建密钥
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-14-Xnip2018-12-14_23-41-35.png)

密钥需要保管好，因为通过 SSH 连接服务器的时候需要用到。我将密钥存在 `~/.ssh/` 路径下。

注：新建的实例默认情况下是 ping 不通的，因为它默认只允许 SSH 流量，需要设置一下
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-14-Xnip2018-12-15_01-36-33.png)

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-14-Xnip2018-12-15_01-39-26.png)

#### 二、SSH 连接服务器 ####
打开终端，输入命令：
``` shell
# ip 或 公有 DNS 可在创建实例的第一步图中的列表中找到
sudo ssh -i ~/.ssh/zyd.pem ubuntu@[ip 或 公有 DNS]
```
#### 三、安装 node.js ####
Ubuntu 下直接执行 `sudo apt install nodejs` 安装的 node 版本回会非常低，所以不推荐直接用这个命令安装。
``` shell
#更新软件
sudo apt update
sudo apt upgrade
#添加 node 源
curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
#安装
sudo apt install -y nodejs
```
安装完后安装 node 版本管理工具，我用的是 n ：
```
sudo npm install n -g
```
n 的使用方法可参考 [这里](https://www.npmjs.com/package/n)。

#### 四、Nginx 服务器代理设置 ####
1、安装 Nginx，安装完通过 `nginx -v` 查看版本号来看是否安装成功
```
sudo apt install nginx
nginx -v
```
2、配置代理
``` shell
cd /etc/nginx/conf.d/
#文件名自定义
sudo vi test-3000.conf
```
按 `i` 进入 vim 的编辑模式，粘贴如下内容：

粘贴前把注释内容删除
``` shell
#这里需要对应域名
upstream test {
    #端口需要根据之后 node 服务运行后的实际端口来配置，Express 的默认端口为 3000
    server 127.0.0.1:3000;
}
server {
    listen 80;
    #绑定的完整域名
    server_name test.evolly.one;

    location / {
        proxy_set_header Host  $http_host;
        proxy_set_header X-Real-IP  $remote_addr;  
        proxy_set_header X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_set_header X-Nginx-proxy true;
        #这里需要对应域名
        proxy_pass http://test;
        proxy_redirect off;
    }
}
```
按 `esc` 并直接在用键盘输出 `:wq` ，回车，保存并关闭文件。终端输入 `ls` 可查看当前目录下的文件列表。

将域名解析至服务器，我的域名是在 GoDaddy 上买的，域名解析都差不多，添加一条 A 类型的解析。
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-14-Xnip2018-12-15_00-30-50.png)

然后输入运行 `sudo nginx -t` 查看是否配置成功
成功终端会打印如下信息：
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
重启 nginx
``` shell
sudo nginx -s reload
```

#### 五、创建和部署 hello world ####
到根目录创建 `www` 目录并进入目录
``` shell
cd /
sudo mkdir www
cd www
```
创建 hello.js
``` shell
sudo vi hello.js
```
写入以下内容：
``` js
const http = require('http')
http.createServer(function(req,res) {
res.writeHead(200,{'Content-Type':'text/plain'})
res.end('hello world')
}).listen(3000) //端口对应之前 nginx 配置中配置的端口

console.log('server test')
```

运行
``` shell
node hello.js
```
现在访问之前解析的域名，就能在页面上看到 `hello world` 字样。
那么有个问题，现在 node 服务是启动了，但是终端被占用了，无法执行其他操作了。这时候我们就需要 pm2，pm2 是 node 进程管理工具，有了它，node 就不会占用你的终端了，当然它的功能不止只一个，还有如性能监控、自动重启、负载均衡等功能。

安装
``` shell
sudo npm install -g pm2
```
运行 node 服务
``` shell
pm2 start hello.js
```
如果你使用了框架，如 Express，那你可以 cd 至项目跟目录，运行 `pm2 start './bin/WWW'`。或者你可以看 `package.json` 中运行项目的实际命令是什么。

运行后可以看到终端打印出了所有进程状态，并且终端没有被占用。pm2 的其他用法可以 [参考这里](https://www.npmjs.com/package/pm2)

#### 六、安装 mysql ####
1、执行安装命令：
``` shell
sudo apt-get install mysql-server
sudo apt install mysql-client
sudo apt install libmysqlclient-dev
```
正常情况下安装过程中会让你输入两次初始密码

通过 `sudo netstat -tap | grep mysql` 测试是否安装成功，如果成功则显示如下：
![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2018-12-14-Xnip2018-12-15_01-05-52.png)

2、设置 mysql 允许远程访问，否则数据库工具无法远程连接
编辑文件
``` shell
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```
注释掉 bind-address = 127.0.0.1 行（最前面加`#`）

保存退出

连接 mysql,执行以下命令后，输入密码
``` shell
mysql -uroot -p
```
执行授权命令并退出（命令中的 `；` 也要复制）：
``` sql
grant all on *.* to root@'%' identified by '你的密码' with grant option;
flush privileges;
quit
```
重启 mysql
``` shell
sudo service mysql restart
```
现在你就可以通过 Navicat 工具连接 mysql 了。

#### mysql 安装过程中没有提示输入初始密码 ####
如果遇到这种情况，可以打开该文件查看默认密码
``` shell
sudo vim /etc/mysql/debian.cnf
```
在这个文件里面有着 MySQL 默认的用户名和用户密码
正常情况下该文件中的 `user` 为 `debian-sys-maint`，而密码是由比较复杂的大小写数字构成，我们先通过这个账户连接 mysql,然后执行修改密码命令给 root 用户修改密码：
``` shell
mysql -u debian-sys-maint -p 
```
输入密码后连接 mysql ,执行：
``` sql
update mysql.user set authentication_string=password('重置的密码') where user='root'and Host = 'localhost';
```
如果显示以下内容则说明修改成功：
```
Query OK, 1 row affected, 1 warning (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 1
```
重启 mysql
``` shell
sudo service mysql restart
```
这样 root 用户的密码就被重置了。

教程到此结束，最后感谢下面这几位作者提供的参考：
- [nodejs 服务器部署教程](https://segmentfault.com/a/1190000010098126)
- [在 Ubuntu16.04 下安装 mysql](https://blog.csdn.net/xiangwanpeng/article/details/54562362)
- [ubuntu 安装 mysql 时未提示输入密码](https://blog.csdn.net/sinat_21302587/article/details/76870457)
