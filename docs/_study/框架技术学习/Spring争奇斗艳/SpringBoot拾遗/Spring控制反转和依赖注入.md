# Spring控制反转和依赖注入
     控制反转(IOC：Inversion of Control)，也叫依赖注入(DI：dependency injection)，是对象通过构造参数、工厂方法参数、对象实例创建以后设置属性或者工厂方法返回属性的方式，定义依赖项的过程。然后容器在创建 bean 时注入这些依赖项。这是bean自己控制实例化和位置(通过直接构造方法或定位器模式的机制)的逆过程(因此得名，控制反转).
     `org.springframework.beans`和`org.springframework.context`包是`Spring Framework`的`IoC`容器的基础。`BeanFactory`接口提供了一种能够管理任何类型对象的高级配置机制。`ApplicationContext`是`BeanFactory`的一个子接口，添加了如下功能。
* 更容易与 Spring 的 AOP 特性集成

* 消息资源处理（用于国际化）

* 活动发布

* 应用层特定上下文，例如用于Web应用程序的`WebApplicationContext`。