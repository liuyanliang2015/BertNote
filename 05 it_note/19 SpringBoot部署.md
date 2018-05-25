## 通过eclipse的maven将工程打成jar

注意：pom.xml中需要配置

    <build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

否则：SpringBoot打包成jar后运行提示没有主清单属性。


运行工程 Run as ->  Maven install,运行完成，就会在target目录下生成文件XXXX-XX-XX-SNAPSHOT.jar

将该jar包拷贝到linux服务器，执行下面命令启动服务：

    [root@lzz springBoot]# java -jar discovery-eureka-0.0.1-SNAPSHOT.jar  >> catalina.out 2>&1 & tail -f catalina.out


杀死springBoot进程

    [root@lzz springBoot]# netstat -apn | grep 8761
	tcp        0      0 :::8761                     :::*                        LISTEN      15311/java 
    [root@lzz springBoot]# kill -9 15311
	[1]+  已杀死               java -jar discovery-eureka-0.0.1-SNAPSHOT.jar >> catalina.out 2>&1


当然你可以自己编写启动和停止服务的脚本程序：

启动脚本：

    \#!/bin/bash

	PROJECTNAME=RangerALdapApi
	
	pid=`ps -ef |grep $PROJECTNAME |grep -v "grep" |awk '{print $2}'`
	
	if [ $pid ]; then
	
	​    echo "$PROJECTNAME  is  running  and pid=$pid"
	
	else
	
	   echo "Start success to start $PROJECTNAME ...."
	
	   nohup java -jar RangerALdapApi-0.0.1-SNAPSHOT.jar  >> catalina.out  2>&1 &
	
	fi


停止脚本：

    \#!/bin/bash

	PROJECTNAME=RangerALdapApi
	
	pid=`ps -ef |grep $PROJECTNAME |grep -v "grep" |awk '{print $2}' `
	
	if [ $pid ]; then
	
	​    echo "$PROJECTNAME is  running  and pid=$pid"
	
	​    kill -9 $pid
	
	​    if [[ $? -eq 0 ]];then
	
	​       echo "sucess to stop $PROJECTNAME "
	
	​    else
	
	​       echo "fail to stop $PROJECTNAME "
	
	​     fi
	
	fi

	
    


