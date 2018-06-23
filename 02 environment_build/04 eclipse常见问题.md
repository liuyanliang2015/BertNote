## 下载maven工具

我下载的是apache-maven-3.5.3

## 配置环境变量

MAVEN_HOME  ： D:\develop\apache-maven-3.5.3

MAVEN_OPTS  ： -Xms128m -Xmx512m


测试环境变量是否配置生效： mvn -version

![mvn](https://github.com/liuyanliang2015/BertNote/blob/master/pics/mvn-1.png)

## eclipse配置maven

windows -> Preferences -> Maven -> Installations ->Add 添加maven的安装目录

![mvn](https://github.com/liuyanliang2015/BertNote/blob/master/pics/mvn-2.png)


![mvn](https://github.com/liuyanliang2015/BertNote/blob/master/pics/mvn-3.png)


配置setting文件：

windows -> Preferences -> Maven -> User Settings

![mvn](https://github.com/liuyanliang2015/BertNote/blob/master/pics/mvn-4.png)

如何自定义jar的下载路径？

编辑maven的setting文件：D:\develop\apache-maven-3.5.3\conf\settings.xml

增加内容：

    <localRepository>E:/repository</localRepository>







