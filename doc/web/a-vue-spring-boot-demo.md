# A VUE and SpringBoot Demo

## SpringBoot结合Mybatis

### mybatis-spring-boot-autoconfigure

[mybatis-spring-boot-autoconfigur](http://www.mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/)

## RESTful API

[RESTful API 最佳实践](http://www.ruanyifeng.com/blog/2018/10/restful-api-best-practices.html)

## 操作笔记

### 卸载之前安装的版本

之前通过yum安装过node，版本比较旧，准备进行更新

参考的[在centos7安装nodejs并升级nodejs到最新版本](https://segmentfault.com/a/1190000015302680)进行更新

- 先通过yum移除

```Shell
yum remove nodejs npm -y
```

- 手动删除残留

由于没有怎么使用node，所以这一步我省略了

### 在云服务器上安装nodejs

服务器为Centos7，下载二进制文件进行安装

```Shell
下载二进制文件
wget https://nodejs.org/dist/v10.13.0/node-v10.13.0-linux-x64.tar.xz

解压缩
tar xvf node-v10.13.0-linux-x64.tar.xz

移动到/usr/local/下:
mv node-v10.13.0-linux-x64 /usr/local/nodejs

添加命令到环境变量里:
vim /etc/profile

在/etc/profile文件末尾添加：
export PATH=$PATH:/usr/local/nodejs/bin

保存推出
使修改生效，不必重启系统
source /etc/profile

测试：
node -v
npm -v

```

参考[Centos7安装node.js ,采用二进制安装:](https://blog.csdn.net/mrtwenty/article/details/80404833)

### 编码

参考了博客[使用vue+elementUI+springboot创建基础后台增删改查的管理页面--(1)](http://www.cnblogs.com/hetutu-5238/p/9456016.html)

## 常见问题

### Error: EACCES: permission denied, access '/usr/local/lib/node_modules' npm ERR! at Error (native)

使用命令行保持类似以下错误:  Error: EACCES: permission denied, access '/usr/local/lib/node_modules' npm ERR! at Error (native)

原因: 执行命令行命令时没有获得管理员权限

解决办法: 在命令前面加上sudo即可.然后输入电脑的管理员密码操作即可完成.

参考[Error: EACCES: permission denied, access '/usr/local/lib/node_modules' npm ERR! at Error (native)](https://blog.csdn.net/soindy/article/details/51135146)

## 参考资料

- [从零开始搭建VUE项目](http://www.cnblogs.com/cczlovexw/p/7691786.html)
- [使用vue+elementUI+springboot创建基础后台增删改查的管理页面--(1)](http://www.cnblogs.com/hetutu-5238/p/9456016.html)
- [VUE 官方文档](https://cn.vuejs.org/v2/guide/installation.html)