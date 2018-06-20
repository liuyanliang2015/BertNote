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



## 导入数据库

DataBase - > DataBase Generation

选择sql导出路径，并且可以连接数据库，直接执行sql语句

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-10.png)

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-11.png)

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-12.png)

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-13.png)

查看导入的数据库

![jdbc](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-14.png)