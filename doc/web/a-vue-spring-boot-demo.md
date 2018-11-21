# A VUE and SpringBoot Demo

## SpringBoot结合Mybatis

### mybatis-spring-boot-autoconfigure

[mybatis-spring-boot-autoconfigur](http://www.mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/)

## RESTful API

[RESTful API 最佳实践](http://www.ruanyifeng.com/blog/2018/10/restful-api-best-practices.html)

### RESTful接口分页查询

如何使分页查询接口符合RESTful风格，并且分页查询接口如何与精确查询接口区分开

我能想到这些（都是get）：

```API
请求列表
/users?from=100&to=200

请求单条数据
/users/id
```

GitHub API（规范参考）：https://api.github.com

```API
{
  "current_user_url": "https://api.github.com/user",
  "current_user_authorizations_html_url": "https://github.com/settings/connections/applications{/client_id}",
  "authorizations_url": "https://api.github.com/authorizations",
  "code_search_url": "https://api.github.com/search/code?q={query}{&page,per_page,sort,order}",
  "emails_url": "https://api.github.com/user/emails",
  "emojis_url": "https://api.github.com/emojis",
  "events_url": "https://api.github.com/events",
  "feeds_url": "https://api.github.com/feeds",
  "following_url": "https://api.github.com/user/following{/target}",
  "gists_url": "https://api.github.com/gists{/gist_id}",
  "hub_url": "https://api.github.com/hub",
  "issue_search_url": "https://api.github.com/search/issues?q={query}{&page,per_page,sort,order}",
  "issues_url": "https://api.github.com/issues",
  "keys_url": "https://api.github.com/user/keys",
  "notifications_url": "https://api.github.com/notifications",
  "organization_repositories_url": "https://api.github.com/orgs/{org}/repos{?type,page,per_page,sort}",
  "organization_url": "https://api.github.com/orgs/{org}",
  "public_gists_url": "https://api.github.com/gists/public",
  "rate_limit_url": "https://api.github.com/rate_limit",
  "repository_url": "https://api.github.com/repos/{owner}/{repo}",
  "repository_search_url": "https://api.github.com/search/repositories?q={query}{&page,per_page,sort,order}",
  "current_user_repositories_url": "https://api.github.com/user/repos{?type,page,per_page,sort}",
  "starred_url": "https://api.github.com/user/starred{/owner}{/repo}",
  "starred_gists_url": "https://api.github.com/gists/starred",
  "team_url": "https://api.github.com/teams",
  "user_url": "https://api.github.com/users/{user}",
  "user_organizations_url": "https://api.github.com/user/orgs",
  "user_repositories_url": "https://api.github.com/users/{user}/repos{?type,page,per_page,sort}",
  "user_search_url": "https://api.github.com/search/users?q={query}{&page,per_page,sort,order}"
}
```

参考

- [在设计有关分页的Restful API时，有啥好的实践吗？](https://segmentfault.com/q/1010000000603084/a-1020000000605223)
- [RESTful API的十个最佳实践](http://www.cnblogs.com/xiaoyaojian/p/4612503.html)
- [RESTful API URI 设计的一些总结](https://www.cnblogs.com/xishuai/p/restful-webapi-uri-design.html)

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