## SpringBoot比较SPringCloud

Spring boot 是 Spring 的一套快速配置脚手架，可以基于spring boot 快速开发单个微服务。

Spring Cloud是一个基于Spring Boot实现的云应用开发工具；

Spring boot专注于快速、方便集成的单个个体，Spring Cloud是关注全局的服务治理框架；



spring boot使用了默认大于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置；

Spring Cloud很大的一部分是基于Spring boot来实现。

Spring boot可以离开Spring Cloud独立使用开发项目，但是Spring Cloud离不开Spring boot，属于依赖的关系。

spring -> spring boot > spring cloud 这样的关系。



## SpringCloud比较Dubbo

从项目的背景来看，Dubbo 国内用的公司挺多，国内影响力大，Spring Cloud 自然在国外影响力较大，所以这个来看不分伯仲了，毕竟都有大公司在使用。

Dubbo曾经长期停止维护，2017年7月31日团队又宣布重点维护，使用起来还是有一定的门槛。


从框架的完整度来看，Dubbo只是实现了服务治理（注册 发现等），而Spring Cloud下面有很多个子项目覆盖了微服务架构下的方方面面，服务治理只是其中的一个方面，一定程度来说，Dubbo只是Spring Cloud Netflix中的一个子集。


虽然Dubbo 支持短连接大数据量的服务提供模式，但绝大多数情况下都是使用长连接小数据量的模式提供服务使用的。所以，对于类似于电商等同步调用场景多并且能支撑搭建Dubbo 这套比较复杂环境的成本的产品而言，Dubbo 确实是一个可以考虑的选择。但如果产品业务中由于后台业务逻辑复杂、时间长而导致异步逻辑比较多的话，可能Dubbo 并不合适。同时，对于人手不足的初创产品而言，这么重的架构维护起来也不是很方便。





