索引对大数据的查询速度的提升是非常大的，Explain可以帮你分析SQL语句是否用到相关索引。

## 1：导致SQL执行慢的原因
A：硬件问题。如网络速度慢，内存不足，I/O吞吐量小，磁盘空间满了等。

B：没有索引或者索引失效。

C：数据过多

D：服务器调优及各个参数设置（调整my.cnf）


## 2:慢SQL分析

1.先观察，开启慢查询日志，设置相应的阈值（比如超过3秒就是慢SQL），在生产环境跑上个一天过后，看看哪些SQL比较慢。

2.Explain和慢SQL分析。比如SQL语句写的烂，索引没有或失效，关联查询太多（有时候是设计缺陷或者不得以的需求）等等。

3.Show Profile是比Explain更近一步的执行细节，可以查询到执行每一个SQL都干了什么事，这些事分别花了多少秒。

4.找DBA或者运维对MySQL进行服务器的参数调优。
