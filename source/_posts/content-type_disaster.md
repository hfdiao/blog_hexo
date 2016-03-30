title: Content-Type引发的血案
tags:
  - content-type
  - css
  - my97datepicker
date: 2010-01-05 00:57:29
---

最近一个页面用到了my97datepicker日期控件，奇怪的是IE下控件很正常，但是在FF下控件死活出不来，点击没有任何反应，抓破头皮也想不出个所以然来。一时心血来潮想看看是不是JS有写错，打开FF的错误控制台调试了下，结果发现这么个错误：  
<pre>
错误： 样式表单 xxx&nbsp;未载入，因为它的MIME类型 "text/html" 不是 "text/css"
</pre>

奇怪啊，文件名明明是.css结尾的，怎么MIME类型是text/html呢？看了下服务器resin和apache的配置，MIME类型的映射关系配置项居然大部分给注释了！OMG！难怪MIME类型不对了！把映射关系加回去后果然在FF下一切正常了。
