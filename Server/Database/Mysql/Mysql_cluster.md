MySql集群技术
===================================

Mysql集群能解决系统中mysql数据量大、数据安全性、数据备份、负载均衡、异地同步等问题。
由于Mysql的License后期全部是GPL，所以也不做过多的研究。
以下介绍几种常见的mysql集群方式。

Master/Slave 主从式
---------------------

Master/Slave方式配置较简单，只需要在mysql默认配置中加入简单设置即可，应用场景较多。
Master负责写入、读取数据功能，Slave只有读取功能(这不算是读写分离)。
 


Master/Master 主主式
-------------------

Master/Master 是在Master/Slave 的基础上，相互Slave。
实现多点同时写入、查询功能。可以简单实现异地系统数据同步、双机热备等问题。



Mysql Cluster
-------------------

Mysql专门的集群服务器，要修改存储引擎为NDB。


Mysql-proxy
-------------------

基于Lua的mysql负载均衡器，类似于Nginx的Proxy_pass,可以轻松实现大批量mysql服务器集群调度、负载均衡、读写分离等功能。
