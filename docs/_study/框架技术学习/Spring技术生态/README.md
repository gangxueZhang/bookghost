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

## SpringForApacheKafka

​     Spring for Apache Kafka (spring-kafka) 项目将核心 Spring 概念应用于基于 Kafka 的消息传递解决方案的开发。 它提供了一个“模板”作为发送消息的高级抽象。 它还通过@KafkaListener 注释和“侦听器容器”为消息驱动的 POJO 提供支持。 这些库促进了依赖注入和声明式的使用。 在所有这些情况下，您将看到与 Spring Framework 中的 JMS 支持和 Spring AMQP 中的 RabbitMQ 支持的相似之处。

## SpringForGraphQL

    `Spring GraphQL`为基于`GraphQL Java`构建的`Spring `应用程序提供支持。 这是两个团队之间的联合协作。 我们的共同理念是减少固执己见，更专注于全面和广泛的支持。

    `Spring GraphQL`是`GraphQL Java`团队的`GraphQL Java Spring`项目的继承者。 它旨在成为所有`Spring`、`GraphQL`应用程序的基础。

    该项目目前正处于 1.0 版本的里程碑阶段，正在寻找反馈。 请使用我们的问题跟踪器报告问题、讨论设计问题或请求功能。

## SpringFramework

    `Spring Framework`为现代基于`Java`的企业应用程序提供了一个全面的编程和配置模型 - 在任何类型的部署平台上。

    `Spring`的一个关键元素是应用程序级别的基础设施支持：`Spring`专注于企业应用程序的“管道”，以便团队可以专注于应用程序级业务逻辑，而无需与特定部署环境产生不必要的联系。

## SpringHateoas

    `Spring HATEOAS`提供了一些`API`来简化创建遵循`HATEOAS`原则的`REST`表示，当使用`Spring`尤其是`Spring MVC`时。 它试图解决的核心问题是链接创建和表示组装。

## SpringIntegration

    扩展 Spring 编程模型以支持著名的企业集成模式。 Spring Integration 在基于 Spring 的应用程序中启用轻量级消息传递，并支持通过声明式适配器与外部系统集成。 这些适配器在 Spring 对远程处理、消息传递和调度的支持上提供了更高级别的抽象。 Spring Integration 的主要目标是为构建企业集成解决方案提供一个简单的模型，同时保持关注点分离，这对于生成可维护、可测试的代码至关重要。



## SpringLdap

    Spring LDAP是一个用于简化Java中LDAP编程的库，其建立原理与Spring Jdbc相同。

    LdapTemplate类封装了传统LDAP编程中涉及的所有管道工作，例如创建，遍历NamingEnumerations，处理异常和清理资源。这使程序员可以处理重要的事情-在哪里查找数据（DN和筛选器）以及如何处理数据（与域对象进行映射，绑定，修改，取消绑定等），方法与JdbcTemplate减轻负担的方式相同除了实际的SQL之外的所有程序员，以及数据如何映射到域模型。

    除此之外，Spring LDAP还提供了从NamingExceptions到未检查的异常层次结构的异常转换，以及用于处理过滤器，LDAP路径和属性的多个实用程序。

## SpringRestDocs

    Spring REST Docs可帮助您记录RESTful服务。

    它结合了用[Asciidoctor](http://asciidoctor.org/)编写的手写文档和[Spring MVC Test](https://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle#spring-mvc-test-framework)生成的自动生成的代码片段。这种方法使您摆脱了[Swagger之](http://swagger.io/)类的工具所产生的文档限制。

    它可以帮助您生成准确，简洁且结构合理的文档。然后，该文档可让您的用户以最少的麻烦获得他们所需的信息。

## SpringSecurity

    Spring Security是一个功能强大且高度可定制的身份验证和访问控制框架。它是用于保护基于Spring的应用程序的实际标准。

    Spring Security是一个框架，致力于为Java应用程序提供身份验证和授权。像所有Spring项目一样，Spring Security的真正强大之处在于它可以轻松扩展以满足定制需求的能力。

## SpringSession

    Spring Session提供了用于管理用户会话信息的API和实现。

## SpringShell

    Spring Shell项目的用户可以通过依赖于Spring Shell jar并添加自己的命令（在Spring bean中作为方法来提供）来轻松构建功能完备的shell（又名命令行）应用程序。创建命令行应用程序可能非常有用，例如与项目的REST API交互或使用本地文件内容。

## SpringStatemachine

    Spring Statemachine是一个框架，供应用程序开发人员在Spring应用程序中使用状态机概念。

## SpringVault

    Spring Vault为访问，存储和吊销机密提供了熟悉的Spring抽象和客户端支持。它提供了与Vault交互的低层和高层抽象，使用户摆脱了基础设施方面的顾虑。

## SpringWebFlow

    Spring Web Flow建立在Spring MVC的基础上，并允许实现Web应用程序的“流”。流程封装了一系列步骤，这些步骤指导用户完成某些业务任务。它跨越多个HTTP请求，具有状态，处理事务数据，可重用，并且本质上可能是动态的并且可以长时间运行。

## SpringWebServices

    Spring Web Services（Spring-WS）是Spring社区的产品，致力于创建文档驱动的Web服务。Spring Web Services旨在促进合同优先SOAP服务的开发，从而允许使用多种操作XML有效负载的方式之一来创建灵活的Web服务。该产品基于Spring本身，这意味着您可以将诸如依赖项注入之类的Spring概念用作Web服务的组成部分。

