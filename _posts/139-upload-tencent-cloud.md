---
title: js 上传文件至腾讯云对象存储
date: 2020-12-19 22:43:25
tags: ['折腾', '学习']
---

今天终于把自己的摄影展示网站发布到了线上。[查看网站](https://album.animalcrossing.life)

这个网站整体并不复杂，但是其中也有不少值得记录的难点，比如从前端上传文件至腾讯云的对象存储，公司业务中我使用过七牛的云存储，腾讯的还从未接触过。本以为应该跟七牛的差不多，但没想到我费了好大的力气各种看文档、看别人的博客才成功上传。

吐槽一句腾讯的文档写的真的好差。

下面分享一下我的上传配置及流程：

整个流程需要前端和后端的配合，所以代码会分为前端代码与后端代码两个部分，下面我会注明。后端我用的是 Node.js。

## 上传流程说明

上传的整体流程大概是这样：

1、上传前，前端发起请求向服务器发起请求获取上传的临时密钥

2、服务器端收到请求，通过腾讯官方的 sdk 计算出临时密钥并返回给前端

3、前端获取到临时密钥，获取选择的文件，计算 md5 值作为文件名（这样相同文件就不会重复上传）

4、通过官方的 sdk 进行上传，在回调中处理上传后的逻辑

##后端代码 Node.js

基础的服务运行环境我就不做展开了，只讲获取临时密钥的过程。

首先需要安装 qcloud-cos-sts 依赖：

```bash
npm install qcloud-cos-sts --save
```

然后就是在你请求的方法文件中编写代码，个人视不同请求而定：

```js
// 这是我的文件 upload.js

// 引入依赖
const STS = require('qcloud-cos-sts');

// 定义配置项
// 密钥可从腾讯云控制台的【API密钥管理】中获取：https://console.cloud.tencent.com/cam/capi
const config = {
  secretId: '你的固定密钥', // 替换你的固定密钥
  secretKey: '你的固定密钥', // 替换你的固定密钥
  proxy: '',
  durationSeconds: 6000, // 密钥有效期
  // 放行判断相关参数
  bucket: 'bucket名字', // 换成你的 bucket
  region: 'bucket 地区', // 换成 bucket 所在地区
  allowPrefix: '*', // 这里改成允许的路径前缀，可以根据自己网站的用户登录态判断允许上传的具体路径，例子： a.jpg 或者 a/* 或者 * (使用通配符*存在重大安全风险, 请谨慎评估使用)
  allowActions: [
    // 所有 action 请看文档 https://cloud.tencent.com/document/product/436/31923
    // 简单上传
    'name/cos:PutObject',
    'name/cos:PostObject',
    // 分片上传
    'name/cos:sliceUploadFile',
    'name/cos:InitiateMultipartUpload',
    'name/cos:ListMultipartUploads',
    'name/cos:ListParts',
    'name/cos:UploadPart',
    'name/cos:CompleteMultipartUpload',
  ],
};

// 我用的是express，所以接口方法这样写，最外层的方法无所谓，每个人都不一样，主要是里面的内容

router.get('/upload/sts', (req, res) => {
  // 获取临时密钥
  const shortBucketName = config.bucket.substr(0, config.bucket.lastIndexOf('-'));
  const appId = config.bucket.substr(1 + config.bucket.lastIndexOf('-'));
  const policy = {
    version: '2.0',
    statement: [{
      action: config.allowActions,
      effect: 'allow',
      principal: { qcs: ['*'] },
      resource: [
        `qcs::cos:${config.region}:uid/${appId}:prefix//${appId}/${shortBucketName}/${config.allowPrefix}`,
      ],
    }],
  };
  STS.getCredential({
    secretId: config.secretId,
    secretKey: config.secretKey,
    proxy: config.proxy,
    durationSeconds: config.durationSeconds,
    policy,
  }, (err, tempKeys) => {
    const result = err || tempKeys || '';
    res.json(new Result({ data: result }));
  });
});
```

存储桶的相关信息可以在控制台的存储桶的【概览】中查看。

注意：**存储桶所在地域只需括号中的内容，不需要中文**。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-19-153605.png)

后端计算临时密钥的方法就是以上这些了。

## 前端代码

在配置文件中定义好基础信息

```js
// @/utils/cosConf.js

export default {
  // ...
  Bucket: '', // Bucket 名称
  Region: '', // Bucket 地域
  Domain: '', // Bucket 访问域名
};

