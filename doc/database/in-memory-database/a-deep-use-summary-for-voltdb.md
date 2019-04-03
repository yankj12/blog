# VoltDB深度使用

## 集群中增加节点

VoltDB集群中增加节点，也就是扩容。

如果要添加节点进入集群，不需要重启

>Another type of upgrade is when you want to reconfigure the cluster as a whole. Reasons for reconfiguring
the cluster are because you want to add or remove servers from the cluster or you need to modify the
number of partitions per server that VoltDB uses.\
\
Adding servers to the cluster can happen without stopping the database. This is called elastic scaling.
Removing servers or changing the number of sites per host requires restarting the cluster during a
maintenance window.\
\
参考自 AdminGuide.pdf - 4.3. Upgrading the Cluster

```AdminGuide.pdf
4.3.3. Adding Servers to a Running Cluster with Elastic Scaling

If you want to add servers to a VoltDB cluster — usually to increase performance and/or capacity — you can do this without having to restart the database. You add servers to the cluster using the voltdb start command with the --add flag. Note, as always, you must initialize a root directory before issuing the start command. 

For example:

$ voltdb init --dir=~/database --config=deployment.xml
$ voltdb start --dir=~/database --host=svr1,svr2 --add

The --add flag specifies that if the cluster full — that is, all of the specified number of servers are currently active in the cluster — the joining node can be added to elastically expand the cluster. You must elastically
add a full complement of servers to match the K-safety value (K+1) before the servers can participate as active members of the cluster. For example, if the K-safety value is 2, you must add 3 servers before they actually become part of the cluster and the cluster rebalances its partitions.

When you add servers to a VoltDB database, the cluster performs the following actions:

1. The new servers are added to the cluster configuration and sent copies of the schema, stored procedures, and deployment file.
2. Once sufficient servers are added, copies of all replicated tables and their share of the partitioned tables are sent to the new servers.
3. As the data is rebalanced, the new servers begin processing transactions for the partition content they have received.
4. Once rebalancing is complete, the new servers are full members of the cluster. 

If the cluster is not at its full complement of servers when you issue a voltdb start --add command, the added server will join the cluster as a replacement for a missing node rather than extending the cluster. Once the cluster is back to its full complement of nodes, the next voltdb start --add command will extend the cluster.

```

## 第9章 理解VoltDB对内存的使用

### 9.1 VoltDB如何使用内存

VoltDB使用的内存可以分为三类：

- 持久化 Persistent
- 半持久化 Semi-persistent
- 临时性 Temporary

#### 持久化的内存

持久化的内存，是指用来存储表中记录的内存，以及索引、表结构等。

#### 半持久化的内存

半持久化的内存，是指处理sql时使用的内存。包括临时表、undo buffer

临时表，当使用sql查询数据的时候，在将数据返回给发起者initiator之前，会先将数据拷贝到临时表。如果表是分区存储的，那么每个分区会创建一个这些数据的拷贝，发起者中进行数据的合并。

半持久化的内存，也有用来缓存系统活动的，比如快照和导出等功能。

#### 临时性的内存

临时性的内存，是用来管理查询和将存储过程分不到不同分区。

## 第10章 可用性

可用性体现在下面四方面

- Snapshots 快照
- Command Logging
- K-safety

    k值表示分区表复制几分

- Database Replication

    和K-safety类似，但又不同。K-safety是指在一个database中存在冗余的分区数据。但是Database Replication对整个database有个拷贝，并且可以分布在不同的地理位置。

### 10.1. K-Safety工作机制

### 10.2. 开启K-Safety

### 10.3. Recovering from System Failures

### 10.4. 避免脑裂（Network Partitions）

## 第11章 数据库异地备份

## 第12章 安全

## 第13章 保存和恢复VoltDB Database

### 13.2. 定时自动化地快照

为了避免不可预期的风险，voltdb根据时间间隔可以定时自动地进行快照

### 13.3. 管理快照

## 示例

### 建表

#### VoltDB数据类型

具体语法参考 官方文档`UsingVoltDB.pdf`中的`CREATE TABLE`章节的`Description`部分

或者参考官方网页文档 [CREATE TABLE](https://docs.voltdb.com/UsingVoltDB/ddlref_createtable.php)

#### 符合索引

To create a composite primary key from a combination of columns in a table, apply the PRIMARY KEY constraint to multiple columns with typical DDL such as the following:

```SQL
$ sqlcmd
1> CREATE TABLE Customer (
2>   FirstName VARCHAR(15),
3>   LastName VARCHAR (15),
4>   CONSTRAINT pkey PRIMARY KEY (FirstName, LastName)
5> );
```

上文中`pkey`是约束的名称

具体语法参考 官方文档`UsingVoltDB.pdf`中的`Appendix A. Supported SQL DDL Statements`章节中的`CREATE TABLE`部分

或者参考官方网页文档 [Creating Tables and Primary Keys](https://docs.voltdb.com/UsingVoltDB/SchemaTables.php)
