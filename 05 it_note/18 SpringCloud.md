## Eureka简介
Eureka是Netflix开发的服务发现框架，本身是一个基于REST的服务，主要用于定位运行在AWS域中的中间层服务，以达到负载均衡和中间层服务故障转移的目的。Spring Cloud将它集成在其子项目spring-cloud-netflix中，以实现Spring Cloud的服务发现功能。
 
Eureka 项目相当活跃，代码更新相当频繁，目前最新的版本是1.5.5。Eureka 2.0的版本也正在紧锣密鼓地开发中，2.0将会带来更好的扩展性，并且使用细粒度的订阅模型取代了基于拉取的模型，但是由于还没有Release，故而不作讲解。

![eureka原理](https://github.com/liuyanliang2015/BertNote/blob/master/pics/eureka.png)


上图是来自Eureka官方的架构图，大致描述了Eureka集群的工作过程。由于图比较复杂，可能比较难看懂，这边用通俗易懂的语言翻译一下：
 
- Application Service 就相当于本书中的服务提供者（用户微服务），Application Client就相当于本书中的服务消费者（电影微服务）；
- Make Remote Call，可以简单理解为调用RESTful的接口；
- us-east-1c、us-east-1d等是zone，它们都属于us-east-1这个region；
 
由图可知，Eureka包含两个组件：Eureka Server 和 Eureka Client。
 
Eureka Server提供服务注册服务，各个节点启动后，会在Eureka Server中进行注册，这样Eureka Server中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观的看到。
 
Eureka Client是一个Java客户端，用于简化与Eureka Server的交互，客户端同时也具备一个内置的、使用轮询（round-robin）负载算法的负载均衡器。
 
在应用启动后，将会向Eureka Server发送心跳（默认周期为30秒）。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，Eureka Server将会从服务注册表中把这个服务节点移除（默认90秒）。
 
Eureka Server之间将会通过复制的方式完成数据的同步。（详见Eureka高可用章节）
 
Eureka还提供了客户端缓存的机制，即使所有的Eureka Server都挂掉，客户端依然可以利用缓存中的信息消费其他服务的API。
 
综上，Eureka通过心跳检测、健康检查、客户端缓存等机制，确保了系统的高可用性、灵活性和可伸缩性。


## Ribbon简介

### 负载均衡有哪些方式

A：服务器端负载均衡

B：客户端负载均衡


![负载均衡](https://github.com/liuyanliang2015/BertNote/blob/master/pics/load.png)


Ribbon是Netflix发布的云中间层服务开源项目，其主要功能是提供客户端侧负载均衡算法。Ribbon客户端组件提供一系列完善的配置项如连接超时，重试等。简单的说，Ribbon是一个客户端负载均衡器，我们可以在配置文件中列出Load Balancer后面所有的机器，Ribbon会自动的帮助你基于某种规则（如简单轮询，随机连接等）去连接这些机器，我们也很容易使用Ribbon实现自定义的负载均衡算法。

![ribbon](https://github.com/liuyanliang2015/BertNote/blob/master/pics/ribbon.png)


Ribbon工作时分为两步：第一步先选择 Eureka Server, 它优先选择在同一个Zone且负载较少的Server；第二步再根据用户指定的策略，在从Server取到的服务注册列表中选择一个地址。其中Ribbon提供了多种策略，例如轮询round robin、随机Random、根据响应时间加权等。
