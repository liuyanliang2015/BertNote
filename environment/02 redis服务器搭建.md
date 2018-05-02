##redis安装：
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

