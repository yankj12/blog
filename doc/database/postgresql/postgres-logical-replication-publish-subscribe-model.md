# Postgres Logical Replication

Logical replication uses a publish and subscribe model with one or more subscribers subscribing to one or more publications on a publisher node. Subscribers pull data from the publications they subscribe to and may subsequently re-publish data to allow cascading replication or more complex configurations.

- [Postgres Logical Replication](#Postgres-Logical-Replication)
  - [逻辑复制快速配置](#%E9%80%BB%E8%BE%91%E5%A4%8D%E5%88%B6%E5%BF%AB%E9%80%9F%E9%85%8D%E7%BD%AE)
    - [配置postgresql.conf](#%E9%85%8D%E7%BD%AEpostgresqlconf)
  - [配置网络访问](#%E9%85%8D%E7%BD%AE%E7%BD%91%E7%BB%9C%E8%AE%BF%E9%97%AE)
    - [发布端数据库(publisher database)配置](#%E5%8F%91%E5%B8%83%E7%AB%AF%E6%95%B0%E6%8D%AE%E5%BA%93publisher-database%E9%85%8D%E7%BD%AE)
    - [订阅端数据库(subscriber database)配置](#%E8%AE%A2%E9%98%85%E7%AB%AF%E6%95%B0%E6%8D%AE%E5%BA%93subscriber-database%E9%85%8D%E7%BD%AE)
    - [验证](#%E9%AA%8C%E8%AF%81)
  - [常见问题](#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)
    - [问题1](#%E9%97%AE%E9%A2%981)
    - [问题2](#%E9%97%AE%E9%A2%982)

## 逻辑复制快速配置

[logical-replication-quick-setup](https://www.postgresql.org/docs/11/logical-replication-quick-setup.html)

全面的配置参考 [链接](https://www.postgresql.org/docs/11/logical-replication-config.html)

>Logical replication requires several configuration options to be set.\
\
On the publisher side, wal_level must be set to logical, and max_replication_slots must be set to at least the number of subscriptions expected to connect, plus some reserve for table synchronization. And max_wal_senders should be set to at least the same as max_replication_slots plus the number of physical replicas that are connected at the same time.\
\
The subscriber also requires the max_replication_slots to be set. In this case it should be set to at least the number of subscriptions that will be added to the subscriber. max_logical_replication_workers must be set to at least the number of subscriptions, again plus some reserve for the table synchronization. Additionally the max_worker_processes may need to be adjusted to accommodate for replication workers, at least (max_logical_replication_workers + 1). Note that some extensions and parallel queries also take worker slots from max_worker_processes.\
\
31.8. Configuration Settings-Chapter - 31. Logical Replication\
https://www.postgresql.org/docs/11/logical-replication-config.html

### 配置postgresql.conf

在发布端配置wal_level，在postgresql.conf文件中配置如下

```postgresql.conf
#wal_level = replica                    # minimal, replica, or logical
                                        # (change requires restart)
wal_level = logical
```

## 配置网络访问

在pg_hba.conf中设置允许应答(原文为replication)的机器及访问方式

```pg_hba.conf
host     all     repuser     0.0.0.0/0     md5
```

上面的repuser我理解的是订阅端用户名

### 发布端数据库(publisher database)配置

在发布端数据库(publisher database)创建发布

```psql
CREATE PUBLICATION mypub FOR TABLE users, departments;
```

### 订阅端数据库(subscriber database)配置

在订阅端数据库(subscriber database)创建订阅

```psql
CREATE SUBSCRIPTION mysub CONNECTION 'dbname=foo host=bar user=repuser' PUBLICATION mypub;
```

### 验证

在发布端表中

## 常见问题

### 问题1

问题描述：

```psql
r021db=# CREATE SUBSCRIPTION get_test CONNECTION 'host=192.168.xxx.xxx port=5432 user=xxx dbname=p000db password=xxx ' PUBLICATION put_test ;
ERROR:  could not create replication slot "get_test": ERROR:  logical decoding requires wal_level >= logical
```

问题原因：

发布端要设置wal_level为logical

问题解决：

在发布端postgresql.conf文件中，设置wal_level为logical

### 问题2

问题描述：

```psql
r021db=> CREATE SUBSCRIPTION get_test CONNECTION 'host=192.168.xxx.xxx port=5432 user=xxx dbname=p000db password=xxx ' PUBLICATION put_test ;
ERROR:  must be superuser to create subscriptions
```

问题原因：

需要超级用户才能创建订阅

问题解决：

使用超级用户登陆订阅库来创建订阅
