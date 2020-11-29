---
title: axios 直传七牛问题
date: 2020-03-08 11:33:26
tags: ['学习']
---

前几天遇到了这样一个需求：监听粘贴事件，将粘贴板中的文件上传至七牛。

这个需求的实现过程我会另开一片文章，这篇文章主要用来记录我在使用 axios 直传七牛的过程中遇到的问题。

我本想这不是很简单：

```js
function uploadFile(){
  const formData = new FormData()
  formData.append('file', file)
  formData.append('token', this.token)
  formData.append('key', this.key)
  axios.post('url', formData).then(res => {
    // code......
  })
}
```

结果返回的却是：

```json
{error: "invalid multipart format: no multipart boundary param in Content-Type"}
```

然后我就开始在网上搜，试着改了 `Content-Type`，但还是没用，最后我在[这里](https://blog.qiatia.cn/2019/05/18/Axios/)找到了与我遇到相同问题的人，于是我用他的方法试了一下：

```js
import axios from 'axios'


function uploadFile(){
  const $axios = axios.create({ withCredentials: false })
  const formData = new FormData()
  formData.append('file', file)
  formData.append('token', this.token)
  formData.append('key', this.key)
  $axios({
    url: 'url',
    data: formData,
    method: 'post'
  }).then(res => {
    // code......
  })
}
```

结果果然成功了！

不过发生了个诡异的事情，在我写这篇文章的时候，我想重新复现一下问题，结果发现我上面直接使用 `axios.post` 也可以正常上传了，但前几天在试的时候的确是返回错误信息的！所以我现在有点怀疑人生。