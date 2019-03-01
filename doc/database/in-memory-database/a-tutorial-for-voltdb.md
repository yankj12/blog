# 一篇VoltDB教程

- [一篇VoltDB教程](#%E4%B8%80%E7%AF%87voltdb%E6%95%99%E7%A8%8B)
  - [安装VoltDB](#%E5%AE%89%E8%A3%85voltdb)
    - [环境准备](#%E7%8E%AF%E5%A2%83%E5%87%86%E5%A4%87)
      - [jdk1.8](#jdk18)
      - [apache ant](#apache-ant)
      - [python2.7](#python27)
      - [安装其他需要的组件](#%E5%AE%89%E8%A3%85%E5%85%B6%E4%BB%96%E9%9C%80%E8%A6%81%E7%9A%84%E7%BB%84%E4%BB%B6)
    - [编译VoltDB](#%E7%BC%96%E8%AF%91voltdb)
    - [配置环境变量](#%E9%85%8D%E7%BD%AE%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
    - [VoltDB启动及关闭](#voltdb%E5%90%AF%E5%8A%A8%E5%8F%8A%E5%85%B3%E9%97%AD)
      - [初始化root文件夹](#%E5%88%9D%E5%A7%8B%E5%8C%96root%E6%96%87%E4%BB%B6%E5%A4%B9)
      - [启动一个single-server database](#%E5%90%AF%E5%8A%A8%E4%B8%80%E4%B8%AAsingle-server-database)
      - [关闭运行的VoltDB](#%E5%85%B3%E9%97%AD%E8%BF%90%E8%A1%8C%E7%9A%84voltdb)
  - [VoltDB入门示例](#voltdb%E5%85%A5%E9%97%A8%E7%A4%BA%E4%BE%8B)
    - [引言](#%E5%BC%95%E8%A8%80)
    - [第一部分 创建数据库](#%E7%AC%AC%E4%B8%80%E9%83%A8%E5%88%86-%E5%88%9B%E5%BB%BA%E6%95%B0%E6%8D%AE%E5%BA%93)
      - [加载数据结构](#%E5%8A%A0%E8%BD%BD%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
    - [第二部分 加载和管理数据](#%E7%AC%AC%E4%BA%8C%E9%83%A8%E5%88%86-%E5%8A%A0%E8%BD%BD%E5%92%8C%E7%AE%A1%E7%90%86%E6%95%B0%E6%8D%AE)
      - [重启数据库](#%E9%87%8D%E5%90%AF%E6%95%B0%E6%8D%AE%E5%BA%93)
      - [加载数据](#%E5%8A%A0%E8%BD%BD%E6%95%B0%E6%8D%AE)
      - [查询数据库](#%E6%9F%A5%E8%AF%A2%E6%95%B0%E6%8D%AE%E5%BA%93)
    - [第三部分 分区](#%E7%AC%AC%E4%B8%89%E9%83%A8%E5%88%86-%E5%88%86%E5%8C%BA)
      - [分区表](#%E5%88%86%E5%8C%BA%E8%A1%A8)
      - [重复表](#%E9%87%8D%E5%A4%8D%E8%A1%A8)
    - [第四部分 数据结构更新和稳固性（Durability）](#%E7%AC%AC%E5%9B%9B%E9%83%A8%E5%88%86-%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E6%9B%B4%E6%96%B0%E5%92%8C%E7%A8%B3%E5%9B%BA%E6%80%A7durability)
      - [保存数据库](#%E4%BF%9D%E5%AD%98%E6%95%B0%E6%8D%AE%E5%BA%93)
    - [第五部分 存储过程](#%E7%AC%AC%E4%BA%94%E9%83%A8%E5%88%86-%E5%AD%98%E5%82%A8%E8%BF%87%E7%A8%8B)
      - [简单的存储过程](#%E7%AE%80%E5%8D%95%E7%9A%84%E5%AD%98%E5%82%A8%E8%BF%87%E7%A8%8B)
      - [复杂的存储过程](#%E5%A4%8D%E6%9D%82%E7%9A%84%E5%AD%98%E5%82%A8%E8%BF%87%E7%A8%8B)
  - [待研究问题](#%E5%BE%85%E7%A0%94%E7%A9%B6%E9%97%AE%E9%A2%98)
    - [voltdb的表原理是怎么做的](#voltdb%E7%9A%84%E8%A1%A8%E5%8E%9F%E7%90%86%E6%98%AF%E6%80%8E%E4%B9%88%E5%81%9A%E7%9A%84)
    - [voltdb中的key-value](#voltdb%E4%B8%AD%E7%9A%84key-value)
    - [voltdb中的事务](#voltdb%E4%B8%AD%E7%9A%84%E4%BA%8B%E5%8A%A1)

## 安装VoltDB

### 环境准备

需要如下：

- Java 1.8
- Apache Ant 1.7 or newer
- A compiler with C++11 support: GNU C++ 4.4 or newer or on OS X, XCode 5.0 (with Clang 3.3) or newer
- Python 2.6 or newer
- cmake 2.8 or newer

说明一下，我的系统是ubuntu-server 18.04

#### jdk1.8

可以通过oracle下载jdk然后进行安装（我是使用这种方式安装的），可以参考 [ubuntu16.04搭建jdk1.8运行环境](https://blog.csdn.net/smile_from_2015/article/details/80056297)

通过apt的方式，网上有这些资料，但是我没有成功

- [Ubuntu 安装 JDK 7 / JDK8 的两种方式](http://www.cnblogs.com/a2211009/p/4265225.html)
- [ubuntu通过apt-get安装JDK8](https://www.cnblogs.com/HendSame-JMZ/p/6088262.html)

或者通过apt安装openjdk

#### apache ant

官网下载安装包进行安装

#### python2.7

通过如下命令安装的（我是使用的这种方式）

```Shell
sudo apt-get install python
```

#### 安装其他需要的组件

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

### 编译VoltDB

```Shell

# 获取源码
git clone https://github.com/VoltDB/voltdb.git

# 编译
cd voltdb
ant

```

### 配置环境变量

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

### VoltDB启动及关闭

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

## VoltDB入门示例

参考VoltDB下面的examples文件夹，可以找到一些示例（但是我参考的是官方tutorial文档）

### 引言

可以访问下方的地址获取教程中的相关数据

`http://downloads.voltdb.com/technologies/other/tutorial_files_50.zip`

### 第一部分 创建数据库

#### 加载数据结构

可以在sqlcmd中手动输入相关sql语句，或者可以通过导入文件的方式加载。

```SQL
# 创建表
create table towns(
    town varchar(64),
    county varchar(64),
    state varchar(2)
);

# 插入数据
insert into towns values ('Billerica','Middlesex','MA');
insert into towns values ('Buffalo','Erie','NY');
insert into towns values ('Bay View','Erie','OH');

# 查询
select * from towns;

select count(*) as total from towns;
```

### 第二部分 加载和管理数据

当使用create table定义表的时候，VoltDB为每个表自动创建了insert记录的存储过程。

有一个命令使用这些默认的存储过程，这样我们可以使用这个命令将数据文件加载到数据库中。

`csvloader`命令读取数据文件（比如comma-separated value CSV文件），并通过默认的存储过程将每一行数据写入到指定的表中。

从[如下地址](http://geonames.usgs.gov/domestic/download_data.htm)中下载Populated Places的数据，文件中列会比较多，我们可以减少到我们需要的列

将建表语句保存到towns.sql文件中

```SQL
CREATE TABLE towns (
    town VARCHAR(64),
    state VARCHAR(2),
    state_num TINYINT NOT NULL,
    county VARCHAR(64),
    county_num SMALLINT NOT NULL,
    elevation INTEGER
);

```

使用cut命令切割和组装数据

```Shell
cut --delimiter="|" --fields=2,4-7,16 POP_PLACES_20181201.txt \
| grep -v "|$" \
| grep -v "||" > data/towns.txt
```

#### 重启数据库

因为towns表的数据结构发生了变化，我们可以使用drop和create命令删除并重新生成对应的表结构。

但是现在我们使用之前学到的命令重新初始化root目录并且启动一个空的数据库。在`voltdb init`命令后添加`--force`参数，来表明我们不需要上个会话中的日志和快照。这次我们使用`FILE`指令加载数据结构。

```Shell
[terminal 1]
$ voltdb init --force
$ voltdb start

[terminal 2]
$ sqlcmd
1> FILE towns.sql;
Command succeeded.
2> exit
```

我是在towns.sql文件所在的目录中执行sqlcmd，并使用FILE命令的。

#### 加载数据

当数据库启动并且数据结构被加载了，我们准备去插入我们的新数据。为了做这些，进入`/data`文件夹，使用`csvloader`加载数据文件

```Shell
$ cd data
$ csvloader --separator "|" --skip 1 \
--file towns.txt towns

WARN: Strict java memory checking is enabled, don't do release builds or performance runs with this enabled. Invoke "ant clean" and "ant -Djmemcheck=NO_MEMCHECK" to disable.
Read 195099 rows from file and successfully inserted 195099 rows (final)
Elapsed time: 7.055 seconds
Invalid row file: /home/yan/csvloader_TOWNS_insert_invalidrows.csv
Log file: /home/yan/csvloader_TOWNS_insert_log.log
Report file: /home/yan/csvloader_TOWNS_insert_report.log

```

前面的命令中：

- `--separator` 参数让我们指定分隔符，将一行数据分隔成多列。GNIS data不是标准的CSV文件，我们使用`--separator`来指定正确的定界符。
- 数据文件带有一行表头。`--skip 1`参数告诉`csvloader`跳过第一行。
- `--file`参数告诉`csvloader`哪个文件作为输入。如果没有指定文件`csvloader`使用标准输入作为输入源。
- 参数`towns`表示将数据加载进入哪个表。

`csvloader`加载所有数据进入数据库，生成了三个日志文件：

- 一个列出了发生测所有错误
- 一个列出了所有不能正确加载的数据
- 最后一个总结报告，包括话费的时间和加载的数条数

上面使用的`csvloader`命令生成的这三个文件分别是

- csvloader_TOWNS_insert_log.log
- csvloader_TOWNS_insert_invalidrows.csv
- csvloader_TOWNS_insert_report.log

#### 查询数据库

```SQL
$ sqlcmd

SELECT town,state,elevation from towns order by elevation desc limit 5;

select town, count(town) as duplicates from towns group by town order by duplicates desc limit 5;

```

还可以进行多表关联查询

```SQL
CREATE TABLE towns (
    town VARCHAR(64),
    state VARCHAR(2),
    state_num TINYINT NOT NULL,
    county VARCHAR(64),
    county_num SMALLINT NOT NULL,
    elevation INTEGER
);

CREATE TABLE people (
    state_num TINYINT NOT NULL,
    county_num SMALLINT NOT NULL,
    state VARCHAR(20),
    county VARCHAR(64),
    population INTEGER
);
CREATE INDEX town_idx ON towns (state_num, county_num);
CREATE INDEX people_idx ON people (state_num, county_num);

# 裁剪数据
grep -v "^040," CO-EST2011-Alldata.csv \
| cut --delimiter="," --fields=4-8 > people.txt

# 导入数据到people表
cd data
csvloader --file people.txt --skip 1 people

# 多表关联
select top 5 min(t.elevation) as height,
t.state,t.county, max(p.population)
from towns as t, people as p
where t.state_num=p.state_num and t.county_num=p.county_num
group by t.state, t.county order by height desc;

```

### 第三部分 分区

分区功能（partitioning）将表分割成自治的单元。类似于分片（sharding）

- voltdb根据指定的分区列自动进行分区，不需要你手动分区。
- 你可以在单服务器上有多个分区或者site。换句话说分区不仅仅是为了扩展数据规模，还提高性能。
- voltdb不仅将数据分区，也将对数据的处理进行分区，这就是voltdb提高吞吐和并行的原因。

#### 分区表

将分区列作为表结构的一部分就可以对这个表进行分区。分区之后，每条数据在插入时会根据分区列决定插入到哪个分区。比如我们对towns表根据name分区，所有相同name的记录会在一个分区中。

虽然根据name分区可能从数据均匀性方面有一定原因，但是分区的目的是将数据和对数据的处理进行分发。我们很少对相同名称的towns数据进行对比，反而是经常对一个地理区域中的数据进行比较。所以我们根据state分区可以很快速地计算出给定state中最大或者最高的town。

towns和people表都有state列，但是一个使用的是缩写（abbreviation）一个使用的是全称，我们可以使用state_num列进行分区。

```SQL
PARTITION TABLE towns ON COLUMN state_num;

# 报错信息如下
VOLTDB ERROR:   Unable to make requested schema change:
  Unable to change whether table TOWNS is replicated because it is not empty.
  Unable to change the partition column of table TOWNS because it is not empty.

```

```Shell
voltadmin shutdown
```

`voltadmin --shutdown`命令会有序地关闭database不论是单节点还是多节点集群。

使用`FILE`命令重新加载数据结构（带有分区），使用`csvloader`命令加载数据。

分区之后，导入数据的效率也得到了提升。

#### 重复表

之前提到的两个表towns和people都有state name这一列，但是不一致（一个是全称，一个是缩写）。作为替代，我们使用state_num列进行分区和join。

state_num是数字，是美国政府官方的并且唯一的标识符，作为计算很有用，但是大部分日常使用中考虑地名使用的是名字而不是数字。所以有一个一致的名字是有必要的。

我们可以创建一个独立于towns和people表的查找表（lookup table），而非在每个表中修改数据结构。可以使用查找表来为每个state_num提供权威的名称。

```SQL
CREATE TABLE states (
  abbreviation VARCHAR(20),
  state_num TINYINT,
  name VARCHAR(20),
  PRIMARY KEY (state_num)
);
```

这种类型的查找表在关系型数据库中很常见，它们减少冗余并且提供一致性。这种查找表往往有两个特性，比较小、数据很少变化（或者不变）。

我们可以根据state_num对states表进行分区，但是当查找表数据比较小并且很少更新的时候，更好的做法是将它复制到各个分区

This way, even if another table is partitioned (such as a customer table partitioned on last name), stored procedures can join the two tables, no matter what partition the procedure executes in.

>Tables where all the records are available to all the partitions are called replicated tables. Note that tables are replicated by default. 

所以，我们只要不指定分区，这些表默认都是复制表。

关于(concerning)复制表的警告(caveat):

复制表的优点在于它能够被每个分区读到。但是缺点是对复制表的任务修改或者写入，都必须一次对所有分区执行。这种多分区的存储（我理解的是一次性对所有分区修改或者写入）过程减少了并行处理性能，并影响了数据库吞吐。这也是为什么我们不应该将频繁更新的表作为复制表。

### 第四部分 数据结构更新和稳固性（Durability）

快速开发的时候可以通过关闭、启动、重新初始化等方式更新数据结构。但是数据已经在运行了，或者生产环境等情况下，就不能这么做了。

#### 保存数据库

VoltDB是个内存数据库，每次你通过init和start命令重新初始化并且启动的时候，都会启动一个新的空的database。显然在生产环境中你希望数据持久化。VoltDB提供了多种特性跨事务保存数据库。

持久化数据库最简便的方式是使用`command logging`，企业版默认开启。`command logging`记录了所有的数据库活动，包括数据结构和数据的变化。如果数据库停了，通过`voltdb start`命令就可以恢复`command logging`。

```Shell
voltadmin shutdown
voltdb start
```

`Command logging`让保存和恢复数据库简便并且自动。
如果你使用的是社区版本或者没有使用`Command logging`，你可以通过快照（snapshot）来保存和恢复你的数据库。

快照是数据库一种完全磁盘化的呈现，包含所有恢复数据库的数据。你可以在任何时间通过`voltadmin save`命令创建快照。

默认情况下快照保存到database root目录的下级文件夹中。你可以在`oltadmin save`命令后面增加参数来指定快照的位置和名称。但是默认位置的快照有个好处就是，`voltdb start`命令会自动地恢复（restore）默认位置的快照。

更简便些，可以在`shutdown`命令后面加上`--save`参数，这样voltdb在关闭的时候会创建一个最后的快照。社区版本推荐这种方式。

```Shell
voltadmin shutdown --save
voltdb start
```

### 第五部分 存储过程

#### 简单的存储过程

简单的存错过程在这里是指一条sql可以解决的

简单的存储过程可以通过`CREATE PROCEDURE 存储过程名称 AS`创建

例如：

```SQL
CREATE PROCEDURE leastpopulated AS
  SELECT TOP 1 county, abbreviation, population
  FROM people, states WHERE people.state_num=?
  AND people.state_num=states.state_num
  ORDER BY population ASC;
```

我们可以通过分区键指定存储过程在哪个分区执行。

我们可以在存储过程中多表关联，但是需要注意的是访问的所有表都需要使用的是相同的分区键并且分区键对应的值相同，或者访问的是复制表

```SQL
CREATE PROCEDURE leastpopulated
  PARTITION ON TABLE people COLUMN state_num
AS
  SELECT TOP 1 county, abbreviation, population
    FROM people, states WHERE people.state_num=?
    AND people.state_num=states.state_num
    ORDER BY population ASC;
```

使用存储过程

```SQL
exec leastpopulated 6;

COUNTY ABBREVIATION POPULATION
-------------- ------------- -----------
Alpine County CA 1175
```

#### 复杂的存储过程

voltdb复杂的存储过程是先用java语法

## 待研究问题

### voltdb的表原理是怎么做的

### voltdb中的key-value

### voltdb中的事务