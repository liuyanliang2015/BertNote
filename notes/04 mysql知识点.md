## 锁
&emsp;&emsp;在数据库当中，除了传统的计算资源（CPU、RAM、I/O等等）的争用之外，数据也是一种供许多用户共享访问的资源。如何保证数据并发访问的一致性、有效性，是所有数据库必须解决的一个问题，锁的冲突也是影响数据库并发访问性能的一个重要因素。从这一角度来说，锁对于数据库而言就显得尤为重要。<br>

&emsp;&emsp;相对于其他的数据库而言，MySQL的锁机制比较简单，最显著的特点就是不同的存储引擎支持不同的锁机制。InnoDB支持行锁和表锁，而MyISAM只支持表锁。<br>
&emsp;&emsp;行锁： 开销大，加锁慢；会出现死锁；锁定粒度小，发生锁冲突的概率低，并发度高。<br>
&emsp;&emsp;表锁： 开销小，加锁快；不会出现死锁；锁定力度大，发生锁冲突概率高，并发度最低。<br>
&emsp;&emsp;很难笼统的说哪种锁最好，只能根据具体应用的特点来决定。

### 悲观锁
&emsp;&emsp;正如其名，它指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度。<br>

场景分析：<br>
商品goods表中有一个字段status，status为1代表商品未被下单，status为2代表商品已经被下单，那么我们对某个商品下单时必须确保该商品status为1。假设商品的id为1。

    //1.查询出商品信息
    select status from t_goods where id=1;
    //2.根据商品信息生成订单
    insert into t_orders (id,goods_id) values (null,1);
	//3.修改商品status为2
	update t_goods set status=2;

&emsp;&emsp;上面这种场景在高并发访问的情况下很可能会出现问题。<br>

&emsp;&emsp;在上面的场景中，商品信息从查询出来到修改，中间有一个处理订单的过程，使用悲观锁的原理就是，当我们在查询出goods信息后就把当前的数据锁定，直到我们修改完毕后再解锁。那么在这个过程中，因为goods被锁定了，就不会出现有第三者来对其进行修改了。

    //0.开始事务
	begin;/begin work;/start transaction; (三者选一就可以)
	//1.查询出商品信息
	select status from t_goods where id=1 for update;
	//2.根据商品信息生成订单
	insert into t_orders (id,goods_id) values (null,1);
	//3.修改商品status为2
	update t_goods set status=2;
	//4.提交事务
	commit;/commit work;

### 乐观锁
&emsp;&emsp;悲观锁并不是适用于任何场景，它也有它存在的一些不足，因为悲观锁大多数情况下依靠数据库的锁机制实现，以保证操作最大程度的独占性。<br>
&emsp;&emsp;如果加锁的时间过长，其他用户长时间无法访问，影响了程序的并发访问性，同时这样对数据库性能开销影响也很大，特别是对长事务而言，这样的开销往往无法承受。所以与悲观锁相对的，我们有了乐观锁。<br>
&emsp;&emsp;乐观锁（ Optimistic Locking ） 相对悲观锁而言，乐观锁假设认为数据一般情况下不会造成冲突，所以在数据进行提交更新的时候，才会正式对数据的冲突与否进行检测，如果发现冲突了，则让返回用户错误的信息，让用户决定如何去做。
#### 方式1：使用数据版本（Version）记录机制实现

    1.查询出商品信息
	select (status,status,version) from t_goods where id=#{id}
	2.根据商品信息生成订单
	3.修改商品status为2
	update t_goods
	set status=2,version=version+1
	where id=#{id} and version=#{version};

![mybatis语法](https://github.com/liuyanliang2015/BertNote/blob/master/pics/mybatis-version.png)

####方式2：时间戳
&emsp;&emsp;第二种实现方式和第一种差不多，同样是在需要乐观锁控制的table中增加一个字段，名称无所谓，字段类型使用时间戳（timestamp）,和上面的version类似，也是在更新提交的时候检查当前数据库中数据的时间戳和自己更新前取到的时间戳进行对比，如果一致则OK，否则就是版本冲突。
## 索引
&emsp;&emsp;索引（Index）是帮助MySQL高效获取数据的数据结构。<br>
&emsp;&emsp;B+Tree是在B-Tree基础上的一种优化，使其更适合实现外存储索引结构，InnoDB存储引擎就是用B+Tree实现其索引结构。<br>
### 索引失效情况
A：对单字段做了索引，where条件多字段<br>
B：建立联合索引，但是where条件单字段<br>
C：对索引列进行各种运算，例如+、-、*、/、<>、like'%_'（%在前）、or、in等操作<br>
D：类型错误，如字段类型为varchar，而where条件用number<br>
E：对索引应用内部函数，这种情况应该建立基于函数的索引<br>
F：is null索引失效<br>

## 事务
在 MySQL 中只有使用了 Innodb 数据库引擎的数据库或表才支持事务。<br>
事务处理可以用来维护数据库的完整性，保证成批的 SQL 语句要么全部执行，要么全部不执行

    mysql> use RUNOOB;
	Database changed
	mysql> CREATE TABLE runoob_transaction_test( id int(5)) engine=innodb;  # 创建数据表
	Query OK, 0 rows affected (0.04 sec)
	 
	mysql> select * from runoob_transaction_test;
	Empty set (0.01 sec)
	 
	mysql> begin;  # 开始事务
	Query OK, 0 rows affected (0.00 sec)
	 
	mysql> insert into runoob_transaction_test value(5);
	Query OK, 1 rows affected (0.01 sec)
	 
	mysql> insert into runoob_transaction_test value(6);
	Query OK, 1 rows affected (0.00 sec)
	 
	mysql> commit; # 提交事务
	Query OK, 0 rows affected (0.01 sec)
    
    //----------------------------------------------------

    mysql> begin;    # 开始事务
	Query OK, 0 rows affected (0.00 sec)
	 
	mysql>  insert into runoob_transaction_test values(7);
	Query OK, 1 rows affected (0.00 sec)
	 
	mysql> rollback;   # 回滚
	Query OK, 0 rows affected (0.00 sec)
	 
	mysql>   select * from runoob_transaction_test;   # 因为回滚所以数据没有插入

## sql调优
### A:选择合适的存储引擎: InnoDB <br>
&emsp;&emsp;MyISAM 比 InnoDB 速度快,但是现在没人用mysql查询来全文检索了，MyISAM不支持事务，因此我们选择InnoDB 。<br>
### B：使用mysql缓存<br>
&emsp;&emsp;MySQL的缓存是对全部的sql有效的， 也就是说一旦开启了查询缓存，那么对所有的sql查询默认都是开启的。但是，不要使用mysql内置的函数例如：now(),这样就不会使用缓存了。
&emsp;&emsp;开启查询缓存的方式也非常简单，在my.cnf配置文件中设置上面的关于查询缓存的变量就可以了。<br>
&emsp;&emsp;query_caceh_type  0：不开启 1：始终开启 2：按需开启
### B：充分使用索引
### C：分析查询日志和慢查询日志
### D：单条查询最后添加 LIMIT 1，停止全表扫描
### E：EXPLAIN 你的 SELECT 查询
&emsp;&emsp;使用 EXPLAIN 关键字可以让你知道MySQL是如何处理你的SQL语句的。这可以帮你分析你的查询语句或是表结构的性能瓶颈
### F：避免 SELECT *
### G：分割表
