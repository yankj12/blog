# Spring Boot

对SpringBoot的使用进行一些笔记

## 目录

- [配置](#配置)
    - [配置加载顺序](#配置加载顺序)
    - [配置端口](#配置端口)
    - [配置项目根路径](#配置项目根路径)
    - [多环境配置文件激活属性](#多环境配置文件激活属性)
    - [application.properties各种配置解释](#application.properties各种配置解释)
- [注解](#注解)
    - [跨域访问@CrossOrigin](#跨域访问@CrossOrigin)
    - [@RequestParam注解](#@RequestParam注解)
    - [@ComponentScan注解](#@ComponentScan注解)
- [监控与管理](#监控与管理)
- [常见问题](#常见问题)
- [参考资料](#参考资料)

## 配置

### 配置加载顺序

我们可以通过spring.profiles.active或者是通过maven来实现多环境的支持，但是当团队越来越大，往往不需要让开发人员知道测试环境或是生产环境的细节，而是由各个环境的负责人来集中维护这些信息。那么如果还是以这样的方式存储配置内容，就会存在很多问题

为了能够更加合理地重写个属性的值，SpringBoot使用了下面这种较为特别的属性加载顺序：

1. 命令行中传入的参数。
2. SPRING_APPLICATION_JSON中的属性。SPRING_APPLICATION_JSON是以JSON格式配置在系统环境变量中的内容。
3. java:comp/env中的JNDI属性。
4. java的系统属性，可以通过System.getProperties()获得的内容。
5. 操作系统的环境变量。
6. 通过random.*配置的随机属性。
7. 位于当前应用jar包之外，针对不同{profile}环境的配置文件内容，例如application-{profile}.properties或是YAML定义的配置文件。
8. 位于当前应用jar包之内，针对不同{profile}环境的配置文件内容，例如application-{profile}.properties或是YAML定义的配置文件。
9. 位于当前应用jar包之外的application.properties和YAML配置内容。
10. 位于当前应用jar包之内的application.properties和YAML配置内容。
11. 在@Configuration注解修改的类中，通过@PropertySource注解定义的属性。
12. 应用默认属性，使用SpringApplication.setDefailtProperties定义的内容。

优先级按照上面的顺序由高到低，数字越小优先级越高。

可以看到，其中第7项和第9项都是从应用jar包之外读取配置文件，所以，实现外部配置化的原理就是从此切入，为其制定外部配置文件的加载顺序来取代jar包之内的配置内容。

### 配置端口

```application.properties
# 配置端口
server.port=8020
```

### 配置项目根路径

```application.properties
# 配置项目根路径
server.servlet.context-path=/xxx
```

#### 配置项目根路径的作用

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

#### spring-boot配置文件中server.context-path=/XXXXXXX不起作用：

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

#### 跨域访问资料参考

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

## @RequestParam注解

@RequestParam

```Java
@RequestMapping("/list")
public String test(int userId) {

    return "list";
}


@RequestMapping("/list")
public String test(@RequestParam int userId) {

    return "list";
}
```

第一种写法参数为非必传，第二种写法参数为必传。参数名为userId。

第二种写法可以通过@RequestParam(required = false)设置为非必传。因为required值默认是true，所以默认必传。

第二种写法可以通过@RequestParam("userId")或者@RequestParam(value = "userId")指定参数名。

第二种写法可以通过@RequestParam(defaultValue = "0")指定参数默认值

用法如下：

```Java
@RequestMapping("/list")
public String test(@RequestParam(value = "userId", defaultValue = "0", required = false) int userId) {

    return "list";
}
```

参考自[@RequestParam加与不加的区别](https://blog.csdn.net/u013805360/article/details/79527175)

### @ComponentScan注解

>ComponentScan做的事情就是告诉Spring从哪里找到bean

Spring Boot项目

总结：

- 如果你的其他包都在使用了@SpringBootApplication注解的main app所在的包及其下级包，则你什么都不用做，SpringBoot会自动帮你把其他包都扫描了
- 如果你有一些bean所在的包，不在main app的包及其下级包，那么你需要手动加上@ComponentScan注解并指定那个bean所在的包

类`SpringbootIn10StepsApplication`在`com.in28minutes.springboot.basics.springbootin10steps`包下，这个类使用了`@SpringBootApplication`注解，该注解定义了Spring将自动扫描包`com.in28minutes.springboot.basics.springbootin10steps`及其子包下的bean

如果你项目中所有的类都定义在上面的包及其子包下，那你不需要做任何事。

但假如你一个类定义在包`com.in28minutes.springboot.somethingelse`下，则你需要将这个新包也纳入扫描的范围,有两个方案可以达到这个目的。

方案1

定义`@ComponentScan(“com.in28minutes.springboot”)`

这么做扫描的范围扩大到整个父包com.in28minutes.springboot

```Java
@ComponentScan(“com.in28minutes.springboot”)
@SpringBootApplication
public class SpringbootIn10StepsApplication {
```

方案2

定义分别扫描两个包
`@ComponentScan({“com.in28minutes.springboot.basics.springbootin10steps”,”com.in28minutes.springboot.somethingelse”})`

```Java
@ComponentScan({"com.in28minutes.springboot.basics.springbootin10steps","com.in28minutes.springboot.somethingelse"})
@SpringBootApplication
public class SpringbootIn10StepsApplication {
```

参考 [Spring Boot学习笔记1：Spring, Spring Boot中的@Component 和@ComponentScan注解用法介绍](https://blog.csdn.net/Lamb_IT/article/details/80918704)

## 监控与管理

SpringBoot在Starter PMs中提供了一个特殊依赖模块spring-boot-starter-actuator，能够自动为SpringBoot构建的应用提供一系列用于监控的端点(end points)。同时SpringCloud在实现各个微服务组件的时候，进一步为该模块做了扩展。

spring-boot-starter-actuator提供的原生端点，可以分为三大类：

- 应用配置类
- 度量指标类
- 操作控制类

## 常见问题

### SpringBoot启动报错Caused by: java.lang.NoSuchMethodError: org.springframework.util.Assert.notNull(Ljava/lang/Object;Ljava/util/function/Supplier;)V

报错信息如下：

```Java
十一月 26, 2018 3:05:44 下午 org.springframework.context.support.GenericApplicationContext refresh
警告: Exception encountered during context initialization - cancelling refresh attempt: org.springframework.beans.factory.BeanDefinitionStoreException: Failed to process import candidates for configuration class [com.sinosoft.interview.registry.InterviewRegistryApplication]; nested exception is java.lang.NoSuchMethodError: org.springframework.util.Assert.notNull(Ljava/lang/Object;Ljava/util/function/Supplier;)V
十一月 26, 2018 3:05:44 下午 org.springframework.test.context.TestContextManager prepareTestInstance
严重: Caught exception while allowing TestExecutionListener [org.springframework.test.context.support.DependencyInjectionTestExecutionListener@6ddf90b0] to prepare test instance [com.sinosoft.interview.registry.MongoTemplateTest@72f926e6]
java.lang.IllegalStateException: Failed to load ApplicationContext
	at org.springframework.test.context.cache.DefaultCacheAwareContextLoaderDelegate.loadContext(DefaultCacheAwareContextLoaderDelegate.java:124)
	at org.springframework.test.context.support.DefaultTestContext.getApplicationContext(DefaultTestContext.java:83)
Caused by: org.springframework.beans.factory.BeanDefinitionStoreException: Failed to process import candidates for configuration class [com.sinosoft.interview.registry.InterviewRegistryApplication]; nested exception is java.lang.NoSuchMethodError: org.springframework.util.Assert.notNull(Ljava/lang/Object;Ljava/util/function/Supplier;)V
	at org.springframework.context.annotation.ConfigurationClassParser.processDeferredImportSelectors(ConfigurationClassParser.java:561)
	... 25 more
Caused by: java.lang.NoSuchMethodError: org.springframework.util.Assert.notNull(Ljava/lang/Object;Ljava/util/function/Supplier;)V
	at org.springframework.boot.autoconfigure.AutoConfigurationImportSelector.getAttributes(AutoConfigurationImportSelector.java:132)
```

查了下资料，发现大部分是说jar包冲突导致，想了想就去[Spring官网](https://start.spring.io/)使用start生成了一个pom

添加组件`web`,`mongo`等

重新刷新项目后，正常了

### Path represents URL or has "url:" prefix: [classpath:/templates/

controller中将url映射到了视图，并且`视图名.html`在templates文件夹下是存在的，但是页面报404错误。

最后经过尝试，在pom文件中添加`thymeleaf`依赖后，可以显示页面

```pom
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

## 参考资料

- [SpringBoot官方文档](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/)
- [application.properties各种配置解释](https://blog.csdn.net/tang430524/article/details/78911556)