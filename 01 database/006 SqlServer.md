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


## 5、查看SqlServer连接端口号

exec sys.sp_readerrorlog 0,1,'listening'


## 5、SqlServer数据库设计


常用的数据类型： <br>
int：从-2147483648到-2147483647之间的整数（可用于标识符列）<br>
money：货币类型，可包含小数。<br>
decimal：小数，位数较大<br>
float：小数<br>
datetime：日期类型<br>
char：可包含8000个字符<br>
varchar：较char可存储更多字符,一般适用于英文和数字<br>
nvarchar：较char可存储更多字符,适用中文和其他字符。<br>
varchar(max)，可以达到2G。<br>
binary：用于存储可达8000字节长的定长的二进制数据<br>
image：大约可存储20亿个二进制数据<br>
