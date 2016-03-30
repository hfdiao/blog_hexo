title: Nginx配置使用过期缓存  
date: 2014-04-15 01:28:46
tags:  
  - nginx
---

今天下午一朋友在群里问起  

    nginx有什么module可以达到这个效果：    1. 对指定uri做缓存，可配置有效时间    2. 失效时会继续返回当前缓存的页面，但会起个线程去读新的内容；并更新已缓存内容    3. 当2发生时，如果请求到的内容非200返回码，不更新缓存，继续等一个超时再去取
另一朋友提议说还是自己写一个吧，第三条需求应该没有能满足的。但一看这问题，全都是很通用的使用场景啊，nginx肯定有现成的模块了吧。翻查了下 [ngx_http_proxy_module](http://nginx.org/cn/docs/http/ngx_http_proxy_module.html) 的文档，找到  
    语法: proxy_cache_use_stale error | timeout | invalid_header | updating | http_500 | http_502 | http_503 | http_504 | http_404 | off ...;      默认值: proxy_cache_use_stale off;

看这描述完全符合这朋友的需求嘛，为何他却没发现呢？打探了一下才发现，原来他们误以为`proxy_cache_use_stale`只能设置一个值，文档上多个值之间是用‘|’隔开的，而且该指令的默认值是说明上只显示了一个`off`，让他们误解成这指令只能设置一个值了。  

其实该指令说明上还有一句，`这条指令的参数与proxy_next_upstream指令的参数相同`，如果认真去查看指令`proxy_next_upstream`的说明也应该能发现是支持多个值的。不过这文档确实也还可以写得更清晰详细一些，避免粗心的人误解。  

另外就是，千万不要总想着自己遇到的问题是特殊的，自己能遇到的问题绝大多数情况下都已经有前人解决过了，多查文档多用搜索，绝对比自己又重新发明一次轮子省时省事而且安全可靠多了。
