title: 在Ivy中引用Maven管理依赖的lib  
date: 2014-03-27 20:20:37
tags:  
  - ivy  
  - maven  
---

若只需要该lib本身而不引入该lib的任何依赖, 在dependency的属性中设置  

    transitive="false"  

若需要该lib和该lib所必须的依赖关系而不引入optional的依赖, 在dependency的属性中设置  

    conf="${yourconf}->default"  

Maven中的scope都被Ivy转换成对应的conf，可以对应着修改设置。  

参考资料:  
https://ant.apache.org/ivy/history/latest-milestone/ivyfile/dependency.html  
https://www.symphonious.net/2010/01/25/using-ivy-for-dependency-management/  
