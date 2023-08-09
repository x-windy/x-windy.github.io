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

# Spring Cloud常用组件

**<font color="red">服务的注册和发现。(`eureka`-netflix,`nacos`-spring官方,`consul`-alibaba) </font>**

**服务的负载均衡。(`ribbon`,`dubbo`)**

服务的相互调用。(`openfegin`,`dubbo`) 

服务的容错。 (`hystrix`，`sentinel`)

服务网关。 (`gateway`，`zuul`)

服务配置的统一管理。(`config-server`,`nacos`,`apollo`)

服务消息总线。(`bus`)

服务安全组件。(`security`,`Oauth2.0`)

服务监控。(`admin`)  (`jvm`)

链路追踪。(`sleuth`+`zipkin`)

| SpringCloud  | spring官方                                     | netflix                          | alibaba                           | 其它                                             |
| ------------ | ---------------------------------------------- | -------------------------------- | --------------------------------- | ------------------------------------------------ |
| 注册中心     | consul                                         | <font color="red">eureka</font>  | <font color="red">nacos</font>    | <font color="red">apache/zookeeper</font>        |
| 负载均衡     |                                                | <font color="red">ribbon</font>  | dubbo                             | GRPC                                             |
| 远程调用     | <font color="red">openFeign(集成ribbon)</font> |                                  | dubbo                             |                                                  |
| 熔断器       |                                                | <font color="red">hystrix</font> | <font color="red">sentinel</font> |                                                  |
| 网关         | <font color="red">gateway</font>               | zuul                             | ----------                        |                                                  |
| 配置文件中心 | configServer                                   |                                  | <font color="red">nacos</font>    | 携程/Apollo                                      |
| 监控组件     | <font color="red">admin</font>                 |                                  | dubbo-admin                       |                                                  |
| 分布式事务   |                                                |                                  | <font color="red">seata</font>    | <font color="red">LCN/TCC/MQ</font>              |
| 消息队列     | stream                                         |                                  | <font color="red">rocketMq</font> | <font color="red">rabbitmq/kafka/activemq</font> |



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

服务的相互调用。(`openfegin`,`dubbo`) 

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

##### Eureka注册理解registry

> 客户端发送注册请求(**POST**)
>
> 服务端接收客户端的注册请求

保存服务实例数据的集合：

第一个key：应用名称(全大写)

Value中的Map的key：应用实例ID

Value中的Map的Value：具体的服务节点信息

```java
ConcurrentHashMap<String,Map<String,Lease<InstanceInfo>>> registry
```

##### Eureka续约renew

客户端发送时间

服务端更新时间（当前时间+续约时间）

##### Eureka剔除

当前时间＞（更新时间+续约时间）

##### Eureka服务发现

> clientA 通过服务的应用名称(例如：clientB)找到服务的具体实例的过程

编写Controller发现接口

```java
@RestController
public class DiscoveryController {
    @Autowired
    private DiscoveryClient discoveryClient;

    @GetMapping("test")
    public String discovery(String serverName){
        // 服务发现
        List<ServiceInstance> instances = discoveryClient.getInstances(serverName);
        instances.forEach(System.out::println);
        return instances.get(0).toString();
    }

}
```

