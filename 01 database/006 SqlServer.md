## SqlServer表和字段注释

--为字段添加注释<br>
--格式如右：execute sp_addextendedproperty 'MS_Description','字段备注信息','user','dbo','table','字段所属的表名','column','添加注释的字段名'; <br>   

--为表添加注释<br>
--格式如右：execute sp_addextendedproperty 'MS_Description','表备注信息','user','dbo','table','表名',null,null; <br>   

## 添加数据

### 第一种方式
insert into tb_user(id,name) values(2,'lisi')


### 第二种方式
insert into tb_user(id,name) <br>
select 3,'wangwu' union <br>
select 4,'sunliu'<br>




## 查看SqlServer连接端口号

exec sys.sp_readerrorlog 0,1,'listening'


