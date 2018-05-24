## 悲观锁

### 定义
悲观锁，正如其名，它指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度，因此，在整个数据处理过程中，将数据处于锁定状态。悲观锁的实现，往往依靠数据库提供的锁机制。

### 场景
商品goods表中有一个字段status，status为1代表商品未被下单，status为2代表商品已经被下单，那么我们对某个商品下单时必须确保该商品status为1。假设商品的id为1。

如果不使用锁机制，操作方法如下：

    //1.查询出商品信息
	select status from t_goods where id=1;
	//2.根据商品信息生成订单
	insert into t_orders (id,goods_id) values (null,1);
	//3.修改商品status为2
	update t_goods set status=2;

上面这种场景在高并发访问的情况下很可能会出现问题。

前面已经提到，只有当goods status为1时才能对该商品下单，上面第一步操作中，查询出来的商品status为1。

但是当我们执行第三步Update操作的时候，有可能出现其他人先一步对商品下单把goods status修改为2了，但是我们并不知道数据已经被修改了，

这样就可能造成同一个商品被下单2次，使得数据不一致。所以说这种方式是不安全的。

### 使用悲观锁

在上面的场景中，商品信息从查询出来到修改，中间有一个处理订单的过程，使用悲观锁的原理就是，当我们在查询出goods信息后就把当前的数据锁定，直到我们修改完毕后再解锁。那么在这个过程中，因为goods被锁定了，就不会出现有第三者来对其进行修改了。

注：要使用悲观锁，我们必须关闭mysql数据库的自动提交属性，因为MySQL默认使用autocommit模式，也就是说，当你执行一个更新操作后，MySQL会立刻将结果进行提交。

我们可以使用命令设置MySQL为非autocommit模式：
set autocommit=0;

    /查询当前数据库自动提交策略：默认1
	SELECT @@autocommit
	//修改为不自动提交

set autocommit=0;
设置完autocommit后，我们就可以执行我们的正常业务了。具体如下：

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



## 乐观锁

悲观锁并不是适用于任何场景，它也有它存在的一些不足，因为悲观锁大多数情况下依靠数据库的锁机制实现，以保证操作最大程度的独占性。

如果加锁的时间过长，其他用户长时间无法访问，影响了程序的并发访问性，同时这样对数据库性能开销影响也很大，特别是对长事务而言，这样的开销往往无法承受。

所以与悲观锁相对的，我们有了乐观锁。

### 定义

乐观锁（ Optimistic Locking ） 相对悲观锁而言，乐观锁假设认为数据一般情况下不会造成冲突，所以在数据进行提交更新的时候，才会正式对数据的冲突与否进行检测，如果发现冲突了，则让返回用户错误的信息，让用户决定如何去做。

### 实现

使用数据版本（Version）记录机制实现

这是乐观锁最常用的一种实现方式。何谓数据版本？即为数据增加一个版本标识，一般是通过为数据库表增加一个数字类型的 “version” 字段来实现。

当读取数据时，将version字段的值一同读出，数据每更新一次，对此version值加一。

当我们提交更新的时候，判断数据库表对应记录的当前版本信息与第一次取出来的version值进行比对，如果数据库表当前版本号与第一次取出来的version值相等，则予以更新，否则认为是过期数据。

![lock锁](https://github.com/liuyanliang2015/BertNote/blob/master/pics/mysql-lock.png)

    1.查询出商品信息
	select (status,status,version) from t_goods where id=#{id}
	2.根据商品信息生成订单
	3.修改商品status为2
	update t_goods
	set status=2,version=version+1
	where id=#{id} and version=#{version};


![lock锁](https://github.com/liuyanliang2015/BertNote/blob/master/pics/mysql-lock2.png)


![lock锁](https://github.com/liuyanliang2015/BertNote/blob/master/pics/mysql-lock3.png)

![lock锁](https://github.com/liuyanliang2015/BertNote/blob/master/pics/mysql-lock4.png)
