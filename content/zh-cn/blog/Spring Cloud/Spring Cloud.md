---
title: "Spring Cloud"
linkTitle: ""
author: "XW"
authorLink: ""
date: 2023-07-12T23:33:21+08:00
lastmod: ""
description: ""
draft: false

---

## 微服务
> 不是一个框架，而是一构建思想。
>
> 它时用来描述将如那件应用程序设计为独立部署的服务的一种特殊方式。
>
> 微服务构架的系统是个 **<font color="red">分布式系统</font>**，按业务领域划分为 **<font color="red">独立的服务单元</font>**，有自动化运维、容错、快速演进的特点，它能够解决传统单体架构系统的痛点，同时也能满足越来越复杂的业务需求

### 什么是微服务？

就是将一个大的应用，拆分成多个小的模块，每个模块都有自己的功能和职责，每个模块可以进行交互，这就是微服务

“微服务”的发明人，`Martin Fowler`的理解：

> 简而言之，微服务架构的风格，就是将单一程序开发成一个微服务每个微服务运行在自己的进程中，并使用 <font color="red">**轻量级机制通信** </font>，通常是**HTTP RESTFUL API**。
> 这些服务围绕业务能力来划分构建的，并通过完全自动化部署机制来独立部署这些服务可以使用不同的编程语言，以及不同数据存储技术，以保证最低限度的集中式管理

### 微服务的特点

优点：

1.按业务(功能)划分为一个独立运行的程序，即服务单元
2.服务之间通过 HTTP 协议相互通信。 http 是一个万能的协议 (web 应用都支持的模式)
3.自动化部署
4.可以用不同的编程语言
5.可以用不同的存储技术
6.服务集中化管理。
7.微服务是一个分布式系统

缺点：

1.微服务的**复杂度**
2.分布式**事务问题**
3.服务的划分(按照**功能**划分 还是按照**组件**来划分呢) 分工

4.服务的**部署**(不用自动化部署 自动化部署)

### CI/CD(持续集成/持续交付) 运维

> 微服务的自动化部署，部署多个jar包

开源软件项目:`Jenkins`

### 微服务架构的设计原则(项目搭建)

> **开闭原则 单一原则**  六大设计原则架构设计和代码设计思路一样的
>
> 23种设计模式

在微服务架构中的三大难题：**服务故障的传播性(熔断)、服务的划分和分布式事务**。



## Spring Cloud

### Spring Cloud介绍

Spring cloud 作为 Java 语言的微服务框架，它依赖于 Spring Boot，有快速开发、持续交付和容易部署等特点。

开源社区Spring官方和Netflix、Pivotal、alibaba提供支持

Spring cloud 的**首要目标就是通过提供一系列开发组件和框架，帮助开发者迅速搭建一个分布式的微服务系统。**

 Spring cloud 是通过包装其他技术框架来实现的，例如包装开源的 Netflix oss 组件，实现了一套通过基于注解、 Java 配置和基于模版开发的微服务框架。

 spring cloud 提供了开发分布式微服务系统的一些常用组件，例如服务注册和发现、配置中心、熔断器、远程调用，智能路由、微代理、控制总线、全局锁、分布式会话等。

### Spring Cloud版本对应关系

命名：ABCDE**FGH**I(2020版)

新版本：年份命名

![image-20230713003533882](Spring%20Cloud.assets/image-20230713003533882-16891797348915.png)

Spring Cloud和Spring Boot版本对应关系：

