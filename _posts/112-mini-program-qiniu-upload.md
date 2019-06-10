---
title: 小程序上传图片至七牛
date: 2019-04-27 00:13:20
tags: ['学习', '小程序']
---

最近天天赶着写小程序，没什么空余时间，所以没怎么写博客，但开发小程序过程中踩了不少坑，到时候可以总结一下。

今天先写下在小程序中如何上传文件至七牛云。

首先去[这里](<https://github.com/gpake/qiniu-wxapp-sdk>)下载小程序上传的 SDK，文件在 `sdk` 目录下。然后在代码中引入就行了。先放上我完整的上传函数

```javascript
uploadImage(){
  		// 引入七牛的上传sdk
      const qiniuUploader = require("../../../../../static/lib/qiniuUploader.js")
      // 调用微信的选择图片 API
      wx.chooseImage({
        success: data => {
          // 图片的路径数组
          let {tempFilePaths} = data
          // 选择成功后显示正在上传的 loading
          wx.showLoading({
            title: '正在上传',
            mask: true
          })
          // 由于是数组，所欲需要循环将每张图片依次上传
          tempFilePaths.map((r,i) => {
            // getQiniu 函数用于获取一些上传的参数，比如 token，服务器地址，绑定的域名等
            getQiniu().then(res => {
              // token
              let uptoken = res.data.val.token
              // 绑定的域名
              let domain = res.data.bucket_url
              // 服务器地址
              let uploadURL = res.data.up_server
              // 开始上传
              qiniuUploader.upload(
                r, //上传的图片
                (req) => {  //回调 success
                  // code...
                }, (error) => { //回调 fail
                  console.log('error: ' + error);
                },
                {
                  // region 要根据你获取到或已知的的上传服务器地址去对应相应的地区，对应表可见下文或 Github 的 README.md
                  region: 'ECN',
                  uptoken: uptoken,
                  uploadURL: uploadURL,
                  domain: domain,
                })
            })
            // 最后一张上传完成后关闭 loading
            if(i === tempFilePaths.length){
              wx.hideLoading()
            }
          })
        }
      })
    },
```

| 存储区域 | 区域代码 | HTTPS 地址                |
| -------- | -------- | ------------------------- |
| 华东     | ECN      | https://up.qiniup.com     |
| 华北     | NCN      | https://up-z1.qiniup.com  |
| 华南     | SCN      | https://up-z2.qiniup.com  |
| 北美     | NA       | https://up-na0.qiniup.com |
| 新加坡   | ASG      | https://up-as0.qiniup.com |