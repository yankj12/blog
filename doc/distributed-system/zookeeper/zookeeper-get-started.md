# Zookeeper Get Started

文章参考自[ZooKeeper Getting Started Guide](http://zookeeper.apache.org/doc/r3.4.14/zookeeperStarted.html)

## zk单节点

### 启动单节点

解压

修改配置文件`conf/zoo.cfg`

```zoo.cfg
tickTime=2000
dataDir=/var/lib/zookeeper
clientPort=2181
```

启动

linux下

```shell
bin/zkServer.sh start
```

windows

```CMD
bin>zkServer.cmd
```

### 客户端连接及简单命令

```Shell
bin/zkCli.sh -server 127.0.0.1:2181
```

连接成功，控制台会出现下面的信息

```console
Connecting to localhost:2181
log4j:WARN No appenders could be found for logger (org.apache.zookeeper.ZooKeeper).
log4j:WARN Please initialize the log4j system properly.
Welcome to ZooKeeper!
JLine support is enabled
[zkshell: 0]
```

常用命令

`ls`查看列表

```Shell
[zkshell: 8] ls /
[zookeeper]
```

`create /zk_test my_data`创建一个znode，并将字符串"my_data"和这个节点关联起来

```Shell
[zkshell: 9] create /zk_test my_data
Created /zk_test
```

`ls /`命令查看文件夹中内容

```Shell
[zkshell: 11] ls /
[zookeeper, zk_test]
```

注意到zk_test文件夹已经被创建了

通过`get`命令验证znode节点关联的数据

```Shell
[zkshell: 12] get /zk_test
my_data
cZxid = 5
ctime = Fri Jun 05 13:57:06 PDT 2009
mZxid = 5
mtime = Fri Jun 05 13:57:06 PDT 2009
pZxid = 5
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0
dataLength = 7
numChildren = 0
```

通过`set`命令修改znode关联的数据

```Shell
[zkshell: 14] set /zk_test junk
cZxid = 5
ctime = Fri Jun 05 13:57:06 PDT 2009
mZxid = 6
mtime = Fri Jun 05 14:01:52 PDT 2009
pZxid = 5
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0
dataLength = 4
numChildren = 0
[zkshell: 15] get /zk_test
junk
cZxid = 5
ctime = Fri Jun 05 13:57:06 PDT 2009
mZxid = 6
mtime = Fri Jun 05 14:01:52 PDT 2009
pZxid = 5
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0
dataLength = 4
numChildren = 0
```

`delete`删除node

```Shell
[zkshell: 16] delete /zk_test
[zkshell: 17] ls /
[zookeeper]
[zkshell: 18]
```

学习探索更多的内容，参考 [Programmer's Guide](http://zookeeper.apache.org/doc/r3.4.14/zookeeperProgrammers.html).

## 常见错误

### ZooKeeper启动错误 Invalid arguments, exiting abnormally 解决方案 Windows

使用bin中的脚本 zkServer.cmd 启动zookeeper，输入命令

```CMD
D:\WORK\Project-Test\zookeeper-3.4.11\bin>zkServer.cmd start
```

发现无法启动，出现如下错误

```Java
2018-01-29 19:37:33,793 [myid:] - ERROR [main:ZooKeeperServerMain@57] - Invalid arguments, exiting abnormally
java.lang.NumberFormatException: For input string: "D:\WORK\Project-Test\zookeeper-3.4.11\bin\..\conf\zoo.cfg"
        at java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
        at java.lang.Integer.parseInt(Integer.java:580)
        at java.lang.Integer.parseInt(Integer.java:615)
        at org.apache.zookeeper.server.ServerConfig.parse(ServerConfig.java:61)
        at org.apache.zookeeper.server.ZooKeeperServerMain.initializeAndRun(ZooKeeperServerMain.java:86)
        at org.apache.zookeeper.server.ZooKeeperServerMain.main(ZooKeeperServerMain.java:55)
        at org.apache.zookeeper.server.quorum.QuorumPeerMain.initializeAndRun(QuorumPeerMain.java:119)
        at org.apache.zookeeper.server.quorum.QuorumPeerMain.main(QuorumPeerMain.java:81)
```

原因： 用法不对，start是在linux下用.sh脚本时的命令，在win下直接打脚本名就好

```CMD
D:\WORK\Project-Test\zookeeper-3.4.11\bin>zkServer.cmd
```