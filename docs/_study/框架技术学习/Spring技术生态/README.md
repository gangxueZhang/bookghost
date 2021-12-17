# Spring技术生态
## SpringAmqp

    Spring AMQP项目是应用Spring核心概念开发的基于`AMQP`的消息传递解决方案。 它为发送和接收消息提供了一个高级抽象的`template`。 它还通过`listener container`为消息驱动的`POJO` 提供支持。 这些库促进了`AMQP`资源的管理，同时促进了依赖注入和声明性配置的使用。 在所有这些情况下，您将看到与`Spring Framework`中` JMS` 支持的相似之处。

    该项目由两部分组成； `spring-amqp` 是基础抽象，`spring-rabbit` 是`RabbitMQ` 实现。

## SpringBatch

    一个轻量级、全面的批处理框架，旨在支持开发对企业系统日常运营至关重要的强大的批处理应用程序。

    Spring Batch 提供了在处理大量记录时必不可少的可重用功能，包括日志记录/跟踪、事务管理、作业处理统计、作业重启、跳过和资源管理。 它还提供更先进的技术服务和功能，通过优化和分区技术实现极高容量和高性能的批处理作业。 简单和复杂的大容量批处理作业都可以以高度可扩展的方式利用该框架来处理大量信息。

## SpringBoot

    `Spring Boot`可以轻松创建独立的、生产级的基于 Spring 的应用程序，您可以"直接运行"这些应用程序。

    我们对`Spring`平台和第三方库提供了默认的配置，因此您可以轻松上手。 大多数`Spring Boot`应用程序需要最少的`Spring`配置。

## SpringCloud

    `Spring Cloud`为开发者提供了快速构建分布式系统中一些常见模式的工具（例如配置管理、服务发现、断路器、智能路由、微代理、控制总线、一次性令牌、全局锁、领导选举、分布式会话、集群状态）。分布式系统的协调采用样板模式，使用`Spring Cloud`开发人员可以快速建立实现这些模式的服务和应用程序。 它们将在任何分布式环境中运行良好，包括开发人员自己的笔记本电脑、裸机数据中心和`Cloud Foundry`等托管平台。

## SpringCloudDataFlow

    用于`Cloud Foundry`和`Kubernetes`的基于微服务的流式和批处理数据处理。

    `Spring Cloud Data Flow`提供了为流和批处理数据管道创建复杂拓扑的工具。 数据管道由使用`Spring Cloud Stream`或`Spring Cloud Task`微服务框架构建的`Spring Boot`应用程序组成。

    `Spring Cloud Data Flow `持一系列数据处理用例，从`ETL`到导入/导出、事件流和预测分析。

## SpringCredHub

    `Spring CredHub`为在`Cloud Foundry`平台中运行的`CredHub`服务器中存储、检索和删除凭据提供客户端支持。

    `CredHub`提供了一个`API`来安全地存储、生成、检索和删除各种类型的凭据。 `Spring CredHub`为`CredHub API`提供` Java`绑定，可以轻松地将`Spring`应用程序与`CredHub`集成。

## SpringData

    `Spring Data`的使命是为数据访问提供熟悉且一致的基于`Spring`的编程模型，同时仍保留底层数据存储的特殊特征。

    它让使用数据访问技术、关系和非关系数据库、`map-reduce`框架和基于云的数据服务变得容易。 这是一个伞形项目，其中包含许多给定数据库的专用子项目。 这些项目是通过与这些技术背后的许多公司和开发商合作开发的。

## SpringFlo

    `Spring Flo`是一个`JavaScript`库，它为管道和简单图形提供了一个基本的可嵌入`HTML5`可视化构建器。 该库用作`Spring Cloud Data Flow`中流构建器的基础。

    `Flo`包括集成流设计器的所有基本元素，例如连接器、控制节点、调色板、状态转换和图形拓扑——重要的是，有一个文本外壳、`DSL`支持和一个图形画布，用于创建和审查综合工作流 .

## SpringFramework

    `Spring Framework`为现代基于`Java`的企业应用程序提供了一个全面的编程和配置模型 - 在任何类型的部署平台上。

    `Spring`的一个关键元素是应用程序级别的基础设施支持：`Spring`专注于企业应用程序的“管道”，以便团队可以专注于应用程序级业务逻辑，而无需与特定部署环境产生不必要的联系。

## SpringGraphQL

    `Spring GraphQL`为基于`GraphQL Java`构建的`Spring `应用程序提供支持。 这是两个团队之间的联合协作。 我们的共同理念是减少固执己见，更专注于全面和广泛的支持。

    `Spring GraphQL`是`GraphQL Java`团队的`GraphQL Java Spring`项目的继承者。 它旨在成为所有`Spring`、`GraphQL`应用程序的基础。

    该项目目前正处于 1.0 版本的里程碑阶段，正在寻找反馈。 请使用我们的问题跟踪器报告问题、讨论设计问题或请求功能。

## SpringHateoas

    `Spring HATEOAS`提供了一些`API`来简化创建遵循`HATEOAS`原则的`REST`表示，当使用`Spring`尤其是`Spring MVC`时。 它试图解决的核心问题是链接创建和表示组装。

## SpringIntegration

    扩展 Spring 编程模型以支持著名的企业集成模式。 Spring Integration 在基于 Spring 的应用程序中启用轻量级消息传递，并支持通过声明式适配器与外部系统集成。 这些适配器在 Spring 对远程处理、消息传递和调度的支持上提供了更高级别的抽象。 Spring Integration 的主要目标是为构建企业集成解决方案提供一个简单的模型，同时保持关注点分离，这对于生成可维护、可测试的代码至关重要。

## SpringKafka

​     Spring for Apache Kafka (spring-kafka) 项目将核心 Spring 概念应用于基于 Kafka 的消息传递解决方案的开发。 它提供了一个“模板”作为发送消息的高级抽象。 它还通过@KafkaListener 注释和“侦听器容器”为消息驱动的 POJO 提供支持。 这些库促进了依赖注入和声明式的使用。 在所有这些情况下，您将看到与 Spring Framework 中的 JMS 支持和 Spring AMQP 中的 RabbitMQ 支持的相似之处。

## SpringLdap

## SpringRestDocs

## SpringRoo

## SpringSecurity

## SpringSession

## SpringShell

## SpringStatemachine

## SpringVault

## SpringWebFlow

## SpringWebServices



