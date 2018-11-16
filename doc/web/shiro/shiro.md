# shiro

## 集成shiro核心分析

集成Shiro的话，我们需要知道Shiro框架大概的一些管理对象。

第一：ShiroFilterFactory，Shiro过滤器工程类，具体的实现类是：
ShiroFilterFactoryBean，此实现类是依赖于SecurityManager安全管理器。

第二：SecurityManager,Shiro的安全管理，主要是身份认证的管理，缓存管理，cookie管理，
所以在实际开发中我们主要是和SecurityManager进行打交道的，ShiroFilterFactory主要配置好
了Filter就可以了。当然SecurityManager并进行身份认证缓存的实现，我们需要进行对应的编
码然后进行注入到安全管理器中。

第三：Realm,用于身份信息权限信息的验证。

第四：其它的就是缓存管理，记住登录之类的，这些大部分都是需要自己进行简单的实
现，然后注入到SecurityManager让Shiro的安全管理器进行管理就好了。

## 编写基本SpringBoot应用

编写基本SpringBoot应用，提供index和login页面

## 集成shiro

集成shiro大概分这么一个步骤：

(a) pom.xml中添加Shiro依赖；

(b) 注入Shiro Factory和SecurityManager。

(c) 身份认证

(d) 权限控制


(c) 身份认证

在认证、授权内部实现机制中都有提到，最终处理都将交给Real进行处理。因为在Shiro
中，最终是通过Realm来获取应用程序中的用户、角色及权限信息的。通常情况下，在
Realm中会直接从我们的数据源中获取Shiro需要的验证信息。可以说，Realm是专用于安全
框架的DAO.

