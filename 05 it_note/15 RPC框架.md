## 1：什么是RPC

### 官方定义
RPC（Remote Procedure Call Protocol）——远程过程调用协议，它是一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议。RPC协议假定某些传输协议的存在，如TCP或UDP，为通信程序之间携带信息数据。在OSI网络通信模型中，RPC跨越了传输层和应用层。RPC使得开发包括网络分布式多程序在内的应用程序更加容易。

### 深入解析

RPC简单的来说就是像调用本地服务一样调用远程服务。

RPC 的主要功能目标是让构建分布式计算（应用）更容易，在提供强大的远程调用能力时不损失本地调用的语义简洁性。

Java中常用的RPC框架有RMI、Hessian、Dubbo。

RPC要做到用户无感知的调用远程服务必定要经过网络传输，而netty正好是是Java编写的快速开发高性能高可靠性的网络编程框架。目前使用netty作为传输层的RPC框架很多，国内知名的有dubbo。

下面以dubbo为例，讲解一下：

dubbo使用Hessian的序列化协议，传输则是TCP协议，使用了高性能的NIO框架Netty。

![dubbo原理](https://github.com/liuyanliang2015/BertNote/blob/master/pics/dubbo.png)








