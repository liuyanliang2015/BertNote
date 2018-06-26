## 设计数据库

打开pd，create New Model ->选择Physical Diagram，注意填写Model name数据库模型名称，以及DBMS数据库类型。

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-1.png)


如果设计好的模型，想修改数据库类型，则可以通过菜单DataBase -> Change Current DBMS

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-2.png)


设计表： Model -> Tables ->设计多个表的名称

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-3.png)


![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-4.png)

设计表字段：

双击具体某个表，选择column，设计具体字段属性，字段描述，字段名称，类型，长度等

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-5.png)

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-6.png)


设计外键：Model -> References

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-7.png)

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-8.png)

点击具体外键的连线：Reference Properties -> Join -》选择字表具体的外键

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-9.png)



设置字段属性，例如：默认值，是否可以为空等

打开表设计：

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-15.png)

双击需要定义的字段，或者右击properties

选中General ->Mandatory，表示该字段不能为空。

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-16.png)

选中Standard Checks ->Default 可以设置默认值。
![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-17.png)



## 导入数据库

DataBase - > DataBase Generation

选择sql导出路径，并且可以连接数据库，直接执行sql语句

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-10.png)

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-11.png)

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-12.png)

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-13.png)

查看导入的数据库

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-14.png)

## 导出数据库到powerDesigner

打开pd，菜单File -> New Model -> Physical Diagram

输入Model name，并选择DBMS数据库类型

![pdm](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pdm.png)


选择菜单Database -> Connect -> Connection profile,选择数据库对象，如果没有，点击Configure按钮进行配置。

![pdm](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pdm-2.png)

点击connect，如果连接成功，就可以执行导出了

菜单Database -> Update Model From Database

![pdm](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pdm-3.png)


选择数据库，拥有者等筛选条件
![pdm](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pdm-4.png)


![pdm](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pdm-5.png)



