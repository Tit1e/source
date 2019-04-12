---
title: Mac 使用终端执行弹出式消息通知
date: 2019-04-08 20:29:12
tags: ['mac']
---

### 安装 terminal-notifier

```
// 不能直接执行 sudo gem install terminal-notifier，安装会报错，/usr/bin 目录不允许写入，因此得安装至 /usr/local/bin 目录下
sudo gem install terminal-notifier -n /usr/local/bin
```

### 执行

```
terminal-notifier -message "Hello, this is my message" -title "Message Title"
```

[参考文章](<http://landcareweb.com/questions/3689/ru-he-zhi-zuo-maczhong-duan-dan-chu-jing-bao-applescriptde>)

