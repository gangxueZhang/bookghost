# JVM知识汇总

## JVM基本情况介绍

    JVM是Java Virtual Machine（Java虚拟机）的缩写，JVM是一种用于计算设备的规范，它是一个虚构出来的计算机，是通过在实际的计算机上仿真模拟各种计算机功能来实现的，因此JVM本身设计遵循冯诺依曼计算机结构。

![image-20210628091056610](_images/JVM知识汇总/image-20210628091056610.png)

    Java语言的一个非常重要的特点就是与平台的无关性。而使用Java虚拟机是实现这一特点的关键。一般的高级语言如果要在不同的平台上运行，至少需要编译成不同的目标代码。而引入Java语言虚拟机后，Java语言在不同平台上运行时不需要重新编译。Java语言使用Java虚拟机屏蔽了与具体平台相关的信息，使得Java语言编译程序只需生成在Java虚拟机上运行的目标代码（字节码），就可以在多种平台上不加修改地运行。Java虚拟机在执行字节码时，把字节码解释成具体平台上的机器指令执行。这就是Java的能够“一次编译，到处运行”的原因。

<img src="../../../_study/开发语言学习/重学Java语言/_images/JVM知识汇总/image-20210625065736355.png" alt="image-20210625065736355" style="zoom:50%;" />

从上图能清晰看到Java平台包含的各个逻辑模块，也能了解到JDK与JRE的区别，对于JVM自身的物理结构，我们可以从下图鸟瞰一下：

|                        JVM平台无关性                         |                      JVM自身的物理结构                       |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| ![image-20210628091203344](_images/JVM知识汇总/image-20210628091203344.png) | ![image-20210628091219303](_images/JVM知识汇总/image-20210628091219303.png) |

## JAVA代码编译和执行过程

    java代码编译是由Java源码编译器来完成，流程图如下所示：

![image-20210628090546137](_images/JVM知识汇总/image-20210628090546137.png)

Java字节码的执行是由JVM执行引擎来完成，流程图如下所示：

![image-20210628090658132](_images/JVM知识汇总/image-20210628090658132.png)

## JVM中的类加载机制

* **类加载过程**

![image-20210628090831186](_images/JVM知识汇总/image-20210628090831186.png)

* **类加载器优先级和功能**

![image-20210628090738752](_images/JVM知识汇总/image-20210628090738752.png)

## JVM内存模型

| 1.8之前 | 1.8开始 |
| :-----: | :-----: |
|         |         |

## JVM调优参数

## JVM垃圾回收

## JVM垃圾回收算法

### 标记-清除(Mark-Sweep)

### 标记-复制(Mark-Copying)

### 标记-整理(Mark-Compact)

### 分代收集算法

## JVM垃圾回收器

### Serial

### Serial Old

### ParNew

### Parallel Scavenge

### Parallel Old

### CMS

### G1(Garbage-First)

### ZGC

## 执行引擎

        JVM采取的是混合模式，也就是解释+编译的方式，对于大部分不常用的代码，不需要浪费时间将其编 译成机器码，只需要用到的时候再以解释的方式运行;对于小部分的热点代码，可以采取编译的方式， 追求更高的运行效率。

### 解释执行

        Interpreter，解释器逐条把字节码翻译成机器码并执行，跨平台的保证。刚开始执行引擎只采用了解释执行的，但是后来发现某些方法或者代码块被调用执行的特别频繁时，就会把这些代码认定为“热点代码”。

#### 热点代码探测

* 方法调用计数器
* 回边计数器



### JIT即时编译器

        Just-In-Time compilation(JIT)，即时编译器先将字节码编译成对应平台的可执行文件，运行速度快。即时编译器会把这些热点代码编译成与本地平台关联的机器码，并且进行各层次的优化，保存到内存中。

#### 本地代码存储位置

* code cache

        JVM生成的native code存放的内存空间称之为Code Cache；JIT编译、JNI等都会编译代码到native code，其中JIT生成的native code占用了Code Cache的绝大部分空间。

* 方法内联



#### 即使编译器类型

* HotSpot虚拟机里面内置了两个JIT:C1和C2

> C1也称为Client Compiler，适用于执行时间短或者对启动性能有要求的程序 
> C2也称为Server Compiler，适用于执行时间长或者对峰值性能有要求的程序

* Java7开始，HotSpot会使用分层编译的方式

> 也就是会结合C1的启动性能优势和C2的峰值性能优势，热点方法会先被C1编译，然后热点方法中的热点会被 C2再次编译









## 附录A

### 参考资料

```
* 《Java 虚拟机规范(Java SE 8版)》
* 《深入理解 Java 虚拟机 第二版》
* 《深入理解 Java 虚拟机 第三版》
* 《实战 Java 虚拟机》
```

```http
JVM_虚拟机目录: https://blog.csdn.net/TZ845195485/article/details/93238857
压缩指针失效_JVM内存超32G题:https://blog.csdn.net/m0_37670016/article/details/112794887
压缩指针失效: https://www.baeldung.com/jvm-compressed-oops
数据存储说明：https://baike.baidu.com/item/存储单位/3943356?fromtitle=计算机存储单位&fromid=795305&fr=aladdin
Java GC机制详解: https://www.cnblogs.com/jobbible/p/9800222.html
JVM 中的永久代: https://blog.csdn.net/hehmxy/article/details/83317885
JVM内存：年轻代、老年代、永久代（推荐 转）:https://blog.csdn.net/weixin_30642305/article/details/98390172?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-2.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-2.control
JVM配置参数列表: https://www.cnblogs.com/fightingcode/p/11232694.html
```



