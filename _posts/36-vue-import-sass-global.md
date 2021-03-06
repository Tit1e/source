---
title: vue-cli 引入全局 scss 文件
date: 2017-12-14 22:11:52
tags: ['学习', 'vue']
---
#### 前言
我们平时在使用 vue-cli 开发项目的时候，写 css 会用到预处理器来提高 css 的编写效率。我们在使用 sass 的时候一般会先在一个全局的`.scss`文件中定义好一些颜色变量或其他变量，然后在需要用的这些变量的组件内把这个文件通过`@import`的方式引入，然后就可以使用文件中定义的变量了。但是这也出现一个问题，因为这个文件中定义的都是比较通用的变量，所以几乎每个`.vue`文件几乎都会将它引入，当组件多了之后，没次要引入这个文件就显得累赘，而且如果这个文件换了路径，那维护起来就比较麻烦，还容易出错。所以最好就是一个地方配置后，全局都可以直接使用，这样是最高效，也是最容易维护的。于是我在网上找了一圈，试了网上的 N 多方法，终于在 [这里](https://segmentfault.com/q/1010000008731809) 找到了我能用的方法，而且配置简单。
<!--more-->
#### 使用`sass`，这两个包是前提:
```
npm install node-sass --save-dev
npm install sass-loader --save-dev
```
#### 要全局引用`.scss`还需要安装`sass-resources-loader`
```
npm install sass-resources-loader --save-dev
```
#### 修改`/build/utils.js`，在`return`中新增如下代码
```javascript
scss: generateLoaders('sass').concat(
  {
    loader: 'sass-resources-loader',
    options: {
      //你要引入的 .scss 文件的路径
      resources: path.resolve(__dirname, '../src/assets/your.scss')
    }
  }
)
```
完成以上步骤后，你就可以在`<style lang="scss">`标签中直接使用先前定义好的变量了。
