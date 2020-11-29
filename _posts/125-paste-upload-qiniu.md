---
title: 快捷键粘贴上传至七牛
date: 2020-03-08 14:00:24
tags: ['学习']
---

之前[这篇博客](/2020/03/08/124-qiniu-axios/)解决了 axios 直传七牛的问题，这篇来讲一下从粘贴板直接上传到七牛云的实现方式。

首先需要了解的是出于安全问题，js 是没权限主动读取用户粘贴板中的内存的，当用户主动触发粘贴事件时，js 是可以通过 `paste` 事件来获取到其中内容的。如何触发这个粘贴事件这里就不做讨论，单说事件触发后怎么获取到文件，其实很简单：

```js
function uploadFile(event) {
  // 我的处理方式是给用户一个 input 用于粘贴
  // 因此这个判断是用于判断用户的粘贴事件是否是在指定元素上触发的
  // 这个判断并不是必须的
  if (event.srcElement.id === 'up_snapshot_img_paste') {
    // 获取粘贴板中的内容
    const items = event.clipboardData && event.clipboardData.items
    let file = null
    if (items && items.length) {
      // 检索粘贴板items
      for (let i = 0; i < items.length; i++) {
        // 获取图片
        if (items[i].type.indexOf('image') !== -1) {
          file = items[i].getAsFile()
          break
        }
      }
    }
    // 触发上传
    if (file) {
      // 此时file就是剪切板中的图片文件
      const formData = new FormData()
      formData.append('file', file)
      formData.append('token', 'token')
      formData.append('key', 'key')
      const $axios = axios.create({ withCredentials: false })
      $axios({
        url: 'url',
        data: formData,
        method: 'POST'
      }).then(res => {
        // 上传成功处理逻辑
      }).catch(e => {
        // 上传失败处理逻辑
      })
    } else {
      // 没有获取到图片时的处理逻辑
    }
  }
}
// 绑定监听
document.addEventListener('paste', uploadFile)
```

以上就是实现的代码了。

