title: 'Java RPC工具eurpc 0.2.0更新日志'
tags:
  - eurpc
  - java
  - rpc
date: 2012-12-22 20:24:21
---

1. 将I/O逻辑从RpcClient中抽取出来到RpcConnection中，得益于此，使得同一个client多次方法调用间可以使用不同的connection（连接池实现起来就更自然了）
2. 一些类名称做了修改，含义更清晰。SimpleSerializer重命名为JDKObjectSerializer，SimpleRpcServer重命名为BIORpcServer
3. JDKObjectSerializer的网络传输方式也增加了length field header，终于和其他Serializer的格式一致（NettyRpcServer/NettyRpcConnection也可以使用JDKObjectSerializer了）
4. 增加了Logger接口定义以及一些常用日志组件的LoggerAdapter，并且提供了日志组件自动检测机制（LoggerHolder类，检测顺序：slf4j-->commons-logging-->log4j-->java.util.Logger）
