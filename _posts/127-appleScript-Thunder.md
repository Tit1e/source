---
title: AppleScript 初尝试
date: 2020-03-14 10:15:48
tags: ['折腾']
---

迅雷有时候会出现刚开始下载的时候速度很快，但是过几十秒，速度就只有几十 kb 的情况。正常下载太慢，循环暂停启动方法倒是可以，但是我总不能什么都不做就光坐电脑前不停点开始任务，暂停任务吧？况且我一般都是睡觉的时候挂着迅雷。

想起以前在用 windows 的时候接触过按键精灵这类模拟键鼠操作的软件，我觉得 Mac OS 平台应该也有类似的软件，但是一圈搜下来并没有找到，倒是找到了官方的脚本语言 AppleScript。它倒是可以模拟键鼠操作，只是得自己折腾。

那我能怕折腾么？于是直接开干，边在<a href="applescript://">脚本编辑器</a>测试边 Google，没多久折腾出来了个能将就用的。后来我又

优化了一下，并且集成到了 Alfred 中。

```
// 如果直接用脚本编辑器执行，不需要第一行与最后行，这是要在 Alfred 中执行才需要的。
on alfred_script(q)
  	tell application "System Events"
		tell application "Thunder"
			--激活主窗口
			activate
		end tell
		--暂停一秒
		delay 1
		--全选任务，因只有选中才能暂停开始任务
		key code 0 using {command down}
		tell process "Thunder"
			tell window "迅雷"
				entire contents
				--q 是传进来的参数，表示循环次数
				--本代码中一次循环用时 20 + 2 秒，可根据实际情况调整
				--q 的次数根据每次循环的时长来计算
				repeat q times
					--等待20秒后暂停任务
					delay 20
					click button 7
					--暂停2秒后重新开始任务
					delay 2
					click button 6
				end repeat
			end tell
		end tell
	end tell
end alfred_script
```

在集成到 Alfred 的过程中踩了个坑，就是传参问题，我之前一直用 `{query}` 来获取传过来的参数，但是一直不成功，查了后原来这要直接用 `q` 就行了。其实在脚本第一行已经说明，但我一直以为是固定格式。

之后我发现使用 AppleScript 还有个好处就是脚本执行的时候可以不保持窗口最前：

![](https://personal-1251959693.cos.ap-chengdu.myqcloud.com/2020-03-14-%E5%B1%8F%E5%B9%95%E5%BD%95%E5%88%B6%202020-03-14%2010.47.37.gif)

由于 AppleScript 的资料太少了，官方文档也很简单，没有实现我想要的效果。其实我本来的逻辑是每次循环前先判断下载列表中有没有任务，有的话就执行，没有就退出循环。但是目前还没找到获取的方法，所以只能先将就一下。

