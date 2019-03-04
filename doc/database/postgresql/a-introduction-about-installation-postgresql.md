# 安装PostgreSQL

- [安装PostgreSQL](#%E5%AE%89%E8%A3%85postgresql)
  - [前言](#%E5%89%8D%E8%A8%80)
  - [Ubuntu上二进制包安装PG](#ubuntu%E4%B8%8A%E4%BA%8C%E8%BF%9B%E5%88%B6%E5%8C%85%E5%AE%89%E8%A3%85pg)
  - [Ubuntu上源码安装PG](#ubuntu%E4%B8%8A%E6%BA%90%E7%A0%81%E5%AE%89%E8%A3%85pg)
    - [环境准备](#%E7%8E%AF%E5%A2%83%E5%87%86%E5%A4%87)
    - [下载源码](#%E4%B8%8B%E8%BD%BD%E6%BA%90%E7%A0%81)
    - [安装PG](#%E5%AE%89%E8%A3%85pg)
      - [configure](#configure)
      - [build](#build)
      - [install](#install)
    - [安装后配置](#%E5%AE%89%E8%A3%85%E5%90%8E%E9%85%8D%E7%BD%AE)
      - [Shared Libraries](#shared-libraries)
      - [Environment Variables](#environment-variables)
    - [启动PG数据库](#%E5%90%AF%E5%8A%A8pg%E6%95%B0%E6%8D%AE%E5%BA%93)
  - [Windows上安装PG](#windows%E4%B8%8A%E5%AE%89%E8%A3%85pg)

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

### 启动PG数据库

[Chapter 18. Server Setup and Operation](https://www.postgresql.org/docs/current/runtime.html)

## Windows上安装PG

我在自己电脑上安装的时候，选择语言的时候，选择的简体中文，无法安装成功，选择local，安装成功了。

需要注意的是，默认root用户的用户名不是root，而是postgres

