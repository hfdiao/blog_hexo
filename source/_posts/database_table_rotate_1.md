title: 数据库表的轮转
tags:
  - mysql
date: 2010-07-24 02:47:00
---

负责的一个项目，在开通时会赠送一批服务（粗略算了下大概有10来个），而开通这些服务有些可能比较耗时，就将这些服务都以后台任务的形式处理了。  

最开始开通量少的时候，这些任务都是在一张表里以不同任务名记录的，处理完就修改下任务状态。后来随着开通量的增加，集中在一张表里已经开始会影响性能了，那么很自然地就想到分表，将不同的任务划分到不同的表里，数据量就少了一个数量级。  

但是开通量增长得比较快，就算分表发现性能也并不能好多少。最开始想偷懒就打算隔一段时间清理一下已经处理完的任务（反正这些任务处理完后基本都不需要再保留的了），但是mysql如果不optimize表的话，删除的空间是不会被回收利用的，索引也是，所以就得经常停机optimize表。  

经常这么折腾，谁都受不了，而且服务也得停会影响线上用户的正常使用也不好。最近就将这些任务表都做成按天轮转，每天轮换一个表，这样每个表的数据就基本在10多万行里，而且空间不够的时候可以直接drop掉一些过期的表（写个脚本定期drop）也不会影响线上服务，维护工作量明显少了很多。
