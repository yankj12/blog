# continuous-integration

## 概述

### 个人备注
持续集成更多的是为了解决功能“集成”的问题，即开发人员开发了单独的功能，并且单独功能单元测试通过，那么能否保证集成到一起后功能还正常呢？持续集成就是为了解决这个问题。


## 相似概念
最近看了一篇文章 The Product Managers' Guide to Continuous Delivery and DevOps 文中对「持续集成（Continuous Integration）」、「持续交付（Continuous Delivery）」和「持续部署（Continuous Deployment）」这三个概念有很详细的解释。这里借用文中的插图，说一下我对这三个概念的理解。

参考 [如何理解持续集成、持续交付、持续部署 yumminhuang的回答](https://www.zhihu.com/question/23444990/answer/89426003)


### 持续集成

![持续集成](./continuous-integration_files/continuous_integration_hd.png)

持续集成强调开发人员提交了新代码之后，立刻进行构建、（单元）测试。根据测试结果，我们可以确定新代码和原有代码能否正确地集成在一起。

### 持续交付

![持续交付](./continuous-integration_files/continuous_delivery_hd.png)

持续交付在持续集成的基础上，将集成后的代码部署到更贴近真实运行环境的「类生产环境」（production-like environments）中。比如，我们完成单元测试后，可以把代码部署到连接数据库的 Staging 环境中更多的测试。如果代码没有问题，可以继续手动部署到生产环境中。


### 持续部署

![持续部署](./continuous-integration_files/continuous_deployment_hd.png)

持续部署则是在持续交付的基础上，把部署到生产环境的过程自动化。


## 参考资料
[知乎话题-持续集成](https://www.zhihu.com/search?type=content&q=%E6%8C%81%E7%BB%AD%E9%9B%86%E6%88%90)

[知乎文章-什么是持续集成（CI）/持续部署（CD）？](https://zhuanlan.zhihu.com/p/42286143)

[jenkins持续集成原理](https://www.cnblogs.com/liyuanhong/p/6548925.html)

[知乎话题-持续集成CI](https://www.zhihu.com/topic/19603714/hot)

[知乎专栏-Jenkins](https://zhuanlan.zhihu.com/c_193701892)

[Gitlab CI 与 DevOps](https://zhuanlan.zhihu.com/p/35749347)

[如何理解持续集成、持续交付、持续部署？](https://www.zhihu.com/question/23444990/answer/89426003)

[The Product Managers’ Guide to Continuous Delivery and DevOps](https://www.mindtheproduct.com/2016/02/what-the-hell-are-ci-cd-and-devops-a-cheatsheet-for-the-rest-of-us/)

[Github 欢迎所有的持续集成工具](https://zhuanlan.zhihu.com/p/30889388)

[「CI」为什么要持续集成](https://www.jianshu.com/p/1cd01bcc77f2)

[什么是持续集成？该怎么做？](http://network.51cto.com/art/201801/563064.htm)

## 相关工具
[8个流行的持续集成工具](https://www.testwo.com/article/1170)

[代码扫描 SonarQube](https://www.sonarqube.org/)

[Gerrit Code Review](https://www.gerritcodereview.com/)