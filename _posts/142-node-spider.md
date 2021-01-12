---
title: Node.js 爬虫初步实践
date: 2020-12-28 21:15:19
tags: ['node', '折腾', '学习']
---

今年年初的时候动森因为本身的影响力加上疫情的原因，在全球大火，很多原本都不知道 Switch 是什么的人，都因为动森入了Switch，当时 Switch 价格一度被炒了上去，动森限定版更是翻了倍。可见其火热程度。

而我那时候也加入的动森大部队，并且加了群，大家在群里聊得热火朝天，各种摸，各种报价格，卖大头菜。那时候我就想着做一个“菜市场”小程序，方便大家卖大头菜。但等我开发出第一版的时候，卖大头菜的热度已经下去了，我就失去了开发的动力。后来在查动森信息的时候，无意间发现了 B 站的[动森 WIKI 页](https://wiki.biligame.com/dongsen/%E9%A6%96%E9%A1%B5)。于是我又转念一想，不如爬它数据做个图鉴吧！不过此时的心态完全是为了写爬虫练手，因为当时已经有很多成熟的图鉴app、小程序出现，我就不凑这个热闹了。

下面是小程序码，欢迎可以扫码体验。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-28-animalCrossingCode.jpg)

言归正传，下面是以[动森小动物图鉴](https://wiki.biligame.com/dongsen/%E5%B0%8F%E5%8A%A8%E7%89%A9%E5%9B%BE%E9%89%B4)页面为目标，写一个简易爬虫。

安装一些插件：

```bash
# npm 初始化
npm init -y
# 安装插件
npm i superagent cheerio
```

* superagent 是 node 环境下的 http 模块，可用来发器请求，稍后用来请求页面
* cheerio 模块可以解析页面，然后可以使用与 jQuery 相同的语法来操作 DOM

新建入口文件 `index.js`

封装请求：

```js
// getContent.js
const BASE_URL = 'https://wiki.biligame.com';
const superagent = require("superagent");
const cheerio = require("cheerio");

/**
 * 获取页面内容方法
 * @param {String} query 爬取的页面后缀
 */
function getContent(query) {
  return new Promise((resolve, reject) => {
    superagent.get(`${BASE_URL}/dongsen/${query}`).end((err, sres) => {
      if (err) {
        return reject(err)
      }
      resolve(cheerio.load(sres.text));
    })
  })
}

module.exports = {
  getContent
}
```

在编写爬取代码前我们要对页面做一个分析：

这是我们要爬取的目标页面

[https://wiki.biligame.com/dongsen/小动物图鉴](https://wiki.biligame.com/dongsen/小动物图鉴)

这是我们要爬取的目标

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-28-141903.png)

每个小动物点击名字可进入详情页：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-28-142155.png)

可以看到详情页中信息比列表上的更为丰富，我们要想办法爬取详情页中的信息。所以需要去获取列表上 a 标签的地址，然后再去获取详情中的动物信息。

所以思路如下：

1. 首先请求列表页
2. 获取列表中的所有小动物的项
3. 循环项取出每项名字，获取名字上的a链接地址，拼接出完整路径
4. 通过完整路径获取小动物的详细信息
5. 写入数据

思路清晰后那么开始编写代码，编辑 `index.js`：

首先我们要爬取所有小动物的数据：

```js
// index.js


/**
 * 爬取小动物页面
 * @param {Date} update_time 爬虫执行时间，非必须
 * @param {*} url 小动物页面的后缀
 */
async function getAnimals(update_time, url) {
  const $ = await getContent(url)
  // 获取列表中的所有项
  const nodes = $("#CardSelectTr tbody tr")
  // 小动物详情页后缀，其实就是名字
  const animals = []
  const LENGTH = nodes.length
 	// 第 0 项是表头，所以索引从 1 开始
  for (let i = 1; i < LENGTH; i++){
    let $element = $(nodes[i]);
    // 列表中有一项错误项，需要排除
    if ($element.find('td').eq(0).find('a').text() !== '40pxString') {
      // 获取小动物链接后缀
      const url = $element.find('td').eq(0).find('a').attr('href').substr(9)
      animals.push(url)
    }
  }
  
  // 删除 animals.txt，开始获取数据前，清除旧数据
  fs.unlink('animals.txt', function (error) {
    if (error) {
      console.log(error);
      return false;
    }
  })
  
  // 为了按列表顺序获取小动物信息，我使用了递归
  let index = 0
  getInfo(animals, index, animalInfo, update_time)
}
```

在 `getAnimals` 方法最后使用了 `getInfo` 递归函数，那么接下来编写 `getInfo`，依旧是在 `index.js`：

```js
// index.js

const fs = require("fs");
const os = require("os");

async function getInfo(list, index, func, update_time) {
  let name = list[index];
  let otherInfo = {};
  try {
    otherInfo = await func(name).catch(() => {
      if (index < list.length - 1) {
        index += 1;
        getInfo(list, index, func, update_time);
      }
    });
  } catch (error) {
    // 出错跳过进入下一个小动物
    if (index < list.length - 1) {
      index += 1;
      getInfo(list, index, func, update_time);
      return;
    }
  }
  // os.EOL 用于换行 http://nodejs.cn/api/os/os_eol.html
  // 获取到数据后写入 animals.txt
  fs.appendFile("animals.txt", JSON.stringify(otherInfo) + os.EOL, (err) => {
    if (err) {
      throw err;
    } else {
      if (index < list.length - 1) {
        index += 1;
        getInfo(list, index, func, update_time);
      }
    }
  });
}
```

