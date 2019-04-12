---
title: Ajax发送请求
date: 2016-11-07 22:31:12
tags: ['学习笔记']
---
学前端以来Js一直是我的噩梦，Ajax更是然我头疼。今天在公司看了好久的Ajax，后来结合微专业的视频看下来思路总算是有点清晰了，也终于了解了那几个参数是怎么回事，其实应该早就该搞懂的东西。下面说说我如今对Ajax的理解情况（还是很菜）。

首先要发起ajax先要创建一个 `XMLHttpRequest` 对象，老版本IE使用`ActiveX` 对象，但我现在没这个需求，也就不考虑这个情况。
``` javascript
var xhr = new XMLHttpRequest;
```
发送请求
``` javascript
//第一个参数为方法，GET或POST；第二个参数为url地址，第三个参数为是否异步加载，默认true为异步加载。
xmlhttp.open("GET","index.json",true);
//因为是get请求，所以不发送数据；
xmlhttp.send(null);
```
<!--more-->
`onreadystatechange`可以监视`readyState`的状态，当`readyState`的值为`4`，并且`status`的值为`200`时，说明请求成功，接下来执行回调函数。
``` javascript
xhr.onreadystatechange=function(){
	if(xhr.readyState==4 && xhr.status==200){
			//请求的数据为json，所以用JSON.parse()转为对象再操作
			var data = JSON.parse(xhr.responseText);
			console.log(data);
			//然后就可以将数据填充到html结构中去了
			document.getElementById("p").innerHTML='我叫' + data.name + ',今年' + data.age + '岁，是个' + data.sex + '。'
	}
}
```
下面是一个简单的demo代码
``` html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<div id="p"></div>
    <！-- 按钮绑定函数 -->
	<button onclick="Ajax();">提交</button>
	<script type="text/javascript">
		//将Ajax封装成一个函数
		function Ajax(){
			var xhr = new XMLHttpRequest();
			xhr.onreadystatechange=function(){
				if(xhr.readyState==4 && xhr.status==200){
					var data = JSON.parse(xhr.responseText);
					console.log(data);
					document.getElementById("p").innerHTML='我叫' + data.name + ',今年' + data.age + '岁，是个' + data.sex + '。'
				}
			}
			xhr.open('get','index.json');
			xhr.send(null);
		}
	</script>
</body>
</html>
```
今天我突然想通了，写技术日志的意义，一个是输出自己学到的东西，还有一个就是当做笔记，至少在下次遇到这方面问题的时候可以直接到自己博客上找而不用去google、百度，至少自己组织的语言解释看起来总比看别人的来的容易理解。