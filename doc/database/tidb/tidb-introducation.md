# TiDB Introducation

## 概述

本文主要参考[TiDB官方文档](https://www.pingcap.com/docs-cn/overview/)编写

## 技术细节

### 三篇文章了解 TiDB 技术内幕

参考如下三篇文章内容

- [三篇文章了解 TiDB 技术内幕 - 说存储](https://pingcap.com/blog-cn/tidb-internal-1/)

- [三篇文章了解 TiDB 技术内幕 - 说计算](https://pingcap.com/blog-cn/tidb-internal-2/)

- [三篇文章了解 TiDB 技术内幕 - 谈调度](https://pingcap.com/blog-cn/tidb-internal-3/)

## TiDB 整体架构 

iDB 的整体架构。TiDB 集群主要包括三个核心组件：TiDB Server，PD Server 和 TiKV Server。此外，还有用于解决用户复杂 OLAP 需求的 TiSpark 组件。 

![TiDB 架构图](./tidb-introducation_files/tidb-architecture.png)

### TiDB Server

TiDB Server 负责接收 SQL 请求，处理 SQL 相关的逻辑，并通过 PD 找到存储计算所需数据的 TiKV 地址，与 TiKV 交互获取数据，最终返回结果。TiDB Server 是无状态的，其本身并不存储数据，只负责计算，可以无限水平扩展，可以通过负载均衡组件（如LVS、HAProxy 或 F5）对外提供统一的接入地址。

### PD Server

Placement Driver (简称 PD) 是整个集群的管理模块，其主要工作有三个：一是存储集群的元信息（某个 Key 存储在哪个 TiKV 节点）；二是对 TiKV 集群进行调度和负载均衡（如数据的迁移、Raft group leader 的迁移等）；三是分配全局唯一且递增的事务 ID。

PD 是一个集群，需要部署奇数个节点，一般线上推荐至少部署 3 个节点。

### TiKV Server

TiKV Server负责存储数据，从外部看 TiKV 是一个分布式的提供事务的 Key-Value 存储引擎。存储数据的基本单位是 Region，每个 Region 负责存储一个 Key Range（从 StartKey 到 EndKey 的左闭右开区间）的数据，每个 TiKV 节点会负责多个 Region。TiKV 使用 Raft 协议做复制，保持数据的一致性和容灾。副本以 Region 为单位进行管理，不同节点上的多个 Region 构成一个 Raft Group，互为副本。数据在多个 TiKV 之间的负载均衡由 PD 调度，这里也是以 Region 为单位进行调度。
TiSpark

### TiSpark

作为 TiDB 中解决用户复杂 OLAP 需求的主要组件，将 Spark SQL 直接运行在 TiDB 存储层上，同时融合 TiKV 分布式集群的优势，并融入大数据社区生态。至此，TiDB 可以通过一套系统，同时支持 OLTP 与 OLAP，免除用户数据同步的烦恼。

## 部署TiDB

### 使用docker的方式部署TiDB

参考

[TiDB Docker 部署方案](https://www.pingcap.com/docs-cn/op-guide/docker-deployment/)

需要多台机器

[使用 Docker Compose 快速构建集群](https://www.pingcap.com/docs-cn/op-guide/docker-compose/)

介绍如何在单机上通过 Docker Compose 快速一键部署一套 TiDB 测试集群。 Docker Compose 可以通过一个 YAML 文件定义多个容器的应用服务，然后一键启动或停止。
>注 ：对于生产环境，不要使用 Docker Compose 进行部署，而应 使用 Ansible 部署 TiDB 集群 。
