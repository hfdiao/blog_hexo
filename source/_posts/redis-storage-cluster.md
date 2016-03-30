title: 搭建Redis存储集群  
date: 2014-03-28 23:18:55
tags:  
  - redis
  - cluster
---

以前都是将redis当做纯粹的cache server使用，关掉snapshot和aof，也不需要slave。其中任何一台挂掉也无所谓，反正可以从db中再加载回来。  

有个项目DBA抱怨了很久，每天频繁往db插入太多数据，而这些数据90%以上都是只需要存储几天就过期，哪怕丢失部分数据也是可以接受的。综合这些条件，这些会过期的数据用redis来做临时存储应该是比较适合的。打开redis的aof，能够尽量保证数据持久存储下来。安全起见，给master配2个slave，再起3个 [sentinel](http://redis.io/topics/sentinel) 做服务监控和故障转移，这样发生故障也尽量不需要人工介入。  

需要注意的是，redis是单进程的，而且slave初次和master同步时，会强制master做一次snapshot。为了充分利用机器cpu资源并减少snapshot的开销，应该在一台机器上起多个redis实例，每个redis实例占用内存不应太大。为了充分利用内存，修改 `/etc/sysctl.conf`，增加配置项 [`vm.overcommit_memory=1`](http://redis.io/topics/faq)。

redis 2.8之前，replication不支持 [Partial resynchronization](http://redis.io/topics/replication)，网络瞬断会导致强制重新同步，造成不必要的开销，影响服务稳定性。故请务必使用redis 2.8以上版本。  

需要注意的是，sentinel严重依赖系统时间，一旦系统时间被非预期的方式修改，或者系统非常繁忙，又或者进程因为某些原因而被阻塞时，sentinel可能会工作不正常。sentinel检测到系统不正常后，将切换到 [TILT](http://redis.io/topics/sentinel) 模式，只监控而不做其他操作（故障转移等）。

Master参考配置:  

    # ${port} 表示实际的端口号
    # 复制N份redis.conf到/etc/redis/redis_${port}.conf
    # 注意：不能共用同一份配置文件，会被redis sentinel重写
    # 需要修改的配置如下（除提到的修改项外其余保持默认配置不变）：
    
    port ${port}

    # /var/run/redis/${port}/ 目录必须已经存在
    dir /var/run/redis/${port}/
    
    pidfile redis.pid
    logfile redis.log
    
    # 开启aof文件记录，启动后再手工关闭aof
    appendonly yes
    appendfilename redis.aof
    
    # 虽然关掉了snapshot，但是slave初次同步时会强制master snapshot
    # 请留够内存用于fork snapshot，建议留40%机器内存
    # 避免多个slave同时SYNC
    dbfilename redis.rdb
    
    # 关闭rdb snapshot
    # save 900 1
    # save 300 10
    # save 60 10000

    # 后台运行
    daemonize yes
    
    # 每个redis 4gb
    maxmemory 4294967296
    maxmemory-policy volatile-ttl
    
Master启动命令:  

    ./redis-server /etc/redis/redis_${port}.conf
    # 若是故障后重启，不需要关闭aof，而是在同步完成后关闭新master的aof
    ./redis-cli -p ${port} config set appendonly no
    
Slave参考配置: 

    # 略，基本同Master配置，只在最后加多一行
    slaveof $master_ip $master_port  

Slave启动命令:  

    ./redis-server /etc/redis/redis_${port}.conf
    
Sentinel参考配置:  

    # ${port} 表示实际的端口号
    port ${port}
    # /var/run/redis/${port}/ 目录必须已经存在
    dir /var/run/redis/${port}/
    
    pidfile redis.pid
    logfile redis.log
    
    # 后台运行
    daemonize yes
    
    #start config for ${master1_name}
    sentinel monitor ${master1_name} ${master1_ip} ${master1_port} 2
    sentinel down-after-milliseconds ${master1_name} 5000
    sentinel failover-timeout ${master1_name} 900000
    sentinel parallel-syncs ${master1_name} 1
    
    # 触发报警脚本，${monitor1_path}是报警脚本的路径
    # 调用脚本时传递两个参数：${event_type}, ${event_description}
    #sentinel notification-script ${master1_name} ${monitor1_path}
    #end config for ${master1_name}

    # 剩余master的配置参考master1的配置项进行补充
    
Sentinel启动命令:  

    ./redis-server /etc/redis/sentinel_${port}.conf --sentinel
    
