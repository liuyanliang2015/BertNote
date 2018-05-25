## 1：搜索日志

(1)grep 

    grep '余额不足' catalina.out

	grep '2017-09-11' catalina.out   | grep '余额不足' 

(2)cat 

    cat -n catalina.out | grep '余额不足'

	cat -n catalina.out |tail -n +m |head -n 20

	搜索m行之后的日志，再取前20条

 

(3)more

    cat -n test.log |grep "地形" |more

	grep -Rn '余额不足' catalina.out | more (使用空格键翻页)

## 2：远程文件拷贝

    scp -r /usr/local/jenkinsdir/ 59.110.163.78:/usr/local

## 3：linux服务器配置查询

查看linux服务器类型及版本

    [root@LinuxServer01 ~]# cat /etc/redhat-release
	CentOS release 6.5 (Final)

查看服务器内存情况

    [root@lzz bin]# free -m
             total       used       free     shared    buffers     cached
	Mem:          1876       1803         73          0          3         28
	-/+ buffers/cache:       1771        105
	Swap:         4031        974       3057

查看linux磁盘使用情况

    [root@lzz bin]# df -k
	Filesystem           1K-blocks    Used Available Use% Mounted on
	/dev/mapper/vg_lzz-lv_root
	                      51475068 2048512  46805116   5% /
	tmpfs                   960996       0    960996   0% /dev/shm
	/dev/sda1               487652   27797    434255   7% /boot


## 查找文件

语法格式：
find [查找路径] [查找条件] [查找后的动作]
find /etc -name "passwd" -exec ls -l {} \; 


## vim

默认是命令模式

按/键，可以向下查找对应的关键字

按?键，可以向上查找对应的关键字

 

按i键，可以进入编辑模式

编辑完成，按Esc键，切回命令模式。输入:wq!保存更改，强制退出。

如果不想保存更改，直接:q!，不保存强制退出。