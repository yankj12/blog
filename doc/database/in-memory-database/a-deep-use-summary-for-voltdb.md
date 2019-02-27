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