递归方法也写好了，接下来就是真正获取小动物数据的方法，还是在 `index.js`：

```js
// index.js

const { getContent } = require('./getContent.js')

/**
 * 获取小动物信息
 * @param {String} url 小动物页面后缀
 */
async function animalInfo(url) {
  return new Promise(async (resolve, reject) => {
    try {
      const $ = await getContent(url).catch(err => {
        reject()
      })
      // 定义小动物各项信息的字段
      const list = ['birthday', 'character', 'mantra', 'hobby', 'style', 'color', 'vioce', 'ethnicity', 'motto', 'foreign_name']
      const nodes = $(".box-poke-left .box-poke")
      const str = $(".box-poke-left .box-title-1").text()
      const name = str.substr(0, str.length -1)
      const sex = str.substr(-1) === '♂' ? '男' : '女'
      const image = $(".box-poke-right").find('img').attr('src')
      const info = {
        name,
        sex,
        image,
      }
      for (let i = 0; i < list.length; i++){
        const attr = list[i]
        const text = nodes.eq(i).find('.box-font').text()
        if (attr === 'birthday') {
          // 处理小动物的信息，也可以不处理
          info[attr] = text.replace('月', '-').replace('日', '')
        } else {
          info[attr] = text
        }
      }
      info.birth_month = info.birthday.split('-').shift()
      resolve(info)
    } catch (error) {
      reject(url + '出错啦')
    }
  })
}
```

最后，在 `index.js` 最后执行方法

```js
// index.js

const now = new Date()
const query = encodeURIComponent('小动物图鉴')
getAnimals(now, query)
```

完整的 `index.js` ：

```js
const fs = require("fs");
const os = require("os");
const { getContent } = require('./getContent.js')



async function getInfo(list, index, func, update_time) {
  let name = list[index];
  let otherInfo = {};
  try {
    otherInfo = await func(name).catch(() => {
      if (index < list.length - 1) {
        index += 1;
        getInfo(list, index, func, update_time);
      }
    });
  } catch (error) {
    // 出错跳过进入下一个小动物
    if (index < list.length - 1) {
      index += 1;
      getInfo(list, index, func, update_time);
      return;
    }
  }
  // os.EOL 用于换行 http://nodejs.cn/api/os/os_eol.html
  fs.appendFile("animals.txt", JSON.stringify(otherInfo) + os.EOL, (err) => {
    if (err) {
      throw err;
    } else {
      if (index < list.length - 1) {
        index += 1;
        getInfo(list, index, func, update_time);
      }
    }
  });
}

/**
 * 获取小动物信息
 * @param {String} url 小动物页面后缀
 */
async function animalInfo(url) {
  return new Promise(async (resolve, reject) => {
    try {
      const $ = await getContent(url).catch(err => {
        reject()
      })
      const list = ['birthday', 'character', 'mantra', 'hobby', 'style', 'color', 'vioce', 'ethnicity', 'motto', 'foreign_name']
      const nodes = $(".box-poke-left .box-poke")
      const str = $(".box-poke-left .box-title-1").text()
      const name = str.substr(0, str.length -1)
      const sex = str.substr(-1) === '♂' ? '男' : '女'
      const image = $(".box-poke-right").find('img').attr('src')
      const info = {
        name,
        sex,
        image,
      }
      for (let i = 0; i < list.length; i++){
        const attr = list[i]
        const text = nodes.eq(i).find('.box-font').text()
        if (attr === 'birthday') {
          info[attr] = text.replace('月', '-').replace('日', '')
        } else {
          info[attr] = text
        }
      }
      info.birth_month = info.birthday.split('-').shift()
      resolve(info)
    } catch (error) {
      reject(url + '出错啦')
    }
  })
}

/**
 * 爬取小动物页面
 * @param {Date} update_time 爬虫执行时间，非必须
 * @param {*} url 小动物页面的后缀
 */
async function getAnimals(update_time, url) {
  const $ = await getContent(url)
  const nodes = $("#CardSelectTr tbody tr")
  const animals = []
  const LENGTH = nodes.length
  for (let i = 1; i < LENGTH; i++){
    let $element = $(nodes[i]);
    if ($element.find('td').eq(0).find('a').text() !== '40pxString') {
      // 获取小动物链接后缀
      const url = $element.find('td').eq(0).find('a').attr('href').substr(9)
      animals.push(url)
    }
  }

  // 删除 animals.txt，开始获取数据前，清除旧数据
  fs.unlink('animals.txt', function (error) {
    if (error) {
      console.log(error);
      return false;
    }
  })
  // 递归获取信息
  let index = 0
  getInfo(animals, index, animalInfo, update_time)
}

// 我们爬取的页面是 https://wiki.biligame.com/dongsen/%E5%B0%8F%E5%8A%A8%E7%89%A9%E5%9B%BE%E9%89%B4
const now = new Date()
const query = encodeURIComponent('小动物图鉴')
getAnimals(now, query)
```

运行爬虫：

```bash
node index.js
```

不出意外会在根目录下生成一个 animals.txt 文件。

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-12-28-145321.png)

[源码查看](https://github.com/Tit1e/Demos/tree/master/142.node-spider)

