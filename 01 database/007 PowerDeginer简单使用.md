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

## 导入表和字段注释到数据库

Database -> Edit Current DBMS

### 编辑表注释：

Script -> Objects -> Column->TableComment

Name：TableComment(自定义)


Comment：Command for adding a table comment using an extended property of MS SQL Server 2000 to store object description: "MS_Description".(自定义)

value：

    [if exists (select 1 from  sys.extended_properties
           where major_id = object_id('[%QUALIFIER%]%TABLE%') and minor_id = 0)
	begin 
	   [%OWNER%?[.O:[execute ][exec ]]sp_dropextendedproperty [%R%?[N]]'MS_Description',  
	   [%R%?[N]]'user', [%R%?[N]]%.q:OWNER%, [%R%?[N]]'table', [%R%?[N]]%.q:TABLE% 
	:declare @CurrentUser sysname 
	select @CurrentUser = user_name() 
	[.O:[execute ][exec ]]sp_dropextendedproperty [%R%?[N]]'MS_Description',  
	   [%R%?[N]]'user', [%R%?[N]]@CurrentUser, [%R%?[N]]'table', [%R%?[N]]%.q:TABLE% 
	] 
	end 
	
	
	][%OWNER%?[.O:[execute ][exec ]]sp_addextendedproperty [%R%?[N]]'MS_Description',  
	   [%R%?[N]]%.q:COMMENT%, 
	   [%R%?[N]]'user', [%R%?[N]]%.q:OWNER%, [%R%?[N]]'table', [%R%?[N]]%.q:TABLE% 
	:select @CurrentUser = user_name() 
	[.O:[execute ][exec ]]sp_addextendedproperty [%R%?[N]]'MS_Description',  
	   [%R%?[N]]%.q:COMMENT%, 
	   [%R%?[N]]'user', [%R%?[N]]@CurrentUser, [%R%?[N]]'table', [%R%?[N]]%.q:TABLE% 
	] 
	
### 编辑字段注释

Script -> Objects -> Column->ColumnComment

Name：ColumnComment(自定义)

Comment：Command for adding a column comment(自定义)

value：

    declare @CurrentUser sysname
	select @CurrentUser = user_name()
	execute sp_addextendedproperty 'MS_Description', 
	  %.60qA:COMMENT%,
	   'user', @CurrentUser, 'table', %TABLE% , 'column', %COLUMN% 
	go



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


## powerdesigner模型导出到word

Report -> Report Templates


![pdm](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-18.png)

点击上方的图标，新建模板

![pdm](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-19.png)

输入模板名称，选择类型：Physical Data Model

![pdm](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-20.png)

在左边找到Table -> List of Table Columns

右边选中，右击设置layout设置布局

show title设置是否显示表清单

Edit title可以设置表清单格式 【表%PARENT%的栏的清单】

![pdm](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-21.png)

选择列：代码属性、注释、数据类型、长度、是否可为空




另外，右击模板名称，可以设置模板的页眉和页脚

![pdm](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-24.png)

右击table item，可以Edit title可以设置表的显示

![pdm](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-25.png)


保存模板。


然后可以导出模型到word了：

Report -> Generate Report 

![pdm](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-22.png)


![pdm](https://github.com/liuyanliang2015/BertNote/blob/master/pics/pd-23.png)





