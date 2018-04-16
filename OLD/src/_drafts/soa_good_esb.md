---
layout: post
title: "精彩的ESB"
description: "如同‘政策是好的，执行出了问题’，ESB的思想是好的，但是ESB的产品出了问题：它们总是希望把各种各样的功能都加到自己的产品之中。"
category: 基础设施
tags: [SOA, ESB]
lastmod: 
---

有很多不同的技术，解决不同层面的问题：

1. RESTful, 客户端--》服务器, 服务器--服务器？
2. SOAP， 
3. Thrift
4. A




# 不要误用ESB

ESB的糟糕之处来自两点：厂商的误导，和用户的误用。厂商的误导在[上一篇](




ESB的糟糕不在于其思想，而在于其设计。还有掺杂的“标准之争”。

需要正确使用，才能发挥ESB的积极作用。

# 消息vsRPC

在众多分布式技术中，消息传递相较文件传递与远程过程调用（RPC）而言，似乎更胜一筹，因为它具有更好的平台无关性，并能够很好地支持并发与异步调用。对于Web Service与RESTful而言，则可以看做是消息传递技术的一种衍生或封装。

基于消息的分布式架构

《面向模式的软件架构（卷四）》一书中，将关于消息传递的模式划归为分布式基础设施的范畴


## 消息通道

消息通道作为在客户端（消费者，Consumer）与服务（生产者，Producer）之间引入的间接层，可以有效地解除二者之间的耦合

虽然消息通道解除了生产者与消费者之间的耦合，使得我们可以任意地对生产者与消费者进行扩展，但它又同时引入了各自对消息通道的依赖，因为它们必须知道通道资源的位置。要解除这种对通道的依赖，可以考虑引入Lookup服务来查找该通道资源。

消息通道通常以队列的形式存在，这种先进先出的数据结构无疑最为适合这种处理消息的场景

## 消息传递的好处

《企业整合模式》(Enterprise Integration Patterns)中指出，消息传递：

- 比文件传输更直接
- 比共享数据库封装性好
- 比RPC可靠

其他好处包括：

- 远程通信，自动处理“串行化”
- 跨平台/语言——通用连接特性是消息总线模式的核心
- 异步通信
- 可变的定时机制
- 节流，避免RPC的单个接收点性能瓶颈
- 可靠的通信
- 无连接运行
- 仲裁
- 线程管理



## Bus

Service Bus是在Message Bus之上封装了语义层


# RPC？

AMQP：

- 模型层：定义				解析 or 代码生成？ IDL
- 会话层：保障传输				路由？
- 传输层：数据封装、传输、解析

OSI?


Thrift:
- 模型层： thriftDL
- TProtocol	: TBinaryProtocol, TCompactProtocol, TJSONProtocol, TSimpleJSONProtocol, etc...
- TTransport: TSocket, TFile, ….



概括地讲，ESB 具有四个主要功能：

   * 消息路由：将传入消息发送到目的地，该目的地通过硬编码方式连接的逻辑确定或基于内容的动态方式确定。路由是启用服务虚拟化的关键功能。在调用方和服务之间建立中间层可以在调用方不知道更改的情况下移动服务的位置。
   * 消息转换：将传入消息从一种格式转换为另一种格式。例如，可以将逗号分隔的消息转换为 SOAP，这样可以将数据传递到 Web 服务。
   * 协议中介：传入消息使用不同的协议从发出位置发送。例如，传入消息可以使用 HTTP，而传出消息可以使用 WebSphere MQ。
   * 事件处理：事件的传入消息一般通过发布和订阅模型分发给许多端点。

在给定的事务中，通常会合并这些主要高级功能。例如，传入消息可能是一个使用 SOAP/HTTP 的 Web 服务调用，而目的地是需要使用 WebSphere MQ 的固定长度消息格式的遗留系统。必须转换消息、协调协议并且必须将消息路由到正确的位置。


* 解耦中介 ：客户对实际服务提供者的身份、物理位置、传输协议和接口定义都是不知道也不关心的，交互集成代码提取到了业务逻辑之外，由ESB平台进行中央的宣告式定义。ESB平台实现协议转换 (WebService，Http，JMS...)，消息转换 (转换、充实、过滤)，消息路由 (同步/异步、发布/订阅、基于内容路由、分支与聚合...)。

# ESB的实现

ESB 是一种体系结构模式，而不是软件产品。不同的软件产品可以构成 ESB。在某些情况下，公司在不同的区域中使用多种产品，利用特定的功能来满足其独特的需求。可以将这些不同的产品联合在一起实现 ESB 模式。



# ESB的核心功能？

从上面可以看到ESB的基本功能仍然是数据传输，消息协议转化，路由三大核心功能。有这三大核心功能也可以看到在进行异构系统的整合时候往往根据需要ESB提供这些功能。没有ESB时候也可以实现SOA，比如借助SCA和BPEL来实现SOA，当时却很难实现消息协议转化和动态路由。

ESB的五个基本功能：　　1)服务的MetaData管理：在总线范畴内对服务的注册命名及寻址 进行管理。 　　2)传输服务：确保通过企业总线互连的业务流程间的消息的正确交付，还包括基于内容的路由功能。 　　3)中介：提供位置透明的路由和定位服务；提供多种消息传递形式；支持广泛使用的传输协议 。 　　4)多服务集成方式： 如JCA，Web服务，Messaging ，Adaptor等. 　　5)服务和事件管理支持： 调用服务的记录、测量和监控数据；提供事件检测、触发和分布功能；


