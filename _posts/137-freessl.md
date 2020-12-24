---
title: 为域名申请免费 https 证书
date: 2020-12-15 20:55:58
tags: ['折腾']
---

今天五六月份上个服务器到期后，一直没有开始折腾新的服务器，只是安装了一下环境：node，Nginx，Mysql之类，很多服务都没有跑起来，今天终于打算把它重新捡起来，目前服务都基本已经恢复，进行得还算顺利，趁热打铁，把蒸腾的一些经过记录一下，其实上次折腾的时候我就打算记录，无奈拖太久，已经回忆不起来了，只能作罢。

服务器基础的一些基础环境的安装之前已经写过一篇博客，就不再展开，要了解可以[点击这里](https://evolly.one/2018/12/14/51-ubuntu-node/)。

域名的购买备案我都在阿里云解决，此处不再展开。今天要讲的是为域名申请免费的 https 证书，因为这个域名之前是为了给小程序写接口申请的，小程序必须使用域名并且要备案，使用 https 访问。

我是在[freessl](https://freessl.cn/)申请的域名，并且申请的是**多域名通配符**类型的证书，这样我就不用给每个二级域名都申请一个证书。不过这种证书有效期比较短，只有三个月，过期后怎么处理目前我还不确定，只能到时候再看。

进入网站后先登录，然后在输入框中输入域名，我需要多域名所以我是这样写的（拿百度举例）：*.baidu.com,baidu.com

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-15-131207.png)

然后网站会让你下载一个应用程序，叫 keyManager。设置好开启密码后，会先让你授权域名运营商，我用的阿里云，它本身就支持，所以我填入两个 key 后马上就授权成功了，后面就是申请，过程很简单，完成后需要你去域名解析那添加一个 TXT 类型的解析，解析内容软件会提供。

解析完了之后，导出证书，我用的 Nginx 那我就选择 Nginx。下载后是一个压缩包，解压后得到两个文件，一个扩展名为`.crt`，另一个为`.key`。这两个文件到时候需要上传到服务器中，Nginx配置中需要用到。

Nginx 的配置文件大致如下

```json
// xxxx.conf
// 重定向，http 重定向为 https
server {
    listen 80;
    server_name 域名;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl;
    server_name 域名;
    ssl_certificate /path/文件名.crt; //存放文件的路径
    ssl_certificate_key /path/文件名.key; //存放文件的路径
    root /home/ubuntu/www/animal-crossing-home; //项目根目录
    index index.html; // 默认文件名
    location / {
      proxy_pass http://127.0.0.1:5000; //本地服务地址加端口
    }
}

```

然后重启Nginx

```bash
sudo nginx -s reload
```

这时，你访问 http 的域名，就会被重定向到 https，chrome 地址前不出意外的话，也应该会显示一把小锁。

域名三个月后过期如何处理我到时候会再写一篇博客进行说明。