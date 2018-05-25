在linux中，每个用户目录下都有一个.bash_history文件(隐藏文件)，每次注销时，本次登录所执行的命令都会写入该文件。

    [root@localhost /]# find -name '.bash_history'
	./root/.bash_history

清空该文件的命令：

    echo > /root/.bash_history

扩展一下：

	echo  > temp.out              清空out文件

    echo "hello word" > temp.out  清空原来的内容，写入新内容“hello word”

    echo “hello word” >> temp.out 追加内容“hello word”

想象一个场景，如果公司的服务器被一些“破坏者”侵入，在服务器上做一些动作，然后清空该日志文件，那么我们将不知道他在服务器上操作了什么，哪有什么办法呢？

### 自定义shell历史文件

    #!/bin/bash
	LOG_DIR="/var/log/audit"
	LOG_NAME="shell_audit.log"
	PROFILE="/etc/profile"
	 
	if [ ! -d $LOG_DIR ];then
	    mkdir $LOG_DIR
	fi
	if [ ! -e $LOG_DIR/$LOG_NAME ];then
	    touch $LOG_DIR/$LOG_NAME
	    chown nobody:nobody $LOG_DIR/$LOG_NAME
	    chmod 002 $LOG_DIR/$LOG_NAME
	    chattr +a $LOG_DIR/$LOG_NAME
	fi
	num=`grep "export AUDIT_FILE=$LOG_DIR/$LOG_NAME" $PROFILE |wc -l`
	num2=`grep "export PROMPT_COMMAND" $PROFILE|wc -l`
	echo "$num"
	echo "$num2"
	if [ $num -eq 0 ];then
	    echo  -e "export AUDIT_FILE=$LOG_DIR/$LOG_NAME\n" >> $PROFILE
	fi

执行上面的sh脚本，然后编辑profile文件：

    export PROMPT_COMMAND='{ date "+%y-%m-%d %T ##### $(who am i |awk "{print \$1\" \"\$2\" \"\$5}") #### $(whoami) #### $(history 1 | { read x cmd; echo "$cmd"; })"; } >>$AUDIT_FILE'

![shell](https://github.com/liuyanliang2015/BertNote/blob/master/pics/shell.png)

执行：source /etc/profile 让配置生效。

查看日志：

tail -f  /var/log/audit/shell_audit.log


