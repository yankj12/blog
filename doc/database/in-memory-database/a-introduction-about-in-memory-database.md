# A INTRODUCTION ABOUT IN MEMORY DATABASE

一篇关于内存数据库的概述

## 技术选型

众多内存数据库中选择 Apache Ignite 和 VoltDB 进行研究

## VoltDB

### VoltDB如何持久化

VoltDB 如何实现了耐久性，它毕竟是一个内存数据库。如果数据库出于某种原因而关闭，那么所有数据都会从内存中删除；毕竟内存是一种易失性存储媒介。VoltDB 使用快照来保存数据。

快照的名称准确表达了它的用途：数据库中存储的数据在给定时间点的快照。可以将 VoltDB 配置为以固定时间间隔自动创建快照，并将这些快照持久存储到磁盘上。当数据库出于某种原因而关闭时，可使用快照将数据库返回到它关闭前的状态。

为此，在项目目录中打开部署文件 deployment.xml 并编辑它，类似于

```XML
<?xml version="1.0"?>
<deployment>
    <cluster hostcount="1" sitesperhost="2" />
    <paths>
        <voltdbroot path="/tmp" />
        <snapshots path="/tmp/autobackup" />
    </paths>
    <httpd enabled="true">
        <jsonapi enabled="true" />
    </httpd>
    <snapshot prefix="acmesave"
              frequency="2m"
              retain="3" />
</deployment>
```

告诉 VoltDB 每隔两分钟在 /tmp/autobackup 中创建一个备份并保留最后 3 个快照：在达到指定的保留限制时，会删除旧快照。在实际中，快照非常适合保存到网络挂载的位置（使用 NFS），以确保它们存储在一个不同的位置。

### VolDB参考资料

[第三方资料 VoltDB简介](https://www.ibm.com/developerworks/cn/opensource/os-voltdb/)

[VoltDB官方文档汇总页](https://docs.voltdb.com/?utm_source=LL-UX&utm_medium=developers-dropdown&utm_content=developers-dropdown-top&utm_campaign=docs)

[VoltDB Tutorial](http://downloads.voltdb.com/documentation/tutorial.pdf)

[UsingVoltDB](http://downloads.voltdb.com/documentation/UsingVoltDB.pdf)

[VoltDB SQL语法](https://docs.voltdb.com/QuickRef/?sql)

## Apache Ignite

### Apache Ignite参考资料

[Apache Ignite中文文档](https://liyuj.gitee.io/doc/java/#_1-1-ignite%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F)
