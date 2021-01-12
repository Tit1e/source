---
title: nginx http 重定向至 https
date: 2020-12-30 21:21:12
tags: ['学习', '折腾', 'Nginx']
---

> **超文本传输安全协议**（英语：**H**yper**T**ext **T**ransfer **P**rotocol **S**ecure，缩写：**HTTPS**；常称为HTTP over TLS、HTTP over SSL或HTTP Secure）是一种通过[计算机网络](https://zh.wikipedia.org/wiki/計算機網絡)进行安全通信的[传输协议](https://zh.wikipedia.org/wiki/網路傳輸協定)。HTTPS经由[HTTP](https://zh.wikipedia.org/wiki/HTTP)进行通信，但利用[SSL/TLS](https://zh.wikipedia.org/wiki/传输层安全)来[加密](https://zh.wikipedia.org/wiki/加密)数据包。HTTPS开发的主要目的，是提供对[网站](https://zh.wikipedia.org/wiki/網站)服务器的[身份认证](https://zh.wikipedia.org/wiki/身份验证)，保护交换资料的隐私与[完整性](https://zh.wikipedia.org/wiki/完整性)。——摘自维基百科

https 目前已经愈发普及，所以我也为自己的[摄影网站](https://album.animalcrossing.life/)申请了 https 证书。如何申请证书可以参考我的这篇博客：[为域名申请免费 https 证书](https://evolly.one/2020/12/15/137-freessl/)。

证书申请完了，https 的 Nginx 配置也配置完了，用 https 开头的 URL 也可以访问到网站了，但是发现只有完整打上带 https 的地址才能正确访问到网站，如果用 http 访问，会发现访问的是另一个网站。但平时浏览比如百度谷歌等一些地址时，我们只要输入域名，访问的时候默认访问的就是 https 的网站，哪怕你访问的是 http 的地址，最后也会自动跳转至 https。路由的重定向其实 Nginx 很容易就可以做到，只要加几行配置就可以了。

```nginx
#加上这段配置
server {
    listen 80;
    server_name xxx.xxxxxxx.xxx;
    return 301 https://$server_name$request_uri;
}
#正常的https配置
server {
    listen 443 ssl;
    server_name xxx.xxxxxxx.xxx;
    ssl_certificate /xxx/xx.crt;
    ssl_certificate_key /xxx/xx.key;
    root /xxx/web;
    index index.html;
    location / {
      proxy_pass http://127.0.0.1:8888;
    }
}
```

这段配置我也同步到了[为域名申请免费 https 证书](https://evolly.one/2020/12/15/137-freessl/)。

然后重启 Nginx：

```bash
sudo nginx -s reload
```