## 背景
将tomcat的端口修改为80，启动服务的时候，发现80端口已经被占用。

## 查看80端口被谁占用

### 查看端口占用情况命令：

netstat -aon | findstr :80

![netstat](https://github.com/liuyanliang2015/BertNote/blob/master/pics/netstat-1.png)

发现占用 0.0.0.0:80 的是 PID 为 4 的家伙

### 查看 PID 为 4 的家伙是谁命令：

tasklist /FI "PID eq 4"

![netstat](https://github.com/liuyanliang2015/BertNote/blob/master/pics/netstat-2.png)

PID 为 4 的是 SYSTEM 这个牛进程

解决方法：修改注册表

1. 位置 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\HTTP

2. 把 REG_DWORD 类型的项 Start 的值改为 0

（注：修改后需要重启系统)