# ESB的扩展功能？

ESB的八个扩展功能：　　1) 面向服务的元数据管理： 他必须了解被他中介的两端,即服务的请求以及请求者对服务的要求，以及服务的提供者和他所提供的服务的描述； 　　2) Mediation ：它必须具有某种机制能够完成中介的作用，如协议转换； 　　3) 通信：服务发布、订阅，响应 请求，同步异步消息，路由和寻址 等； 　　4) 集成： 遗留系统适配器，服务编排和映射，协议转换，数据变换，企业应用集成 中间件 的连续等。 　　5) 服务交互： 服务接口定义，服务实现的置换，服务消息模型，服务目录和发现等。 　　6) 服务安全： 认证和授权、不可否认和机密性、安全标准的支持等； 　　7) 服务质量： 事务，服务的可交付性等； 　　8) 服务等级： 性能、可用性等。 　　ESB 中最常提到的两个功能是消息转换和消息路由。


# 各产品实现了哪些核心功能和扩展功能？

服务是交互的，所以服务一定会有请求和响应。体现为一种调用（call，可以是同步或一步）
服务不是消息，服务的参数和返回值可以是消息。

SOA中的一个服务是一组远程调用。与传统的远程调用体系结构（如CORBA）或组件模型（如EJB，COM+）不同：
SOA的服务更强调服务描述的一致性和开放性

服务应该有较粗的粒度，要有业务意义上的价值

为了实现请求方与提供方的松耦合，强调应该描述服务的接口，根据接口可以有多个实现




# SOA中的服务操作

- SOA是策略、实践和框架/设计方法/体系结构/原则和方法论
- 服务具有明确的业务功能/与业务一致/满足业务需求
- 服务有合适的粒度, 服务可以重用
- 服务可以调用、发布和发现
- 服务抽象成接口的形式
- 服务的双方（提供者，使用者）松耦合
- 服务以组件的形式构建/服务不是子系统、系统或组件/将应用程序功能作为服务

哪些操作由ESB来解决？

# ESB的生存环境

没有服务的ESB是无源之水，没有消息总线的ESB是无根之木。要让ESB更好的发挥作用，还需要一系列的企业架构组件进行配合：

-
- MQ
- 规则引擎
- 流程中心
- UDDI

## 解决分布式系统的RPC问题

SOA中的各个服务分布在不同的系统中，系统之间的调用是典型的RPC（Remote Process Call）。
很不幸，ESB承担了这个

# ESB的实现

从架构角度来说，ESB是一个中间件

# 对ESB的典型误用



# 参考资料

- [SOA 术语概述: 第 1 部分，服务、体系结构、治理和业务术语](http://www.ibm.com/developerworks/cn/webservices/ws-soa-term1/)
- [关于ESB实施的几点建议](http://www.infoq.com/cn/articles/mgy-esb-implementation-suggestion)