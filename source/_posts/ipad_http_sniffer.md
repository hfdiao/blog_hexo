title: '如何抓包iPad的 http/https 请求'
tags:
  - http
  - sniffer
  - ipad
date: 2011-05-15 05:13:47  
---  

ipad上目前没发现有抓包的工具， 就算有也应该不会好用到哪里去， 但要对ipad的http或https请求进行抓包该怎么办呢？方法还是有的， 前提是有两块能上网的网卡（至少有一块无线网卡，主要是当无线路由用，本身有无线路由的就忽略connectify这一步吧）以及一个Windows操作系统（都怪Fiddler2只有windows版本）。  

首先， 通过Ad-hoc open（Access Point等其他方式也可以， 我懒得输密码就设成Ad-hoc open了）将无线网卡共享出去（可以通过[Connectify](http://www.connectify.me/)傻瓜化地设置， 不过dhcp似乎经常出问题啊）  

![]({{BASE_PATH}}/images/1a58992e494361019203c5b922f11f41955fd9ed.png "")  

然后， 开启[Fiddler2](http://www.fiddler2.com/fiddler2/) （一个强大的http抓包工具）， 如果需要抓取https的话需要在Tools->Fiddler Options->HTTPS里勾选上Decrypt https traffic  

![]({{BASE_PATH}}/images/67db5ae9703a0251529fbb5a4ebb8513256135ca.png "")  

接着需要设置Fiddler2的代理服务，如果不希望抓取非ipad的http请求请不要勾选WinINET Connections的相关选项  

![]({{BASE_PATH}}/images/9f8dc44a87b5d511dea71cd72adfac7627b289fe.png "")  

最后就是设置ipad的wifi了， wifi通过刚刚共享的无线进行上网并将HTTP代理设置为手动， 服务器地址填上192.168.2.1，端口填上8888（就是Fiddler2->Tools->Fiddler Options中填写的Fiddler listens on port），这样ipad上的所有http/https请求都能在Fiddler2上看到了。  

PS： Fiddler2抓https请求是通过伪造证书来实现的，需要信任这个伪造的证书。  