访问接口：[localhost:8080/test?serverName=eureka-client-b](http://localhost:8080/test?serverName=eureka-client-b)

返回：

```
[EurekaDiscoveryClient.EurekaServiceInstance@75bc3ce1 instance = InstanceInfo [instanceId = localhost:eureka-client-b:8081, appName = EUREKA-CLIENT-B, hostName = 192.168.121.1, status = UP, ipAddr = 192.168.121.1, port = 8081, securePort = 443, dataCenterInfo = com.netflix.appinfo.MyDataCenterInfo@4dc123b5]
```

获取IP和端口，即可调用clientB的接口

```java
ServiceInstance serviceInstance = instances.get(0);
 // 获取IP和端口
String host = serviceInstance.getHost();
int port = serviceInstance.getPort();
System.out.println(host+":"+port);
```

##### Docker

Eureka-Server服务器(jar包)到Docker镜像部署

`eureka-server-1.0.jar`

```properties
eureka.client.service-url.defaultZone=${EUREKA_SERVER_URL:http://peer1:8761/eureka}
eureka.client.register-with-eureka=${REGISTER_WITH_EUREKA:true}
```

```
mkdir eureka-server
cd eureka-server
pwd
# 获取路径workdir
```


`Dockerfile`

```
FROM openjdk:8
# 环境变量
ENV workdir=/PAYH
# 将当前目录拷贝到workdir
COPY . ${workdir}
# 切换到workdir
WORKDIR ${workdir}
# 端口
EXPOSE 8761
CMD ["java","-jar","eureka-server.jar"]
```

run.sh

```sh
# 基于文件夹 -t:tag
cd .. && docker build ./eureka-server -t eureka-server:1.0
```

```sh
chmod 777 run.sh
./run.sh
doucker run --name eureka-server -p 8761:8761 -e REGISTER_WITH_EUREKA=false -d eureka-server:1.0
docker logs eureka-server
```

`run -p 端口 -d 后台运行(守护) --link 指定网络host文件映射的 -e REGISTER_WITH_EUREKA=false(指定参数) -v 文件挂载`

##### restTemplate使用

```xml
 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
```

编写测试接口

```JAVA
@RestController
public class TestController {

    @GetMapping("testGet")
    public String get(String name){
        System.out.println(name);
        return "ok";
    }

    /**
     * post接收参数
     * 1.json数据
     * header content-type=application/json
     * @RequestBody User user
     * 加注解
     * 2.表单数据
     * header content-type=x-www-form-undercode
     * User user
     * 不加注解
     * @return
     */
    @PostMapping("testPost")
    public String testPost(@RequestBody User user){
        System.out.println(user);
        return "ok";
    }
    
    @PostMapping("testPost2")
    public String testPost2(User user){
        System.out.println(user);
        return "ok";
    }
}
```

使用restTemplate发起请求

```java

@Test
    void testGet(){
        RestTemplate restTemplate = new RestTemplate();
        String url = "http://localhost:8080/testGet?name=zq";
        String object = restTemplate.getForObject(url, String.class);
        System.out.println(object);
    }

    @Test
    void testGet2(){
        RestTemplate restTemplate = new RestTemplate();
        String url = "http://localhost:8080/testGet?name=zq";
        ResponseEntity<String> responseEntity = restTemplate.getForEntity(url, String.class);
        // http:// 协议(规范,接头暗号)
        // 包含：请求头、请求参数、响应头、响应状态码、报文等。。。
        System.out.println(responseEntity);
    }


// <200,ok,[Content-Type:"text/plain;charset=UTF-8", Content-Length:"2", Date:"Sun, 16 Jul 2023 18:36:25 GMT", Keep-Alive:"timeout=60", Connection:"keep-alive"]>

// 表单参数传参
  @Test
    void testPost2(){
        RestTemplate restTemplate = new RestTemplate();
        String url = "http://localhost:8080/testPost2";
        // 构建表单参数
        LinkedMultiValueMap<String, Object> map = new LinkedMultiValueMap<>();
        map.add("name","zq");
        map.add("age",20);
        String s = restTemplate.postForObject(url, map, String.class);
        System.out.println(s);
    }
```



#### 客户端负载均衡工具(Ribbon)

> Spring Cloud Ribbon是一个基于HTTP和TCP的客户端**负载均衡**工具
>
> 将面向服务的REST模板请求自动转换成客户端负载均衡的服务调用
>
> 负载均衡的算法：轮询(请求次数 % 机器数量)、iphash、权重、随机
>
> 响应时间最短轮询、并发量最小轮询、过滤出符合条件的轮询、区域内亲和轮询算法
>
> Ribbon是netfix公司的一个开源项目，主要功能：**提供客户端负载均衡算法和服务调用**
>
> 两种方法结合使用：
>
> 1. RestTemplate
>
> 1. Openfegin

负载均衡

> Load Balance(LB)
>
> http://（http协议）
>
> lb://（负载均衡协议）
>
> 将负载（工作任务）进行平衡、分摊到 多个操作单元上进行运行

##### Ribbon的使用

> 先启动提供者，再启动消费者（不然消费者会先拉去注册中心的服务信息，没有启动者的服务信息）

`provider` `provider-b` 接口提供者

> eureka客户端

![image-20230725132719884](Spring%20Cloud.assets/image-20230725132719884-16902628418271.png)

`application.yml`

```yml
# 集群，端口不同
server:
  port: 8080
# 应用名一样
spring:
  application:
    name: provider
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka
  instance:
    hostname: localhost
    prefer-ip-address: true
    instance-id: ${eureka.instance.hostname}:${spring.application.name}:${server.port}
```

`ProviderController.java` 提供同样的接口

```java
@RestController
public class ProviderController {
    @GetMapping("hello")
    public String hello() {
        return "provider的接口";
    }
}
```



`consumer` 接口消费者，用户，使用ribbon达到负载均衡的效果

`application.yml`

```yml
# 集群，端口不同
server:
  port: 8082
# 应用名一样
spring:
  application:
    name: consumer
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka
  instance:
    hostname: localhost
    prefer-ip-address: true
    instance-id: ${eureka.instance.hostname}:${spring.application.name}:${server.port}
```

将`restTemplate`给`springboot`和`ribbon`托管

```java
@Bean
@LoadBalanced // 被ribbon接管
public RestTemplate restTemplate(){
    return new RestTemplate();
}
```

`ConsumerController.java` 测试接口，使用服务名进行调用

```JAVA

@RestController
public class ConsumerController {
    // 自动装配
    @Autowired
    private RestTemplate restTemplate;

    /**
     * ribbon
     * 1.拦截这个请求
     * 2.截取主机名称 serverName
     * 3.借助eureka来做服务发现  List<ServiceInstance> instances = discoveryClient.getInstances(serverName);
     * 4.通过负载均衡算法，拿其中一个服务ip,port
     * 5.reConstructURL 重构地址( http://localhost:8080/hello)
     * 6.发起请求
     * @param serviceName
     * @return
     */
    @GetMapping("testRibbon")
    public String testRibbon(String serviceName){
        // 根据服务名来调用，不需要写端口号，ribbon帮忙负载均衡，默认算法：轮询
        // http://provider/hello
        String object = restTemplate.getForObject("http://" + serviceName + "/hello", String.class);
        return object;
    }
}

```

##### 负载均衡和远程调用

> 访问不同的服务，可以使用不同的算法规则
>
> `shift+shift`查询`IRule`算法接口 ,实现接口可自定义算法

```yml
# 自定义的负载均衡算法
provider: # 提供者的服务名称
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule # 几种算法的全限定类名
```

```JAVA
    /**
     * 提供者算法全局变量，自定义算法
     * @return
     */
    @Bean
    public IRule myRule(){
        return new RandomRule();
    }
```

##### Ribbon默认的配置类：DefaultClientConfigImpl

```YML
ribbon:
  eureka:
    enabled: true   # 开启eureka支持
  eager-load:
    enabled: false   # 加载模式 第一次访问的时候(false)或者第一次启动的时间(true)访问eureka服务列表
  http:   # ribbon使用restTemplate封装了java.net.HttpUrlConnection发送的请求(不支持连接池)
    client:   # 发送请求的工具有很多 httpClient(支持连接池) 添加依赖(使用其它工具)
      enabled: false
  okhttp:   # 也是请求工具，在移动端用的多
    enabled: false
```

总结：

> ILoadBalancer接口：
>
> 从eureka拉取服务列表，使用IRule算法实现客户端调用的负载均衡

#### 声明式(注解)web服务客户端(Openfegin)

> **是完成服务之间调用的组件**(远程调用的组件)http调用
>
> 使用fegin创建一个接口并进行注解(包含：fegin和JAX-RS注解)
>
> 还支持可热拔编码器和解码器。
>
> `fegin`集成了`Ribbon`(Ribbon集成了eureka)

> 微服务思想：**我不会，别人会，我去调用别人**
>
> 前端——》user-sevice(/userOrder)——》RPC(fegin远程过程调用HTTP协议通信)——》order-service(/doOrder)
>
> fegin默认等待时间为1s
>
> 超过1s报错超时

```
Whitelabel Error Page
This application has no explicit mapping for /error, so you are seeing this as a fallback.

Wed Jul 26 23:10:29 CST 2023
There was an unexpected error (type=Internal Server Error, status=500).
```

配置fegin超时时间



##### `order-service`

`pom.xml`

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
```

`application.yml`

```YML
server:
  port: 8080
spring:
  application:
    name: order-service
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka

```

`OrderController.java`

```java
@RestController
public class OrderController {
    @GetMapping("doOrder")
    public String doOrder(){
        return "订单-01麻辣香锅";
    }
}
```

##### `user-service`

> 开启fegin的客户端功能 才可以发送远程调用
>
> @EnablefeginClients

`UserServiceApplication`

```JAVA
@SpringBootApplication
@EnableEurekaClient
@EnablefeginClients
public class UserServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }

}
```

`pom.xml`

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfegin</artifactId>
</dependency>

<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
```

