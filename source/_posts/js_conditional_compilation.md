title: JS条件编译
tags:
  - javascript
date: 2012-05-05 18:08:33
---

最近给公司开发新邮件提醒的浏览器扩展时，需要分别开发搜狗和Chrome版本，而这两个版本绝大部分代码都是相同的，只有少数代码的区别。如果因为这么几行代码的不同而需要分拆到多个js文件中就会存在重复代码，后续有修改时就需要修改多处，维护起来不太方便。

在C语言里，可以根据不同的平台编译出不同的二进制代码，那么是否也可以对JS做这样的处理呢？答案是肯定的，google后发现有两个项目可以处理这个事情，一个是[js_build_tools](http://code.google.com/p/js-build-tools/ "js_build_tools")，另一个是[js-prepross](https://github.com/bramstein/js-preprocess "js-preprocess")。试用js_build_tools时发现这个东西只能对单个文件进行处理，不支持fileset，且输入文件不能和输出文件同名，使用起来不太方便。而js-preprocess支持fileset，输入文件可以和输出文件同名。相比js_build_tools，js-preprocess使用较为方便，但也存在一些不足。js-preprocess只支持相对于项目的相对路径，ant的taskname只能命名为preprocess。js_build_tools里的编译指令相比C的编译指令多了注释符号//，对js IDE相对友好。

花了一点时间对js-preprocess做了一些修改，fork的项目地址-&gt;[传送门](https://github.com/hfdiao/js-preprocess "传送门")。fork后支持绝对路径，ant的taskname没有限制，编译指令也改成js_build_tools的方式。
