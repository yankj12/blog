# 微服务

## 微服务与单体应用

单体应用由于面对的业务需求更加宽泛，不断扩大的需求会使得单体应用变得越来越臃肿

由于单体系统部署在一个进程内，往往我们修改了一个很小的功能，为了部署上线会影响其他功能的运行。并且，单体应用中的这些功能模块的使用场景、并发量、消耗的资源类型都各有不同，对于资源的利用又互相影响，这样使得我们对于各个业务模块的系统容量很难给出较为准确的评估。所以单体系统在初期虽然可以非常方便地进行开发和使用，但是随着系统的发展，维护成本会变得越来越大，而且难以控制。

### 微服务拆分后的挑战

- 运维的新挑战

    对服务的编排和组织。要求运维人员有一定的编程能力，将这些进程编排和组织起来，让其自动化的运行。

- 接口的一致性

    由单体应用的代码依赖转变为接口依赖。我们需要更加完善的接口和版本管理，或者尽量遵循开闭原则。

- 分布式的复杂性

    拆分后的微服务都是通过通信进行协作，所以分布式的问题都需要进行考虑，比如网络延迟、分布式事务、异步消息等。

Martin Fowler在Microservices一文中，提炼出了微服务架构的九大特性，用于指导大家设计架构。

1. 服务组件化

2. 按业务组织团队

3. 做“产品”的态度

    每个团队都应该以做产品的方式，对其整个生命周期负责。而不是以做项目的模式，以完成开发和交付并将产品交接给维护者为最终目标。

4. 智能端点与哑管道

    单体应用中组件之间的调用通过函数调用的方式进行，微服务后，组件之间通过http进行。如果将原有的通信方式直接调整为rpc调用，会导致服务之间繁琐的通信，使系统表现地更为糟糕。所以我们需要更粗粒度的通信协议。

    在微服务架构中，通常会使用如下两种通信协议：

    - 使用HTTP的RESTful API或轻量级的消息发送协议，实现消息传递与服务的调用。
    - 通过在轻量级消息总线上传递消息，类似RabbitMQ等一些中间件。

5. 去中心化治理

    微服务架构中通过轻量化的协议接口，将各个组件可以根据其具体业务选择不同的技术平台。

6. 去中心化管理数据

    一方面我们将原有数据库中的数据拆分到多个相同类型的数据库中（比如mysql拆分到mysql中），另一方面，我们可以将不同业务数据拆分到不同类型的数据库中（比如mysql拆分到redis或者mongodb中）

    数据分散到不同数据库后，数据的一致性成为一个问题。分布式事务本身实现难度就比较大，在微服务架构下，我们更强调在服务之间“无事务”的调用。对于数据的一致性，只要求在数据在最终的处理状态是一致的即可。若在中间出现错误，通过补偿机制处理。

7. 基础设置自动化

8. 容错设计

9. 演进式设计