`application.yml`

```yml
server:
  port: 8081
spring:
  application:
    name: user-service
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka
```

`UserController`

```java

@RestController
public class UserController {
    /**
     * 动态代理 jdk(java interface 接口 $Proxy) /cglib(subClass 子类)
     * jdk动态代理 只要是代理对象调用的方法必须走invoke方法(InvocationHandler类里的)
     * java.lang.reflect.InvocationHandler#invoke(java.lang.Object, java.lang.reflect.Method, java.lang.Object[])
     *                       (代理对象,调用方法,调用方法参数)
     *  public Object invoke(Object proxy, Method method, Object[] args)
     */
    @Autowired
    public UserOrderfegin userOrderfegin;

    @GetMapping("userOrder")
    public String userOrder(){
        System.out.println("用户下单");
        // 发起远程调用
        String s = userOrderfegin.doOrder();
        return s;
    }
}

```

`fegin/UserOrderfegin.java`

```java
// User模块调用Order模块的fegin
// value = "order-service" 服务提供者的应用名称
@feginClient(value = "order-service")
public interface UserOrderfegin {
    // 需要调用哪个接口
    /**
     *     写方法签名：
     *     (注解)
     *     @GetMapping("doOrder")
     *     (修饰符 返回值 方法名 方法参数)
     *     public String doOrder();
     * @return
     */
    @GetMapping("doOrder")
    public String doOrder();
    
}
```