[Spring Cloud](https://spring.io/projects/spring-cloud#learn)

[start.spring.io/actuator/info](https://start.spring.io/actuator/info)

Alibaba Spring Cloud支持版本对应：

[版本说明 · alibaba/spring-cloud-alibaba Wiki (github.com)](https://github.com/alibaba/spring-cloud-alibaba/wiki/版本说明/662c076ef8f4af0ec94caa86ade169442684904f)

### Spring Cloud常用组件表

**<font color="red">服务的注册和发现。(`eureka`-netflix,`nacos`-spring官方,`consul`-alibaba) </font>**

**服务的负载均衡。(`ribbon`,`dubbo`)**

服务的相互调用。(`openFeign`,`dubbo`) 

服务的容错。 (`hystrix`，`sentinel`)

服务网关。 (`gateway`，`zuul`)

服务配置的统一管理。(`config-server`,`nacos`,`apollo`)

服务消息总线。(`bus`)

服务安全组件。(`security`,Oauth2.0)

服务监控。(`admin`)  (`jvm`)

链路追踪。(`sleuth`+`zipkin`)

#### 服务的注册和发现(Eureka)

> 注册发现中心
> Eureka 来源于古希腊词汇，意为“发现了”。在软件领域，Eureka 是 **Netflix 在线影片公司**开源的一个**服务注册与发现的组件**，和其他Netflix 公司的服务组件 (例如负载均衡熔断器、网关等) 一起，被Spring cloud 社区整合为 **Spring cloud Netflix 模块**。Eureka 是Netflix 贡献给 Spring cloud 的一个框架! 
>
> Netflix 给 Spring cloud 贡献了很多框架

##### Spring Cloud Eureka 和 Zookeeper的区别

###### 什么是 CAP 原则(面试)

在分布式微服务里面CAP定理
`问: 为什么 zookeeper 不适合做注册中心?`
CAP 原则又称 CAP 定理，指的是在一个分布式系统中
**<font color="red">一致性</font> (Consistency)** :

三个机器中的数据中一致的 `C`
**<font color="red">可用性</font> (Availability)** :

当有一个节点挂掉了整个集群可以继续对外提供服务`A`
**<font color="red">分区容错性</font> (Partition tolerance) (这个特性是不可避免的)**

由于机房网络或者分区等原因会导致各个机器中的数据短暂不一致`P`

**<font color="red">CAP 原则指的是，这三个要素最多只能同时实现两点，不可能三者兼顾</font>**

CP(数据是一致的但如果有节点挂了，整个服务在几分钟的时间内不能提供服务)

AP(数据可能是不一致的)

> Eureka和Zookeeper区别：
>
> Zookeeper遵循CP原则
>
> Eureka遵循AP原则(高可用)

##### 搭建一个注册中心

###### `eureka-server`

> 注册中心：可用让别人注册，还可以自己注册自己

Springboot项目导包：

```xml
	<dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
	</dependency>
```

1.`pom.xml`

> springboot和sprincloud版本对应

```xml
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>2.3.12.RELEASE</version>
```

```xml
<spring-cloud.version>Hoxton.SR12</spring-cloud.version>
```

2.`application.properties`

```properties
# eureka端口 默认8761
server.port=8761
# 应用名称 一般：eureka-server
spring.application.name=eureka-server
```

3.`EurekaServerApplication`

```java
// 开启eureka注册中心的功能
@EnableEurekaServer
```

启动springboot，访问http://localhost:8761/

![image-20230713142920735](Spring%20Cloud.assets/image-20230713142920735-16892297661181.png)

实例ID：localhost:eureka-server:8761

###### `eureka-client-a`

> 客户端a

Springboot导包：

```xml
 		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
```

1.`pom.xml`

> springboot和sprincloud版本对应

```xml
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-parent</artifactId>
<version>2.3.12.RELEASE</version>
```

```xml
<spring-cloud.version>Hoxton.SR12</spring-cloud.version>
```

2.`application.properties`

```properties
# 客户端端口无要求
server.port=8080
# 应用名称
spring.application.name=eureka-server-a
# 注册：将信息(ip和port等)发送到注册中心
# 指定注册地址
eureka.client.service-url.defaultZone=http://localhost:8761/eureka
```

3.`EurekaClientAApplication`

```java
// 开启客户端的功能
@EnableEurekaClient
```

启动：

![image-20230713144713741](Spring%20Cloud.assets/image-20230713144713741-16892308346772.png)

`eureka-client-b`客户端b

> 

###### 配置文件

> eureka三类配置：server client instance

服务端配置

```properties
# eureka端口 默认8761
server.port=8761
# 应用名称 一般：eureka-server
spring.application.name=eureka-server
# 服务端间隔多少毫秒做定期删除的操作
eureka.server.eviction-interval-timer-in-ms=10000
# 续约百分比
eureka.server.renewal-percent-threshold=0.85
# 主机名称：应用名称：端口号
eureka.instance.instance-id=${eureka.instance.hostname}:${eureka.instance.name}:${server.port}
# 主机名称/服务IP
eureka.instance.hostname=localhost
# 以IP的形式显示具体的服务信息
eureka.instance.prefer-ip-address=true
# 服务实例(eureka-server即使服务端也是客户端)的续约时间间隔
eureka.instance.lease-renewal-interval-in-seconds=5
```

客户端配置

```properties
# 客户端端口无要求
server.port=8080
# 应用名称
spring.application.name=eureka-server-a
# 指定注册地址
eureka.client.service-url.defaultZone=http://localhost:8761/eureka
# eureka-server注册开关
eureka.client.register-with-eureka=true
# 应用是否拉去服务列表到本地
eureka.client.fetch-registry=true
# 为了缓解服务列表的脏读问题，时间越短藏独越少 性能消耗越大
eureka.client.registry-fetch-interval-seconds=10
# 应用的主机名称(主机IP)
eureka.instance.hostname=localhost
eureka.instance.instance-id=${eureka.instance.hostname}:${spring.application.name}:${server.port}
# 显示IP
eureka.instance.prefer-ip-address=true
# 续约时间
eureka.instance.lease-renewal-interval-in-seconds=10
```

##### 高可用的Eureka-Server集群

> 复制配置文件

集群的方案：中心化集群、主从模式集群、去中心化集群

###### 注册中心集群（去中心化集群）

> 没有主从概念。eureka会将数据进行广播和扩散**（相互注册）**

`eureka-server-A、eureka-server-B、eureka-server-C`

> 应用名称不变
>
> 端口 8761、8762、8763
>
> 开启注解@EnableEurekaServer

`eureka-server-A`——`application.properties`

 ```properties
 # 默认在8761注册
 eureka.client.service-url.defaultZone=http://localhost:8762/eureka,http://localhost:8763/eureka
 ```

`eureka-server-B`——`application.properties`

 ```properties
 # 默认在8761注册
 eureka.client.service-url.defaultZone=http://localhost:8761/eureka,http://localhost:8763/eureka
 ```

`eureka-server-C`——`application.properties`

 ```properties
 # 默认在8761注册
 eureka.client.service-url.defaultZone=http://localhost:8761/eureka,http://localhost:8762/eureka
 ```
![image-20230715021754379](Spring%20Cloud.assets/image-20230715021754379-16893586759521.png)

![image-20230715022703622](Spring%20Cloud.assets/image-20230715022703622-16893592243142.png)

准备三个台服务器，将eureka-server部署到三个不同的服务器里，实现集群。

一台电脑模拟集群，本地修改`host`文件

`C:\Windows\System32\drivers\etc\HOSTS`

```
127.0.0.1 peer1
127.0.0.1 peer2
127.0.0.1 peer3
```

虚拟映射地址

修改各自的配置文件`application.properties`的主机名和注册地址

![image-20230715022822204](Spring%20Cloud.assets/image-20230715022822204-16893593033343.png)

客户端修改配置文件`application.properties`向eureka-server注册

```properties
eureka.client.service-url.defaultZone=http://peer1:8761/eureka,http://peer2:8762/eureka,http://peer3:8763/eureka
```



![image-20230715030244727](Spring%20Cloud.assets/image-20230715030244727-16893613661814.png)



###### 分布式数据一致性协议`Paxos` `raft`(主从模式集群)

> 在有主从模式的集群中，遵循这这些协议
>
> [Raft (thesecretlivesofdata.com)](http://thesecretlivesofdata.com/raft/)

1.数据同步更新

Raft：客户端A将数据1---->主节点：服务端A(数据1存放到日志中，未更新节点的值)---->副节点：服务端B...---->回应主节点(主节点更新)---->副节点更新

> 当集群中的大部分节点，都做出了响应时，进行同步

2.主节点选举

每个节点都有一个随机的苏醒时间，谁先苏醒谁成为候选节点(给自己投一票)，候选节点向其它节点发送投票请求，接收节点未投票则将票投给候选节点，候选节点获得多数票时成为领导者(主节点)

主节点向所有副节点发送心跳（数据同步更新），当主节点宕机后，副节点重新选举。

> 当有两个节点同时成为候选节点，则重新洗牌(重置苏醒时间)





##### Eureka 概念的理解

1. 服务的注册
   当项目启动时 (**eureka的客户端**)，就会向 `eureka-server` 发送自己的**元数据 (原始数据)**<font color="red">(运行的ip，端口 port，健康的状态监控等，因为使用的是 `http/ResuFul`请求风格)</font>`eureka-server` 会在**自己内部保留这些元数据**(内存中)。(有一个服务列表) (`restful` 风格，以http动词(**GET（获取资源）、POST（创建资源）、PUT（更新资源）和DELETE（删除资源）**)的请求方式，完成对url资源的操作)

> [一文详解RESTful风格 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/617187396)

1. 服务的续约

   项目启动成功了，除了向`eureka-server` 注册自己成功，还会**定时**的向`eureka-serve`r汇报自己，**心跳**，表示自己还活着。(修改一个时间)

2. 服务的下线(主动下线)
   当项目关闭时，会给 `eureka-server` 报告，说明自己要下机了

3. 服务的剔除(被动下线，主动剔除)
   当项目**超过了指定时间**没有向 `eureka-server` 汇报自己，那么 `eureka-server` 就会认为此节点死掉了，会把它剔除掉，也不会放流量和请求到此节点了

### Spring Cloud实现方案

> SpringCloud 就是微服务理念的一种**具体落地实现方式**

Dubbo+Zookeeper 半自动化的微服务实现架构

SpringCloud Netflix 一站式微服务架构

SpringCloud Alibaba 新的一站式微服务架构
