# Spring Boot

对SpringBoot的使用进行一些笔记

## 目录

## 配置

### server

#### 配置端口

```application.properties
# 配置端口
server.port=8020
```

#### 配置项目根路径

```application.properties
# 配置项目根路径
server.servlet.context-path=/xxx
```

##### 配置项目根路径的作用

定义：`server.servlet.context-path= # Context path of the application`. 应用的上下文路径，也可以称为项目路径，是构成url地址的一部分。

在每个module的application.properties文件都可以配置server.context-path这个属性。开始使用spring boot的时候没有注意这个属性，其实默认可以不配置，直接在controller层通过@RequestMapping来设定url的地址路径。

```Java
@RestController
@RequestMapping("/mqcp")
public class MQCPContorller {

    @Autowired MQCPServiceImpl mqcpService;
    @RequestMapping("/convert")
    public String convert(){

        mqcpService.convert();
        return "ok";
    }
}
```

如果`server.servlet.context-path`没有配，请求的url地址就是 `localhost:port/mqcp/convert`

如果`server.servlet.context-path = "/market/task"`, 请求的url地址就是 `localhost:port/market/task/mqcp/convert`

在 task这个模块下的所有web层的url地址都需要添加`server.servlet.context-path`。

参考资料[Spring Boot 应用中server.context-path的作用](https://blog.csdn.net/onedaycbfly/article/details/80108129)

##### spring-boot配置文件中server.context-path=/XXXXXXX不起作用：

spring-boot配置文件中`server.context-path=/XXXXXXX`不起作用：原因是更新后写法变成了`server.servlet.context-path=/XXXXXX`，这样写即可

### 多环境配置文件激活属性

在SpringBoot中切换多个环境配置

```application.properties
＃ 多环境配置文件激活属性
spring.profiles.active=dev      ＃加载application-dev.properties配置文件内容 
application-dev.properties：    ＃开发环境
application-test.properties：   ＃测试环境
application-prod.properties：   ＃生产环境
```

#### 测试不同配置的加载

以不同环境配置不同的服务端口为例，进行样例实验。

针对各环境新建不同的配置文件application-dev.properties、application-test.properties、application-prod.properties

在这三个文件均都设置不同的server.port属性，如：dev环境设置为8001，test环境设置为8002，prod环境设置为8003

application.properties中设置spring.profiles.active=dev，就是说默认以dev环境设置

```Java
执行 java -jar xxx.jar，可以观察到服务端口被设置为8001，也就是默认的开发环境（dev）
执行 java -jar xxx.jar --spring.profiles.active=test，可以观察到服务端口被设置为8002，也就是测试环境的配置（test）
执行 java -jar xxx.jar --spring.profiles.active=prod，可以观察到服务端口被设置为8003，也就是生产环境的配置（prod）
```

按照上面的实验，可以如下总结多环境的配置思路：

application.properties中配置通用内容，并设置spring.profiles.active=dev，以开发环境为默认配置

application-{profile}.properties中配置各个环境不同的内容，通过命令行方式去激活不同环境的配置

### application.properties各种配置解释

参考[application.properties各种配置解释](https://blog.csdn.net/tang430524/article/details/78911556)

## 注解

### 跨域访问@CrossOrigin

1、对于前后端分离的项目来说，如果前端项目与后端项目部署在两个不同的域下，那么势必会引起跨域问题的出现。

针对跨域问题，我们可能第一个想到的解决方案就是jsonp，并且以前处理跨域问题我基本也是这么处理。

但是jsonp方式也同样有不足，不管是对于前端还是后端来说，写法与我们平常的ajax写法不同，同样后端也需要作出相应的更改。并且，jsonp方式只能通过get请求方式来传递参数，当然也还有其它的不足之处，针对于此，我并没有急着使用jsonp的方式来解决跨域问题，去网上找寻其它方式，也就是本文主要所要讲的，在springboot中通过cors协议解决跨域问题。

2、Cors协议

H5中的新特性：Cross-Origin Resource Sharing（跨域资源共享）。通过它，我们的开发者（主要指后端开发者）可以决定资源是否能被跨域访问。

cors是一个w3c标准，它允许浏览器（目前ie8以下还不能被支持）像我们不同源的服务器发出xmlHttpRequest请求，我们可以继续使用ajax进行请求访问。

具体关于cors协议的文章 ，可以参考 [跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html) 这篇文章，讲的相当不错。

#### 参考

- [Springboot后台设置允许跨域的方法](https://blog.csdn.net/hlp4207/article/details/80870716)
- [springboot中通过cors协议解决跨域问题](https://www.cnblogs.com/520playboy/p/7306008.html)

#### 在允许跨域请求的controller中使用@CrossOrigin 注解

```Java
import org.springframework.web.bind.annotation.CrossOrigin;
@CrossOrigin
@RestController
public class HelloWorldController {

}
```

## 参考资料

- [SpringBoot官方文档](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/)
- [application.properties各种配置解释](https://blog.csdn.net/tang430524/article/details/78911556)