##### fegin原理

> jdk动态代理 + ribbon + restTemplate
>
> 使用代理拦截方法，获取方法上的`GetMapping`注解的值(接口)
>
> 获取类上的`feginClient`注解的值(应用名)
>
> 拼接调用地址http://applicationName/api
>
> 使用ribbon远程调用

```JAVA
@SpringBootTest
class UserServiceApplicationTests {
    @Autowired
    private RestTemplate restTemplate;
    @Test
    void contextLoads() {
        UserOrderfegin o = (UserOrderfegin) Proxy.newProxyInstance(UserController.class.getClassLoader(), new Class[]{UserOrderfegin.class}, new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                // 服务发现，拿到ip、port和方法注解的值value
                String name = method.getName();
                // 获取GetMapping注解的值
                GetMapping annotation = method.getAnnotation(GetMapping.class);
                String[] value = annotation.value();
                // 获取第一个值
                String path = value[0];
                // 获取声明类
                Class<?> aClass = method.getDeclaringClass();
                // 获取类的名字
                String name1 = aClass.getName();
                // 获取声明类的feginClient注解
                feginClient annotation1 = aClass.getAnnotation(feginClient.class);
                // 获取feginClient注解的值
                String applicationName = annotation1.value();
                String url = "http://"+applicationName+"/"+path;
                // 发送远程调用(ribbon使用),不需要端口
                String forObject = restTemplate.getForObject(url, String.class);
                return forObject;
            }
        });
        String s = o.doOrder();
        System.out.println(s);
    }

}
```

##### fegin开发重点

> 消费者 和 提供者 的参数列表 一致(传参数) 返回值、方法签名一致
>
> 1. URL传参数(`testUrl/{name}/add/{age}`)，GET请求 参数列表使用 @PathVariable("")
> 2. GET请求(`?value=`)，每个基本参数必须加 @RequestParam("")
> 3. POST请求，传对象、集合等参数，必须加@Requestbody `请求体`或 @RequestParam `表单`

