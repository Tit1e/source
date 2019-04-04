---
title: 一些工作中常用到的代码
date: 2017-05-14 20:17:00
tags: ['学习笔记']
---
很久没有写博客了，回原来的公司后就基本上没有更新了，到现在也快两个月了。这个博客其实很早前久应该写了，然后一直拖拖拖拖到现在。之前我全部记在了bear中，现在搬到博客上来，后续有更新就在现在基础上接着更新。其实不光涉及到工作中经常用的，还有一些经常是平时折腾的时候记下来怕日后忘记的。

**因为公司业务基本用的是jQuery，所以这里用到的代码基本上是基于jQuery。**
## iframe动态改变高度
```javascript
    //页面加载时获取iframe高度
    $("#iframepage").on('load', function () {
        var mainheight = $(this).contents().find("body").height() + 30;
        $(this).height(mainheight);
    });
    //动态改变iframe高度
    setInterval(function () {
        var mainheight = $("#iframepage").contents().find("body").height() + 30;
        $("#iframepage").height(mainheight);
    }, 200);
```
<!-- more -->
## 滚动条到底部自动加载数据
``` javascript
    //滚动条到底部加载
    $(function () {
        ajaxFlag = true;
        var page = 10;
        var num = page;
        $(window).scroll(function () {
            var scrollTop = $(this).scrollTop();                             // 滚动条距离顶部的高度
            var scrollHeight = $(document).height();                          // 当前页面的总高度
            var windowHeight = $(this).height();                              // 当前可视的页面高度
            var expectHeight = 0;                                               // 预加载距离
            if (scrollTop + windowHeight >= scrollHeight - expectHeight) {    // 距离顶部+当前高度 >=文档总高度 即代表滑动到底部
                //code....
            }
        });
    });
```
## 获取url中的参数 
*公司后端用的thinkphp，所以获取参数直接用TP的I方法就可以，上次无意间搜到了，就顺便存了*
``` javascript
    function getQueryString(name) {
        var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
        var r = window.location.search.substr(1).match(reg);
        if (r != null) return unescape(r[2]); return null;
    }
```
## 判断单选或复选框是否被选中以及改变选中状态
```javascript
    if ($(this).find('input').prop("checked")) {
        $(this).find('input').removeAttr('checked');
    } else {
        $(this).find('input').attr("checked", "checked");
    }
```
## 用阴影实现遮罩
``` css
    box-shadow: inset 0 0 20px 1px, 0 0 5px 2000px rgba(0, 0, 0, 0.8);
```
## 实时监听输入框变化
``` javascript
    $('#media_name').on('input propertychange', function () {
       //code.....
    });
```
## 去除chrome表单黄色背景
```css
    input:-webkit-autofill, 
    textarea:-webkit-autofill, 
    select:-webkit-autofill { 
        -webkit-box-shadow: 0 0 0 1000px white inset; 
    }
    input[type=text]:focus, input[type=password]:focus, textarea:focus {
        -webkit-box-shadow: 0 0 0 1000px white inset; 
    }
```
## ThinkPHP时间戳格式化
``` php
    //如果在Action里 
    $time = date("Y-m-d", $time);
    //如果在模板里 
    {$time|date='Y-m-d',##} 
```
## ThinkPHP去掉url中的index.php
在应用的根目录下面新建一个 .htaccess 文件(linux环境下)。在文件里面加入如下代码：
```
    <IfModule mod_rewrite.c>
    RewriteEngine on
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(.*)$ index.php/$1 [QSA,PT,L]
    </IfModule>
```
其实以上就已经实现了去除 url 中的 index.php 字符直接访问应用了。但是仅仅以上两步操作还会出现的一个问题就是 thinkphp 的常量 __URL__ 中还是会自动带上 index.php 这段字符串，彻底解决这个问题的办法是在项目的配置文件里加上如一条如下配置：`'URL_MODEL'=>'2'`
## vue for循环，把内容填充到 href，src中
```
:href='list.href'
:src='list.src'
```
## 首行缩进css 
```css
    text-indent:2em;
```
##  iphone动态加载元素单击无效问题
``` javascript
    $('#list').delegate('.tbody', 'click', function () {
        $('#loading').show();
        location.href = $(this).attr('data');
    })
```
## 去除移动端点击半透明感遮罩
```css
    -webkit-tap-highlight-color: rgba(0,0,0,0);
```
## 图片和文字垂直水平居中
```css
    #add_supplier {
        line-height: 2rem;
        font-size: 1.2rem!important;
    }

    #add_supplier>div {
        display: table;
        margin: 0 auto;
    }

    #add_supplier span {
        color: #00AC5A;
    }

    #add_supplier img {
        height: 50%;
        float: left;
        margin-right: 0.2rem;
    }
```
```html
	<li id="add_supplier">
        <div>
            <div>
                <img src="Public/img/company/add.png">
                <span>添加供应商信息</span>
            </div>
        </div>
    </li>
```
## ThinkPHP<eq>用法
```php
//name中不用加$
<eq name="Think.get.pm" value="1">checked</eq>
```
第一次大概就这么点，其他的之后会继续添加。