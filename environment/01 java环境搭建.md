##1：在D盘根目录下，新建三个文件夹
A：install   下载的，待安装的源文件
B：develop   java编码相关的软件的安装目录
C：soft      办公以及其他软件的安装目录


##2：安装JDK，并配置环境变量
注意：环境变量分用户变量和系统变量。JAVA_HOME需要配置在系统变量中 <br>
    
    JAVA_HOME    D:\develop\Java\jdk1.7.0_51
	CLASSPATH    .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar
	PATH         ;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin

打开cmd，输入path回车，看到path中已经将%JAVA_HOME%替换为D:\develop\Java\jdk1.7.0_51 <br>
如果在用户变量中设置JAVA_HOME，cmd输入path回车，%JAVA_HOME%不生效，执行javac，找不到命令 <br>
验证环境变量是否配置完成：在cmd中输入以下命令<br>
    
    java、javac、java -version



##3：安装高级文本编辑器
例如Sublime或者NotePad++ <br>
D:\develop\Sublime Text 3 <br>
设置Sublime编码为UTF-8 <br>
选择菜单Preferences->Settings-User，增加一项配置"default_encoding": "UTF-8"<br>


##4：安装Tomcat(压缩免安装版)

##5：安装mysql，以及可视化工具Navicat for MySQL 

##6：安装IDE开发工具Eclipse或者IDEA

##7：安装SVN客户端TortoiseSVN

##8：安装linux远程连接工具Xshell和Xftp
   注意：安装的时候选择免费版，选择Free for Home/School



