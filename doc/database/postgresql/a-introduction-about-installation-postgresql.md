# 安装PostgreSQL

- [安装PostgreSQL](#%E5%AE%89%E8%A3%85PostgreSQL)
  - [前言](#%E5%89%8D%E8%A8%80)
  - [Ubuntu上二进制包安装PG](#Ubuntu%E4%B8%8A%E4%BA%8C%E8%BF%9B%E5%88%B6%E5%8C%85%E5%AE%89%E8%A3%85PG)
  - [Ubuntu上源码安装PG](#Ubuntu%E4%B8%8A%E6%BA%90%E7%A0%81%E5%AE%89%E8%A3%85PG)
    - [环境准备](#%E7%8E%AF%E5%A2%83%E5%87%86%E5%A4%87)
    - [下载源码](#%E4%B8%8B%E8%BD%BD%E6%BA%90%E7%A0%81)
    - [安装PG](#%E5%AE%89%E8%A3%85PG)
      - [configure](#configure)
      - [build](#build)
      - [install](#install)
    - [安装后配置](#%E5%AE%89%E8%A3%85%E5%90%8E%E9%85%8D%E7%BD%AE)
      - [Shared Libraries](#Shared-Libraries)
      - [Environment Variables](#Environment-Variables)
  - [启动和停止pg](#%E5%90%AF%E5%8A%A8%E5%92%8C%E5%81%9C%E6%AD%A2pg)
    - [初始化PG](#%E5%88%9D%E5%A7%8B%E5%8C%96PG)
    - [启动pg](#%E5%90%AF%E5%8A%A8pg)
    - [停止pg](#%E5%81%9C%E6%AD%A2pg)
    - [pg_ctl的帮助文档](#pgctl%E7%9A%84%E5%B8%AE%E5%8A%A9%E6%96%87%E6%A1%A3)
  - [其他设置](#%E5%85%B6%E4%BB%96%E8%AE%BE%E7%BD%AE)
    - [允许远程连接](#%E5%85%81%E8%AE%B8%E8%BF%9C%E7%A8%8B%E8%BF%9E%E6%8E%A5)
    - [修改pg数据库用户postgres的密码](#%E4%BF%AE%E6%94%B9pg%E6%95%B0%E6%8D%AE%E5%BA%93%E7%94%A8%E6%88%B7postgres%E7%9A%84%E5%AF%86%E7%A0%81)
      - [1.修改PostgreSQL数据库默认用户postgres的密码](#1%E4%BF%AE%E6%94%B9PostgreSQL%E6%95%B0%E6%8D%AE%E5%BA%93%E9%BB%98%E8%AE%A4%E7%94%A8%E6%88%B7postgres%E7%9A%84%E5%AF%86%E7%A0%81)
      - [2.修改linux系统postgres用户的密码](#2%E4%BF%AE%E6%94%B9linux%E7%B3%BB%E7%BB%9Fpostgres%E7%94%A8%E6%88%B7%E7%9A%84%E5%AF%86%E7%A0%81)
    - [安装PG文档精简版](#%E5%AE%89%E8%A3%85PG%E6%96%87%E6%A1%A3%E7%B2%BE%E7%AE%80%E7%89%88)
      - [安装前linux环境准备](#%E5%AE%89%E8%A3%85%E5%89%8Dlinux%E7%8E%AF%E5%A2%83%E5%87%86%E5%A4%87)
      - [解压pg压缩包](#%E8%A7%A3%E5%8E%8Bpg%E5%8E%8B%E7%BC%A9%E5%8C%85)
      - [编译安装](#%E7%BC%96%E8%AF%91%E5%AE%89%E8%A3%85)
      - [修改配置文件](#%E4%BF%AE%E6%94%B9%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
      - [添加用户postgres](#%E6%B7%BB%E5%8A%A0%E7%94%A8%E6%88%B7postgres)
      - [初始化pg的database](#%E5%88%9D%E5%A7%8B%E5%8C%96pg%E7%9A%84database)
      - [PG允许远程连接](#PG%E5%85%81%E8%AE%B8%E8%BF%9C%E7%A8%8B%E8%BF%9E%E6%8E%A5)
      - [修改PG数据库用户postgres的密码](#%E4%BF%AE%E6%94%B9PG%E6%95%B0%E6%8D%AE%E5%BA%93%E7%94%A8%E6%88%B7postgres%E7%9A%84%E5%AF%86%E7%A0%81)
      - [启动和停止命令](#%E5%90%AF%E5%8A%A8%E5%92%8C%E5%81%9C%E6%AD%A2%E5%91%BD%E4%BB%A4)
    - [启动PG数据库](#%E5%90%AF%E5%8A%A8PG%E6%95%B0%E6%8D%AE%E5%BA%93)
  - [Windows上安装PG](#Windows%E4%B8%8A%E5%AE%89%E8%A3%85PG)
  - [常见问题](#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)
    - [宿主机上链接不到虚拟机中的pg](#%E5%AE%BF%E4%B8%BB%E6%9C%BA%E4%B8%8A%E9%93%BE%E6%8E%A5%E4%B8%8D%E5%88%B0%E8%99%9A%E6%8B%9F%E6%9C%BA%E4%B8%AD%E7%9A%84pg)
  - [示例](#%E7%A4%BA%E4%BE%8B)
    - [建表](#%E5%BB%BA%E8%A1%A8)
    - [导入数据](#%E5%AF%BC%E5%85%A5%E6%95%B0%E6%8D%AE)
  - [PostgreSQL常用概念](#PostgreSQL%E5%B8%B8%E7%94%A8%E6%A6%82%E5%BF%B5)
  - [psql操作](#psql%E6%93%8D%E4%BD%9C)
    - [psql使用shell脚本操作postgres](#psql%E4%BD%BF%E7%94%A8shell%E8%84%9A%E6%9C%AC%E6%93%8D%E4%BD%9Cpostgres)

## 前言

看了下下载和安装PG的官方资料 [PostgreSQL Core Distribution](https://www.postgresql.org/download/)，其中介绍了集中安装方式

- 二进制包
- 源码安装
- 第三方分发

看完之后，感觉比较靠谱的还是二进制包、源码安装这两种方式。

## Ubuntu上二进制包安装PG

可以参考官方资料 [apt安装PostgreSQL](https://www.postgresql.org/download/linux/ubuntu/)

因为公司网络原因，二进制包安装的方式不适合。

我看了下好像使用`sudo apt-get install postgresql`安装，不过安装的是10版本

## Ubuntu上源码安装PG

[Installation from Source Code](https://www.postgresql.org/docs/current/installation.html)

### 环境准备

编译PostgreSQL需要的软件

下面列出的是必须的软件

- make 3.80或者以上版本

    通过make --version查看版本

- gcc
- tar
- GNU Readline library
  
    `sudo apt-get install libreadline-dev`

- zlib compression library

    `sudo apt-get install zlib1g-dev`

The following packages are optional. They are not required in the default configuration, but they are needed when certain build options are enabled, as explained below:

- libperl

    `sudo apt-get install libperl-dev libperl5.26`

- python libpython

    `sudo apt-get install libpython-dev libpython2.7 libpython2.7-dev`，ubuntu环境上安装python2.7后已经有这些了

- tcl8.4

    sudo apt-get install tcl8.5

if you are building from a Git tree instead of using a released source package, or if you want to do server development, you also need the following packages:

- flex和bison

    sudo apt-get install flex bison

- perl5.8.3版本以上

    sudo apt-get install perl6

### 下载源码

The PostgreSQL 11.2 sources can be obtained from the download section of our website: https://www.postgresql.org/download/. You should get a file named postgresql-11.2.tar.gz or postgresql-11.2.tar.bz2.

获取源码后进行解压

```Shell
gunzip postgresql-11.2.tar.gz
tar xf postgresql-11.2.tar
```

### 安装PG

我都是用的是默认的参数，如果想自定义，可以参考 [官方文档-安装PG](https://www.postgresql.org/docs/current/install-procedure.html)

默认参数会安装到`/usr/local/pgsql`路径下

#### configure

我使用的是默认的命令`./configure`

#### build

使用的是编译所有可以编译的，使用命令`make world`

#### install

安装这一步需要拥有安装目录的权限，一般使用root用户进行安装

如果上一步使用`make world`进行的编译，现在使用`make install-world`命令进行安装

### 安装后配置

#### Shared Libraries

On some systems with shared libraries you need to tell the system how to find the newly installed shared libraries.

```/etc/profile
LD_LIBRARY_PATH=/usr/local/pgsql/lib
export LD_LIBRARY_PATH
```

#### Environment Variables

If you installed into `/usr/local/pgsql` or some other location that is not searched for programs by default, you should add `/usr/local/pgsql/bin` (or whatever you set `--bindir` to in Step 1) into your PATH. Strictly speaking, this is not necessary, but it will make the use of PostgreSQL much more convenient.

虽然这一步不是必需，但是这样做会让我们使用PostgreSQL更加方便。

To do this, add the following to your shell start-up file, such as `~/.bash_profile` (or `/etc/profile`, if you want it to affect all users):

```/etc/profile
PATH=/usr/local/pgsql/bin:$PATH
export PATH
```

To enable your system to find the man documentation, you need to add lines like the following to a shell start-up file unless you installed into a location that is searched by default:

```/etc/profile
MANPATH=/usr/local/pgsql/share/man:$MANPATH
export MANPATH
```

## 启动和停止pg

### 初始化PG

```Shell
postgres@middleware:~$ initdb -D /usr/local/pgsql/data
initdb: command not found
postgres@middleware:~$ source /etc/profile
postgres@middleware:~$ initdb -D /usr/local/pgsql/data
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.UTF-8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /usr/local/pgsql/data ... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting dynamic shared memory implementation ... posix
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

WARNING: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.

Success. You can now start the database server using:

    pg_ctl -D /usr/local/pgsql/data -l logfile start

postgres@middleware:~$ 
```

或者，可以通过`pg_ctl`程序来执行`initdb`

```Shell
postgres$ pg_ctl -D /usr/local/pgsql/data initdb
```

如果使用`pg_ctl`启动和停止数据库的话，上面这种方式可能更加符合使用习惯

如果指定的数据文件夹不存在,`initdb`会去尝试创建,所以需要`initdb`拥有在父级文件夹写的权限(实际上是postgres拥有在父级文件夹写的权限)否则会报错。

```Shell
root# mkdir /usr/local/pgsql
root# chown postgres /usr/local/pgsql
root# su postgres
postgres$ initdb -D /usr/local/pgsql/data
```

如果data文件夹中存在数据，initdb命令不会执行，这样可以避免意外操作导致将数据覆盖。

### 启动pg

```Shell
postgres@middleware:~$ pg_ctl -D /usr/local/pgsql/data -l /var/lib/postgresql/pgsql_logs/pgsql.log start
```

### 停止pg

使用pg_ctl停止服务

```Shell
postgres@middleware:~$ pg_ctl -D /usr/local/pgsql/data stop
```

### pg_ctl的帮助文档

查看pg_ctl的帮助文档

```Shell
postgres@middleware:~$ pg_ctl --help
pg_ctl is a utility to initialize, start, stop, or control a PostgreSQL server.

Usage:
  pg_ctl init[db] [-D DATADIR] [-s] [-o OPTIONS]
  pg_ctl start    [-D DATADIR] [-l FILENAME] [-W] [-t SECS] [-s]
                  [-o OPTIONS] [-p PATH] [-c]
  pg_ctl stop     [-D DATADIR] [-m SHUTDOWN-MODE] [-W] [-t SECS] [-s]
  pg_ctl restart  [-D DATADIR] [-m SHUTDOWN-MODE] [-W] [-t SECS] [-s]
                  [-o OPTIONS] [-c]
  pg_ctl reload   [-D DATADIR] [-s]
  pg_ctl status   [-D DATADIR]
  pg_ctl promote  [-D DATADIR] [-W] [-t SECS] [-s]
  pg_ctl kill     SIGNALNAME PID

Common options:
  -D, --pgdata=DATADIR   location of the database storage area
  -s, --silent           only print errors, no informational messages
  -t, --timeout=SECS     seconds to wait when using -w option
  -V, --version          output version information, then exit
  -w, --wait             wait until operation completes (default)
  -W, --no-wait          do not wait until operation completes
  -?, --help             show this help, then exit
If the -D option is omitted, the environment variable PGDATA is used.

Options for start or restart:
  -c, --core-files       allow postgres to produce core files
  -l, --log=FILENAME     write (or append) server log to FILENAME
  -o, --options=OPTIONS  command line options to pass to postgres
                         (PostgreSQL server executable) or initdb
  -p PATH-TO-POSTGRES    normally not necessary

Options for stop or restart:
  -m, --mode=MODE        MODE can be "smart", "fast", or "immediate"

Shutdown modes are:
  smart       quit after all clients have disconnected
  fast        quit directly, with proper shutdown (default)
  immediate   quit without complete shutdown; will lead to recovery on restart

Allowed signal names for kill:
  ABRT HUP INT KILL QUIT TERM USR1 USR2

Report bugs to <pgsql-bugs@postgresql.org>.
```

## 其他设置

### 允许远程连接

打开postgresql的服务启动配置文件`postgres.conf`（有可能文件名为`postgresql.conf`）一般情况下在data文件夹下：

找到`listen_address`配置节，把`local`改成`*`。如果`listen_address`被注释掉了，那么就要取消注释。

打开postgresql的远程访问配置文件`pg_hba.conf`，一般情况下在data文件夹下：

打开没有注释掉的代码

```pg_hba.conf
host    all    all    127.0.0.1/32    md5
```

把它改成：

```pg_hba.conf
host    all    all    0.0.0.0/0        password
```

### 修改pg数据库用户postgres的密码

修改pg数据库默认用户postgres的密码，需要两步：1、修改数据库用户的密码；1、修改linux用户的密码

#### 1.修改PostgreSQL数据库默认用户postgres的密码

PostgreSQL数据库创建一个postgres用户作为数据库的管理员，密码随机，所以需要修改密码，方式如下：

步骤一：登录PostgreSQL

```shell
sudo -u postgres psql
```

步骤二：修改登录PostgreSQL密码

```psql
ALTER USER postgres WITH PASSWORD 'postgres';
```

注：

    密码postgres要用引号引起来
    命令最后有分号

步骤三：退出PostgreSQL客户端

```psql
\q
```

回到顶部

#### 2.修改linux系统postgres用户的密码

PostgreSQL会创建一个默认的linux用户postgres，修改该用户密码的方法如下：

步骤一：删除用户postgres的密码

```shell
sudo  passwd -d postgres
```

步骤二：设置用户postgres的密码

```shell
sudo -u postgres passwd
```

系统提示输入新的密码

```shell
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

参考 [修改postgres密码](http://www.cnblogs.com/kaituorensheng/p/4735191.html)

### 安装PG文档精简版

#### 安装前linux环境准备

```shell

sudo apt install -y make gcc tar libreadline-dev zlib1g-dev libperl-dev libperl5.26 tcl8.5 flex bison perl6

# 如果安装过python2.7，那么下面的额软件已经安装过了
sudo apt install -y libpython-dev libpython2.7 libpython2.7-dev
```

#### 解压pg压缩包

```shell
gunzip postgresql-11.2.tar.gz
tar xf postgresql-11.2.tar
```

#### 编译安装

```shell
cd postgresql-11.2

./configure

make world

sudo make install-world
```

#### 修改配置文件

```shell
sudo vi /etc/profile

# postgresql
LD_LIBRARY_PATH=/usr/local/pgsql/lib
export LD_LIBRARY_PATH

PATH=/usr/local/pgsql/bin:$PATH
export PATH

MANPATH=/usr/local/pgsql/share/man:$MANPATH
export MANPATH
```

#### 添加用户postgres

```shell
sudo useradd -m -s /bin/bash postgres

sudo passwd postgres
```

#### 初始化pg的database

```shell
#切换到postgres用户

su - postgres

#创建data和log的文件夹
mkdir data logs

#初始化database
initdb -D /home/postgres/data
```

#### PG允许远程连接

打开postgresql的服务启动配置文件`postgres.conf`（有可能文件名为`postgresql.conf`）一般情况下在data文件夹下：

找到`listen_address`配置节，把`local`改成`*`。如果`listen_address`被注释掉了，那么就要取消注释。

打开postgresql的远程访问配置文件`pg_hba.conf`，一般情况下在data文件夹下：

打开没有注释掉的代码
把它改成：

```pg_hba.conf
host    all    all    0.0.0.0/0        password
```

#### 修改PG数据库用户postgres的密码

修改pg数据库默认用户postgres的密码，需要两步：1、修改数据库用户的密码；1、修改linux用户的密码

修改PostgreSQL数据库默认用户postgres的密码

PostgreSQL数据库创建一个postgres用户作为数据库的管理员，密码随机，所以需要修改密码，方式如下：

步骤一：登录PostgreSQL

```shell
sudo -u postgres psql
```

步骤二：修改登录PostgreSQL密码

```psql
ALTER USER postgres WITH PASSWORD 'postgres';
```

注：密码postgres要用引号引起来命令最后有分号

步骤三：退出PostgreSQL客户端

```psql
\q
```

修改linux用户postgres用户，使用sudo passwd修改即可

#### 启动和停止命令

```shell
#启动
pg_ctl -D /home/postgres/data -l /home/postgres/logs/postgres.out start

#停止
pg_ctl -D /home/postgres/data stop
```

### 启动PG数据库

[Chapter 18. Server Setup and Operation](https://www.postgresql.org/docs/current/runtime.html)

## Windows上安装PG

我在自己电脑上安装的时候，选择语言的时候，选择的简体中文，无法安装成功，选择local，安装成功了。

需要注意的是，默认root用户的用户名不是root，而是postgres

## 常见问题

### 宿主机上链接不到虚拟机中的pg

在宿主机上链接虚拟机中的pg，报错信息如下

```err
FATAL: no pg_hba.conf entry for host "192.168.19.1", user "postgres", database "postgres"
  FATAL: no pg_hba.conf entry for host "192.168.19.1", user "postgres", database "postgres"
  FATAL: no pg_hba.conf entry for host "192.168.19.1", user "postgres", database "postgres"
```

宿主机连接虚拟机中pg，需要设置远程访问。还需要设置用户的密码

## 示例

### 建表

数据类型参考`postgresql-11-A4.pdf`中`Part II. The SQL Language`部分`Chapter 8. Data Types`章节

```sql
CREATE TABLE towns (
   town character(64),
   state character(2),
   state_num smallint NOT NULL,
   county character(64),
   county_num smallint NOT NULL,
   elevation INTEGER
);
```

### 导入数据

```pg
COPY towns
FROM '/var/lib/postgresql/data/towns.txt' DELIMITER '|' CSV HEADER;
```

1. 在`COPY`关键字后指定表名`towns`
2. 在`FROM`关键字后指定数据文件`/var/lib/postgresql/data/towns.txt`，因为使用的是文本文件，需要使用关键字`DELIMITER`和`CSV`
3. 关键字`HEADER`表示数据文件有一行表头

参考 [Import CSV File Into PosgreSQL Table](http://www.postgresqltutorial.com/import-csv-file-into-posgresql-table/)

## PostgreSQL常用概念

模式

数据库

参考 [PostgreSQL模式介绍](http://www.mamicode.com/info-detail-1394118.html)

## psql操作

```pg
\dn [模式]       列出模式 (加 "+" 获取更多的信息)
\dp [模式]       列出表, 视图, 序列的访问权限
\l               列出所有数据库 (加 "+" 获取更多的信息)

```

参考 [psql操作手册](http://www.cnblogs.com/happyhotty/articles/1920455.html)

### psql使用shell脚本操作postgres

psql -U postgres -w -c "COPY rank TO '/etc/www.csv' WITH csv"

形式就是  ： psql -U username -c 要执行的操作【一定要把操作写在引号内】

```psql
  -c, --command=COMMAND    run only single command (SQL or internal) and exit
  -U, --username=USERNAME  database user name (default: "postgres")
  -w, --no-password        never prompt for password
  -W, --password           force password prompt (should happen automatically)
```

psql 操作的参数可以 --help

```psql
postgres@middleware:~$ psql --help
psql is the PostgreSQL interactive terminal.

Usage:
  psql [OPTION]... [DBNAME [USERNAME]]

General options:
  -c, --command=COMMAND    run only single command (SQL or internal) and exit
  -d, --dbname=DBNAME      database name to connect to (default: "postgres")
  -f, --file=FILENAME      execute commands from file, then exit
  -l, --list               list available databases, then exit
  -v, --set=, --variable=NAME=VALUE
                           set psql variable NAME to VALUE
                           (e.g., -v ON_ERROR_STOP=1)
  -V, --version            output version information, then exit
  -X, --no-psqlrc          do not read startup file (~/.psqlrc)
  -1 ("one"), --single-transaction
                           execute as a single transaction (if non-interactive)
  -?, --help[=options]     show this help, then exit
      --help=commands      list backslash commands, then exit
      --help=variables     list special variables, then exit

Input and output options:
  -a, --echo-all           echo all input from script
  -b, --echo-errors        echo failed commands
  -e, --echo-queries       echo commands sent to server
  -E, --echo-hidden        display queries that internal commands generate
  -L, --log-file=FILENAME  send session log to file
  -n, --no-readline        disable enhanced command line editing (readline)
  -o, --output=FILENAME    send query results to file (or |pipe)
  -q, --quiet              run quietly (no messages, only query output)
  -s, --single-step        single-step mode (confirm each query)
  -S, --single-line        single-line mode (end of line terminates SQL command)

Output format options:
  -A, --no-align           unaligned table output mode
  -F, --field-separator=STRING
                           field separator for unaligned output (default: "|")
  -H, --html               HTML table output mode
  -P, --pset=VAR[=ARG]     set printing option VAR to ARG (see \pset command)
  -R, --record-separator=STRING
                           record separator for unaligned output (default: newline)
  -t, --tuples-only        print rows only
  -T, --table-attr=TEXT    set HTML table tag attributes (e.g., width, border)
  -x, --expanded           turn on expanded table output
  -z, --field-separator-zero
                           set field separator for unaligned output to zero byte
  -0, --record-separator-zero
                           set record separator for unaligned output to zero byte

Connection options:
  -h, --host=HOSTNAME      database server host or socket directory (default: "local socket")
  -p, --port=PORT          database server port (default: "5432")
  -U, --username=USERNAME  database user name (default: "postgres")
  -w, --no-password        never prompt for password
  -W, --password           force password prompt (should happen automatically)

For more information, type "\?" (for internal commands) or "\help" (for SQL
commands) from within psql, or consult the psql section in the PostgreSQL
documentation.

```

shell编写的informix数据库脚本

```shell
set `dbaccess $lv_dbname<<! 2>> ./test.log
     SELECT count(*) FROM t_test WHERE ...;
!`
if [ $2 != 0 ]
then
  v_times=$2+1
else
  v_times=1
fi
```

shell编写的postgres数据库脚本

```shell
result=`psql -d postgres -w -t << EOF 2>> ./test.log
    SELECT count(*) FROM t_test WHERE ...;
EOF`
if [ $result != 0 ]
then
  v_times=$result+1
else
  v_times=1
fi
```

informix的脚本中使用到了`$2`，意识是shell脚本后面传来的参数

linux中shell变量$#,$@,$0,$1,$2的含义解释

```markdown
linux中shell变量$#,$@,$0,$1,$2的含义解释:
变量说明:
$$
Shell本身的PID（ProcessID）
$!
Shell最后运行的后台Process的PID
$?
最后运行的命令的结束代码（返回值）
$-
使用Set命令设定的Flag一览
$*
所有参数列表。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
$@
所有参数列表。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
$#
添加到Shell的参数个数
$0
Shell本身的文件名
$1～$n
添加到Shell的各参数值。$1是第1参数、$2是第2参数…。

示例：
1 #!/bin/bash
 2 #
 3 printf "The complete list is %s\n" "$$"
 4 printf "The complete list is %s\n" "$!"
 5 printf "The complete list is %s\n" "$?"
 6 printf "The complete list is %s\n" "$*"
 7 printf "The complete list is %s\n" "$@"
 8 printf "The complete list is %s\n" "$#"
 9 printf "The complete list is %s\n" "$0"
10 printf "The complete list is %s\n" "$1"
11 printf "The complete list is %s\n" "$2

结果：
[Aric@localhost ~]$ bash params.sh 123456 QQ
The complete list is 24249
The complete list is
The complete list is 0
The complete list is 123456 QQ
The complete list is 123456
The complete list is QQ
The complete list is 2
The complete list is params.sh
The complete list is 123456
The complete list is QQ
```