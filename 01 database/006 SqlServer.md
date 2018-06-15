## 1、SqlServer表和字段注释

--为字段添加注释<br>
--格式如右：execute sp_addextendedproperty 'MS_Description','字段备注信息','user','dbo','table','字段所属的表名','column','添加注释的字段名'; <br>   

--为表添加注释<br>
--格式如右：execute sp_addextendedproperty 'MS_Description','表备注信息','user','dbo','table','表名',null,null; <br>   

## 2、添加数据

### 第一种方式
insert into tb_user(id,name) values(2,'lisi')


### 第二种方式
insert into tb_user(id,name) <br>
select 3,'wangwu' union <br>
select 4,'sunliu'<br>

## 3、查询数据

### 第一种方式
SELECT TOP 10 id,name,age FROM test.dbo.tb_user;

### 第二种方式
SELECT TOP 2 id,name,age FROM test..tb_user;


#### 查询前20%数据
select top 20 percent * from test.dbo.tb_user;

#### 查询以a或者b或者c开头的数据
select * from test..tb_user where name like '[abc]%';

#### 查询不是以a和b开头的数据
select * from test..tb_user where name like '[^ab]%';



## 4、高级sql

### group by +having
select user_id,COUNT(*) as booknum from test..tb_book_order  group by USER_ID having COUNT(*) > 2

### select newid() 随机数

select  newid()  返回例如：81EB90CD-0211-4095-B8A3-948354B64836

### order by newid() 随机查询数据
select top 3* from test..tb_book_order order by newid() 


## 4、查看SqlServer连接端口号

exec sys.sp_readerrorlog 0,1,'listening'