```JAVA
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Order {
    private Integer id;
    private String name;
    private Double price;
    private Date time;
}
```

提供者 不同传参接口

```java
/**
 * URL传参 /doOrder2/麻辣香锅/add/可乐/
 * GET传递一个参数
 * GET传递多个参数
 * POST传递一个对象
 * POST传递一个对象+一个基本参数
 * POST传递两个对象(封装) https://zhuanlan.zhihu.com/p/102389552
 *
 * @return
 */
@GetMapping("testUrl/{name}/add/{age}")
public String testUrl(@PathVariable("name") String name,@PathVariable("age")Integer age){
    System.out.println(name+":"+age);
    return "ok";
}

@GetMapping("oneParam")
public String oneParam(@RequestParam(required = false) String name){
    System.out.println(name);
    return "ok";
}

@GetMapping("twoParam")
public String twoParam(@RequestParam(required = false) String name,@RequestParam(required = false) Integer age){
    System.out.println(name);
    System.out.println(age);
    return "ok";
}

@PostMapping("oneObject")
public String oneObject(@RequestBody Order order){
    System.out.println(order);
    return "ok";
}

@PostMapping("oneObjectAndParam")
public String oneObjectAndParam(@RequestBody Order order,@RequestParam String name){
    System.out.println(name);
    System.out.println(order);
    return "ok";
}
```

消费者 

使用fegin远程调用

```java
// User模块调用Order模块的fegin
// value = "order-service" 服务提供者的应用名称
@feginClient(value = "order-service")
public interface UserOrderfegin {
    // 需要调用哪个接口
    /**
     *     写方法签名：
     *     (注解)
     *     @GetMapping("doOrder")
     *     (修饰符 返回值 方法名 方法参数)
     *     public String doOrder();
     * @return
     */
    @GetMapping("doOrder")
    String doOrder();

    @GetMapping("testUrl/{name}/add/{age}")
    String testUrl(@PathVariable("name") String name, @PathVariable("age")Integer age);
    @GetMapping("oneParam")
    String oneParam(@RequestParam(required = false) String name);

    @GetMapping("twoParam")
    String twoParam(@RequestParam(required = false) String name,@RequestParam(required = false) Integer age);

    @PostMapping("oneObject")
    String oneObject(@RequestBody Order order);

    @PostMapping("oneObjectAndParam")
    String oneObjectAndParam(@RequestBody Order order,@RequestParam String name);
}
```

```java
// 测试接口
@GetMapping("testParam")
public String testParam(){
    String zq = userOrderfegin.testUrl("zq", 20);
    System.out.println(zq);

    String oneParam = userOrderfegin.oneParam("zq1");
    System.out.println(oneParam);

    String twoParam = userOrderfegin.twoParam("zq2", 20);
    System.out.println(twoParam);

    Order order = Order.builder().name("麻辣香锅").price(188D).time(new Date()).id(1).build();
    String s = userOrderfegin.oneObject(order);
    System.out.println(s);

    String oneObjectAndParam = userOrderfegin.oneObjectAndParam(order, "zq");
    System.out.println(oneObjectAndParam);

    return "OK";
}
```

时间传参问题

消费者测试接口

```java
   /**
     * 1.单独传送时间参数，会差10个小时
     * 2.转成字符串即可 或
     * 3.使用jdk的时间类型 LocalDate(年月日) LocalDateTime(时分秒)会丢失秒s
     * 4.改fegin的源码
     * 
     * @return
     */
    @GetMapping("time")
    public String time(){
        Date date = new Date();
        // 年月日
        LocalDate now = LocalDate.now();
        // 时分秒
        LocalDateTime dateTime = LocalDateTime.now();

        System.out.println(date);
        // Thu Jul 27 12:48:32 CST 2023
        String s = userOrderfegin.testTime(date);
        // Fri Jul 28 02:48:32 CST 2023
        return s;
    }
```

`fegin/UserOrderfegin`

```java
 @GetMapping("testTime")
    public String testTime(@RequestParam Date date);
```

`OrderController.java `提供者接口