```

然后安装以下依赖

```bash
npm install cos-js-sdk-v5 spark-md5 --save
```

具体上传代码：

```js
import COS from 'cos-js-sdk-v5';
import SparkMD5 from 'spark-md5';
// 这个方法是文章中上面写的获取临时密钥的请求方法
import { sts } from '@/api/upload';
// 上面定义的基础信息
import cosConfig from './cosConf';
let key = '';
// 配置

// 初始化实例
const cos = new COS({
  async getAuthorization(options, callback) {
    // 获取临时密钥
    const res = await sts();
    const authdata = res.data;
    const auth = {
      TmpSecretId: authdata.credentials.tmpSecretId,
      TmpSecretKey: authdata.credentials.tmpSecretKey,
      XCosSecurityToken: authdata.credentials.sessionToken,
      ExpiredTime: authdata.expiredTime, // 在ExpiredTime时间前，不会再次调用getAuthorization
    };
    callback(auth);
  },
  FileParallelLimit: 3, // 文件并发数
  ChunkParallelLimit: 8, // 同一个上传文件的分块并发数
  ChunkSize: 1024 * 1024 * 8, // 分块上传时，每块的字节数大小
});

// 获得文件md5
function getFileMD5(file, callback) {
  // 声明必要的变量
  const fileReader = new FileReader();
  // 文件每块分割2M，计算分割详情
  const chunkSize = 2 * 1024 * 1024;
  const chunks = Math.ceil(file.size / chunkSize);
  let currentChunk = 0;

  // 创建md5对象（基于SparkMD5）
  const spark = new SparkMD5();

  // 每块文件读取完毕之后的处理
  fileReader.onload = (e) => {
    // 每块交由sparkMD5进行计算
    spark.appendBinary(e.target.result);
    currentChunk += 1;

    // 如果文件处理完成计算MD5，如果还有分片继续处理
    if (currentChunk < chunks) {
      // eslint-disable-next-line no-use-before-define
      loadNext();
    } else {
      callback(spark.end());
    }
  };

  // 处理单片文件的上传
  function loadNext() {
    const start = currentChunk * chunkSize;
    const end = start + chunkSize >= file.size ? file.size : start + chunkSize;

    fileReader.readAsBinaryString(file.slice(start, end));
  }
  loadNext();
}

// 小文件直接上传-通过putObject上传
export function uploadFile(file, callback, progressBc) {
  // 得到md5码
  getFileMD5(file, (md5) => {
    // 存储文件的md5码
    file.md5 = md5;
    const subfix = file.name.substr(file.name.lastIndexOf('.'));
    key = process.env.VUE_APP_BUCKET_PATH + file.md5 + subfix;
    cos.putObject({
      Bucket: cosConfig.Bucket,
      Region: cosConfig.Region,
      Key: key,
      Body: file,
      onProgress(progressData) {
        // 上传进度
        progressBc(progressData.percent);
      },
    }, (err, data) => {
      // 成功或出错回调
      callback(err, data);
    });
  });
}

// 大文件分片上传-通过sliceUploadFile上传
export function uploadFile2(file, callback, progressBc) {
  // 得到md5码
  getFileMD5(file, (md5) => {
    // 存储文件的md5码
    file.md5 = md5;
    const subfix = file.name.substr(file.name.lastIndexOf('.'));
    key = process.env.VUE_APP_BUCKET_PATH + file.md5 + subfix;
    cos.sliceUploadFile({
      Bucket: cosConfig.Bucket,
      Region: cosConfig.Region,
      Key: key,
      Body: file,
      onProgress(progressData) {
        // 上传进度
        progressBc(progressData.percent);
      },
    }, (err, data) => {
      callback(err, data);
    });
  });
}

```

调用上传的话，只需这样：

```vue
<template>
  <div>
    <input type="file" accept="image/*" ref="upload" id="upload">
    <button @click="submit">上 传</button>
  </div>
</template>
<script>
	import { uploadFile } from '@/utils/uploadfile';
  export default {
    methods: {
      submit(){
        const file = this.$refs.upload.files[0]
        uploadFile(file, (err, data) => {
            // 回调
            if(!err){
              // 上传成功处理
            }else{
              // 出错处理
            }
          }, progress => {
            // 这里可以设置上传进度
          });
      }
    }
  }
</script>
```



## 存储桶设置

最后，存储桶还需要设置跨域访问，否则哪怕前面都正确，文件也无法上传。

在存储桶的【安全管理】-【跨域访问CORS设置】中添加规则，设置域名白名单，保存生效后，不出意外就可以正常上传文件了。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-19-155231.png)

