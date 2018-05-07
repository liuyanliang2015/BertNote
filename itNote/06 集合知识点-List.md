## List集合
list用于存储有序的，允许重复的元素。
## ArrayList
ArrayList是一个数组实现的列表，由于数据是存入数组中的，所以它的特点也和数组一样，查询很快，但是中间部分的插入和删除很慢。
## Vector
Vector就是ArrayList的线程安全版，它的方法前都加了synchronized锁，其他实现逻辑都相同。 <br>
如果对线程安全要求不高的话，可以选择ArrayList，毕竟synchronized也很耗性能
## LinkedList
故名思意就是链表,还是一个双向链表。<br>
内部维护了3个成员变量，一个是当前链表的头节点，一个是尾部节点，还有是链表长度。
## 总结
通过上面对ArrayList和LinkedList的分析，可以理解List的3个特性 <br>
1.是按顺序查找 <br>
2.允许存储项为空 <br>
3.允许多个存储项的值相等 <br>
可以知其然知其所以然<br>

然后对比LinkedList和ArrayList的实现方式不同，可以在不同的场景下使用不同的List <br>
ArrayList是由数组实现的，方便查找，返回数组下标对应的值即可，适用于多查找的场景 <br>
LinkedList由链表实现，插入和删除方便，适用于多次数据替换的场景<br>