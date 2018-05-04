## redis简介
Redis是一种高级key-value数据库。它跟memcached类似，不过数据可以持久化，而且支持的数据类型很丰富。有字符串，链表，集 合和有序集合。支持在服务器端计算集合的并，交和补集(difference)等，还支持多种排序功能。<br>

Redis的所有数据都是保存在内存中，然后不定期的通过异步方式保存到磁盘上(这称为“半持久化模式”)；也可以把每一次数据变化都写入到一个append only file(aof)里面(这称为“全持久化模式”)。 

##redis安装
https://blog.csdn.net/freedomwjx/article/details/78602490

linux服务器redis.conf上配置bind 0.0.0.0 任何客户端都可以访问


##检测后台进程是否存在
ps -ef |grep redis

##检测6379端口是否在监听
netstat -lntp | grep 6379



##redis启动
[root@localhost redis-4.0.9]# redis-server redis.conf &


##redis关闭
[root@localhost redis-4.0.9]# src/redis-cli -a lzz5699  shutdown

##redis持久化策略与配置

### RDB持久化
RDB持久化是指在指定的时间间隔内将内存中的数据集快照写入磁盘，实际操作过程是fork一个子进程，先将数据集写入临时文件，写入成功后，再替换之前的文件，用二进制压缩存储。<br>
![RDB持久化](https://github.com/liuyanliang2015/BertNote/blob/master/pics/RDB.png)

    # The filename where to dump the DB
	dbfilename dump.rdb
	# Note that you must specify a directory here, not a file name.
	dir /usr/local/redis-4.0.9/db/
	save 900 1       #在900秒(15分钟)之后，如果至少有1个key发生变化，则dump内存快照。
	save 300 10      #在300秒(5分钟)之后，如果至少有10个key发生变化，则dump内存快照。
	save 60 10000    #在60秒(1分钟)之后，如果至少有10000个key发生变化，则dump内存快照。


### AOF持久化
AOF持久化以日志的形式记录服务器所处理的每一个写、删除操作，查询操作不会记录，以文本的方式记录，可以打开文件看到详细的操作记录。<br>
![AOF持久化](https://github.com/liuyanliang2015/BertNote/blob/master/pics/AOF.png) <br>

在Redis的配置文件中存在三种同步方式，它们分别是：<br>
appendfsync always      #每次有数据修改发生时都会写入AOF文件。<br>
appendfsync everysec    #每秒钟同步一次，该策略为AOF的缺省策略。<br>
appendfsync no          #从不同步。高效但是数据不会被持久化。<br>

### 两种方式比较
#### RDB优点
1：对于灾难恢复而言，RDB是非常不错的选择。<br>
2：性能最大化。对于Redis的服务进程而言，在开始持久化时，它唯一需要做的只是fork出子进程，之后再由子进程完成这些持久化的工作，这样就可以极大的避免服务进程执行IO操作了<br>
3: 相比于AOF机制，如果数据集很大，RDB的启动效率会更高<br>

#### RDB缺点
1：如果你想保证数据的高可用性，即最大限度的避免数据丢失，那么RDB将不是一个很好的选择。因为系统一旦在定时持久化之前出现宕机现象，此前没有来得及写入磁盘的数据都将丢失。<br>
2: 由于RDB是通过fork子进程来协助完成数据持久化工作的，因此，如果当数据集较大时，可能会导致整个服务器停止服务几百毫秒，甚至是1秒钟。



