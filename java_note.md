# JAVA

## 1、 JVM

### JVM 运行时数据区包括哪些部分？哪些区域是线程私有的？

JVM 运行时数据区分为：程序计数器、Java 虚拟机栈、本地方法栈、Java 堆、方法区；还有一块直接内存不属于运行时数据区。

- 线程私有（每个线程独立，随线程创建销毁）：程序计数器、Java 虚拟机栈、本地方法栈。

- 线程共享（所有线程共用，JVM 启动创建，关闭销毁）：Java 堆、方法区。


### 程序计数器、虚拟机栈和本地方法栈分别有什么作用？

- 程序计数器：当前线程执行的字节码行号指示器。Java 多线程靠线程切换，每个线程私有，用来记录下一条要执行的指令地址；唯一一个没有 OOM 的区域。执行 native 方法时，计数器值为空。

- Java 虚拟机栈：Java 方法执行的内存模型，每个 Java 方法执行时都会创建一个栈帧，存放局部变量表、操作数栈、动态链接、方法返回地址等。方法调用就是栈帧入栈，方法结束栈帧出栈。

- 本地方法栈：作用和虚拟机栈类似，只不过服务于Native 本地方法（C/C++ 写的方法），HotSpot 中本地方法栈和虚拟机栈是合二为一的。


# 框架

## 1、 基础

### Spring Cloud 和 Spring Cloud Alibaba组件对比

|能力|Spring Cloud Netflix（原生老实现）|Spring Cloud Alibaba|
|---|---|---|
|服务注册 & 发现|Eureka（停更）|Nacos（注册中心 + 配置中心二合一）|
|配置中心|Config + Bus|Nacos|
|服务远程调用|OpenFeign|OpenFeign / Dubbo（可选，高性能RPC）|
|熔断、降级、限流|Hystrix（停更）|Sentinel（功能更强，支持限流、熔断、热点参数限流）|
|网关|Zuul（阻塞，停更）|Spring Cloud Gateway（SCA不自带网关，共用官方Gateway）|
|分布式事务|无原生组件|Seata（AT/TCC/SAGA，国产分布式事务）|
|消息驱动|Spring Cloud Stream （Rabbit/Kafka）|Spring Cloud Stream + RocketMQ Binder|
|负载均衡|Ribbon（停更）|Spring Cloud LoadBalancer（新版SpringCloud默认）|


## 2、 Zookeeper

### Zookeeper是什么

Zookeeper是一个分布式协调服务，不是数据库，不是消息队列。

它提供小型树形目录（ZNode）存储元数据，具备强一致性、Watcher监听、会话机制，用来解决分布式系统里的协调问题：服务注册发现、分布式锁、集群选主、配置管理。


### Zookeeper的数据模型是什么？什么是ZNode？

ZK采用树形层级模型，类似Linux文件系统，根是 `/`，树上每一个节点叫ZNode。

- ZNode可以存少量数据（建议 < 1MB，不适合业务大数据）；

- 每个ZNode自带版本号、创建时间等元信息；

- ZNode分为4中类型：持久、临时、持久顺序、临时顺序。


### ZNode的4种类型，分别有什么特点？

1.持久节点 (Persistent): 创建后永久保存，客户端断开连接不会删除。多用于配置存储。

2.临时节点 (Ephemeral): 绑定客户端Session，会话失效（进程宕机、网络断开、超时）节点自动删除。临时节点不能有子节点

3.持久顺序节点 (Persistent Sequential): 持久节点，创建时ZK自动在名称后追加全局自增序号。

4.临时顺序节点 (Ephemeral Sequential): 临时 + 自增序号，分布式锁最常用。
