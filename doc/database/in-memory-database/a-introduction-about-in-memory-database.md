# A INTRODUCTION ABOUT IN MEMORY DATABASE

一篇关于内存数据库的概述

## 技术选型

众多内存数据库中选择 Apache Ignite 和 VoltDB 进行研究

[数据库排名](https://db-engines.com/en/ranking)

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

## 本地环境测试

### 虚拟机中安装ubuntu

选择ubuntu为ubuntu-server

安装ubuntu-server，参考[Ubuntu 18.04 Server 版安装过程图文详解](https://blog.csdn.net/zhengchaooo/article/details/80145744)

安装ubuntu-server的时候就配置了代理

安装之后，配置了apt的国内镜像，选择的阿里镜像，可以参考 [配置阿里镜像](
https://blog.csdn.net/zhangjiahao14/article/details/80554616)

ubuntu中安装VoltDB社区版

安装jdk8

安装python2.7

看到网上大部分资料是使用源码编译安装的，我看了下好像apt可以直接安装python2.7

```Shell

安装python2.7
sudo apt-get install python

验证是否安装成功
python进入交互界面，可以看到python版本为2.7.15

```

安装gcc

安装make

```Shell
ubuntu怎么切换到root用户，我们都知道使用su root命令，去切换到root权限，此时会提示输入密码，可是怎么也输不对，提示“Authentication failure”，
此时有两种情况一个是真的是密码错了，另一种就是刚安装好的Linux系统，没有给root设置密码。

给root用户设置密码：
命令：sudo passwd root
输入密码，并确认密码。
```

### Ubuntu修改DNS

#### Ubuntu18.04 修改DNS

```Shell
sudo vim /etc/systemd/resolved.conf
```

修改如下：

```resolved.conf
[Resolve]

DNS=119.29.29.29
```

保存退出后

```Shell
systemctl restart systemd-resolved.service
```

#### DNS选择

DNSPod

>DNSPod：相比于去年今年的DNSPod在解析速度上，比以往要快上许多，国内最快节点：上海延迟3ms，最慢节点：新疆哈密延迟73ms\
\
DNS 服务器 IP 地址：\
首选：119.29.29.29\
备选：182.254.116.116

114DNS

>114DNS：作为国内用户最多的DNS，自然有他的强大之处，国内最快节点：江苏扬州延迟2ms，最慢节点：辽宁沈阳延迟123ms\
\
DNS 服务器 IP 地址：\
首选：114.114.114.114\
备选：114.114.114.115

[2018公共DNS服务器地址评估—DNS推荐](https://jingyan.baidu.com/article/6dad50753e6031a123e36e1f.html)

安装maven

安装ant