```java
  // 传时间
    @GetMapping("testTime")
    public String testTime(@RequestParam Date date){
        System.out.println(date);
        return "ok";
    }
```

##### fegin调用状态码

> fegin包的Client接口108行
>
> `HttpStatus.java`(所有状态码)

200 成功

400 请求参数错误

401 没有权限

403 权限不够

404 路径不匹配

405 方法不允许

500 提供者报错了

302 资源重定向

##### fegin日志

> Level(日志等级)
>
> 接口请求时间500毫秒/300毫秒 合理
>
> ```
> public static enum Level {
>         NONE,
>         BASIC,
>         HEADERS,
>         FULL;
> 
>         private Level() {
>         }
>     }
> ```

```JAVA
 /**
     * feign
     * 打印日志信息 级别
     * @return
     */
    @Bean
    public Logger.Level level(){
        return Logger.Level.BASIC;
    }
```

```yml
logging:
  level:
    com.zq.userservice.feign.UserOrderFeign: debug
    # 打印这个接口下面的日志
```



#### 熔断器(Hystrix)

> **服务雪崩**
>
> 核心本质：线程没有及时回收
>
> 熔断：直接return，业务不能完成，但是可以缓解服务器压力
>
> 解决方案：
>
> 1. 调整等待时间，可缓解解压，有局限性(有的服务需要多的时间去执行)
> 2. 在上游服务知道下游服务的状态，下游服务OK，正常发请求；宕机，则直接return，不发请求。
> 3. 设置备选方案
>
> 链式调用：A服务——》B服务——》C服务(挂了)
>
> 1. 用户请求A服务，A服务tomcat分配一个线程支持用户的访问。A服务发现需要完成用户的操作，需要调用B服务
> 2. A服务请求B服务，B服务tomcat分配一个线程支持A服务的访问，B服务发现需要完成用户的操作，需要调用C服务
> 3. B服务调用C服务，C服务宕机了，B并不知道。导致A和B的线程没有回收，此时大量请求进入A服务或者B服务，AB会报503 service unavailable

在分布式的链路中，只要有一个服务宕机，可能导致整个业务线都瘫痪。

> Hystrix(Netflx)
>
> **用来保护微服务不雪崩的方法**。
>
> **能够阻止分布式系统中出现联动故障.**
>
> 通过隔离服务的访问点阻止联动故障，并提供了故障的解决方案，从而提高整个分布式系统的弹性。

```yml
server:
  port: 8080
spring:
  application:
    name: customer-service
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka
  instance:
    hostname: localhost
    instance-id: ${eureka.instance.hostname}:${spring.application.name}:${server.port}
feign:
  hystrix:
    enabled: true   # 在F版以上默认开启
#  circuitbreaker:
#    enabled: true   # cloud2020以上的配置

```

#### `customer-service `消费者

`pom.xml`

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
		
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
```

`feign/CustomerRentFeign.java`

```java
// 指定熔断的类，fallback = 失败回调的类
@FeignClient(value = "rent-car-service",fallback = CustomerRentFeignHystrix.class)
public interface CustomerRentFeign {
    @GetMapping("rent")
    public String rent();
}
```

`feign/hystrix/CustomerRentFeignHystrix.java`

```java
@Component
public class CustomerRentFeignHystrix implements CustomerRentFeign {
    /**
     * 备选方案
     * @return
     */
    @Override
    public String rent() {
        return "备选方案";
    }
}
```

#### `rent-car-service`提供者

`pom.xml`

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

#### 断路器状态机制

> 关：默认情况下关闭，正常调用服务
>
> 开：尝试调用服务，在一个时间窗口(10s)内，多次访问，失败次数达到一个阈值，认为拓机了，启动备选方案
>
> 半开：让少许流量去尝试调用，如果正常了，就关掉断路器
>
> 关——》开——》半开——》关

`hystrix/spare`手写断路器代码




### Spring Cloud实现方案

> SpringCloud 就是微服务理念的一种**具体落地实现方式**

Dubbo+Zookeeper 半自动化的微服务实现架构

SpringCloud Netflix 一站式微服务架构

SpringCloud Alibaba 新的一站式微服务架构
