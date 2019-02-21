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

### 编译VoltDB

#### 环境准备

需要如下：

- Java 1.8
- Apache Ant 1.7 or newer
- A compiler with C++11 support: GNU C++ 4.4 or newer or on OS X, XCode 5.0 (with Clang 3.3) or newer
- Python 2.6 or newer
- cmake 2.8 or newer

说明一下，我的系统是ubuntu-server 18.04

##### jdk1.8

可以通过oracle下载jdk然后进行安装（我是使用这种方式安装的），可以参考 [ubuntu16.04搭建jdk1.8运行环境](https://blog.csdn.net/smile_from_2015/article/details/80056297)

通过apt的方式，网上有这些资料，但是我没有成功

http://www.cnblogs.com/a2211009/p/4265225.html
https://www.cnblogs.com/HendSame-JMZ/p/6088262.html

##### apache ant

官网下载安装包进行安装

##### python2.7

通过如下命令安装的（我是使用的这种方式）

```Shell
sudo apt-get install python
```

##### 安装其他需要的组件

For Recent Ubuntu Linuxes (We test on 14.04, 16.04):

Run this command to install dependencies as packages:

从apache官网下载的ant安装的

我是自己通过apt安装的git，git-arch好像不能通过apt安装会提示找不到

```Shell
sudo apt-get -y install git
```

```Shell
sudo apt-get -y install build-essential ant-optional default-jdk python cmake \
    valgrind ntp ccache git-completion git-core git-svn git-doc \
    git-email python-httplib2 python-setuptools python-dev apt-show-versions
```

If JDK8 is not installed on 14.04, you can get it from Oracle.

16.04 defaults to JDK8, so that's the recommended platform for now.

参考 [GitHub-Building-VoltDB](https://github.com/VoltDB/voltdb/wiki/Building-VoltDB)

### 第一步

将VoltDB的bin文件夹添加到PATH变量中

```Shell
# 将VoltDB的bin文件夹添加到PATH变量中
sudo vim /etc/profile

# VoltDB
export VOLTDB_HOME=/home/yan/software/voltdb
export PATH=$PATH:${VOLTDB_HOME}/bin

# 使配置生效
source /etc/profile
```

验证下是否生效

```Shell
voltdb --version

控制台输出结果
voltdb version 9.0
```

#### 初始化root文件夹

初始化root文件夹

```Shell

# 使用命令初始化
voltdb init --dir ~/data/voltdb/mydb

# 终端输出如下内容
When using the INIT command, some deployment file settings (hostcount and voltdbroot path) are ignored
Initialized VoltDB root directory /home/yan/data/voltdb/mydb/voltdbroot

# 查看目录，下面产生了一个voltdbroot文件夹
# voltdbroot文件夹下生成了config  large_query_swap  log等文件夹

```

#### 启动一个single-server database

```Shell
# 启动一个single-server database
voltdb start --dir ~/data/voltdb/mydb --background

# 终端输出如下信息
WARNING: Unsupported release: Ubuntu release 18.04 bionic
INFO: Starting VoltDB server in the background...
INFO:   Output files are in "/home/yan/.voltdb_server".
Background process started with process ID 24628.

```

开启一个SQL控制台，来输入SQL DDL, DML 或者 DQL

```Shell
sqlcmd
```

登陆web-based VoltDB Management Console (VMC)

浏览器中打开`http://localhost:8080/`

#### 关闭运行的VoltDB

关闭 VoltDB cluster, 使用 shutdown 命令. 对于商业用户, 数据库中的内容会自动地被保存. 对于社区版本用户, 添加 --save 参数来手动保存数据库中的内容:

```Shell
voltadmin shutdown [--save]
```

现在可以使用下面的命令启动一个单节点数据库

```Shell
voltdb start [--dir ~/mydb] [--background]
```

### 下一步

#### 示例

参考VoltDB下面的examples文件夹，可以找到一些示例