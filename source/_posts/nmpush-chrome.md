title: '网易新邮件提醒Chrome扩展开发'
tags:
  - chrome
  - pushmail
date: 2011-11-20 01:26:19
---

最近突然对chrome扩展开发来了兴趣，刚好最近了解了下网易邮箱助手获取新邮件到达的方法就想着自己动手写一个新邮件到达提醒的chrome扩展（其实挺蛋疼的，网易邮箱网页版本身就提供了提醒功能）。协议可以通过抓包来了解，也比较简单易懂。

周六睡醒就花了几个小时一边看chrome扩展的[开发文档](http://open.chrome.360.cn/html/dev_manifest.html)一边码代码，花了几个小时就把第一版弄出来了，chrome的扩展开发还是挺方便的。

一般来说，extensions都会有一个背景页面（background_page）用于主流程处理，逻辑代码通常都在这里。除了流程处理外一般还会有extension的设置需要处理，那么一般也会提供一个选项页面（options_page）。extension各个页面之间可以通过chrome.extension的api来进行通讯，比如可以通过chrome.extension.getBackgroudPage()获得背景页面的DOM树。API都相对比较简单，用的时候翻翻手册就很容易明白了（PS：360翻译的质量真不敢恭维）。

项目代码托管在google code上，有兴趣的可以自行[查看](http://code.google.com/p/nmpush/)。

TODO：
<del> 1\. 目前版本账号密码都是明文保存的，可以改成md5处理。</del>
<del> 2\. 点击进入邮箱查看邮件。</del>
<del> 3\. 5秒后自动关闭弹窗。</del>
<del> 4\. 自动更新。</del>
5\. 多账号支持。
6\. 异常情况处理。
