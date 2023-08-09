---
title: "Redis"
description: ""
author: "XW"
draft: false
---

# Redis

> [Redis 教程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/redis/redis-tutorial.html)

## 简介

> *REmote DIctionary Server(Redis) 是一个由 Salvatore Sanfilippo 写的 **key-value** 存储系统，是跨平台的**非关系型数据库**。*
>
> Redis 是一个开源的使用 ANSI **C 语言**编写、遵守 BSD 协议、支持网络、可**基于内存**、分布式、可选持久性的**键值对(Key-Value)存储数据库**，并提供多种语言的 API。
>
> Redis 通常被称为**数据结构服务器**，因为值（value）可以是字符串(String)、哈希(Hash)、列表(list)、集合(sets)和有序集合(sorted sets)等类型。

基本数据结构：

- String: 字符串

- Hash: 哈希

  key:{

  ​	key:value

  }

- List: 列表

- Set: 集合

- Sorted Set: 有序集合



Redis 与其他 key - value **缓存产品**有以下三个**特点**：

- Redis支持**数据的持久化**，可以将内存中的数据保存在**磁盘**中，重启的时候可以再次加载进行使用。
- Redis不仅仅支持**简单的key-value类型**的数据，同时还提供`list，set，zset，hash`等数据结构的存储。
- Redis支持**数据的备份**，即`master-slave`模式的数据备份。



## Redis 优势

- 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
- 丰富的数据类型 – Redis支持二进制案例的 `Strings, Lists, Hashes, Sets 及 Ordered Sets` 数据类型操作。
- **原子** – Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。
- 丰富的特性 – Redis还支持 **publish/subscribe, 通知, key 过期**等等特性。

## Redis安装

> **下载地址：**[Releases · tporadowski/redis (github.com)](https://github.com/tporadowski/redis/releases)

1. 下载解压
2. 配置环境变量
3. cmd命令行下：

```shell
# 启用默认配置文件
redis-server.exe
# 指定配置文件
redis-server.exe redis.windows.conf
```

切换到 redis 目录下运行:

```
# 连接数据库
redis-cli.exe -h 127.0.0.1 -p 6379
```

设置键值对:

```
set myKey myValue
```

取出键值对:

```
get myKey
```

## Redis配置

> 配置文件名为 **`redis.conf`**(Windows 名为 **`redis.windows.conf`**)

### 查看或设置配置项

```shell
# CONFIG命令格式
redis 127.0.0.1:6379> CONFIG GET [CONFIG_SETTING_NAME]
# 获取所有配置项
redis 127.0.0.1:6379> CONFIG GET *
```

### 编辑配置项

> 修改配置文件 **`redis.conf`**(Windows 名为 **`redis.windows.conf`**)

使用**CONFIG set** 命令修改配置:

```SHELL
# CONFIG SET命令格式
redis 127.0.0.1:6379> CONFIG SET [CONFIG_SETTING_NAME] [NEW_CONFIG_VALUE]

redis 127.0.0.1:6379> CONFIG SET loglevel "notice"
```

### 配置文件参数说明

`redis.conf` 配置项说明如下：

| 序号 | 配置项                                                       | 说明                                                         |
| :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 1    | `daemonize no`                                               | Redis **默认**不是以守护进程的方式运行，可以通过该配置项修改，使用 `yes` **启用守护进程**（Windows **不支持**守护线程的配置为 no ） |
| 2    | `pidfile /var/run/redis.pid`                                 | 当 Redis 以守护进程方式运行时，Redis **默认**会把 `pid` 写入 `/var/run/redis.pid` 文件，可以通过 pidfile 指定 |
| 3    | `port 6379`                                                  | 指定 Redis **监听端口**，默认端口为 `6379`，作者在自己的一篇博文中解释了为什么选用 6379 作为默认端口，因为 6379 在手机按键上 MERZ 对应的号码，而 MERZ 取自意大利歌女 Alessia Merz 的名字 |
| 4    | `bind 127.0.0.1`                                             | 绑定的**主机地址**                                           |
| 5    | `timeout 300`                                                | 当客户端**闲置**多长秒后**关闭连接**，如果指定为 0 ，表示关闭该功能 |
| 6    | `loglevel notice`                                            | 指定**日志记录级别**，Redis 总共支持四个级别：`debug`、`verbose`、`notice`、`warning`，**默认**为 `notice` |
| 7    | `logfile stdout`                                             | **日志记录方式**，**默认**为标准输出，如果配置 Redis 为**守护进程方式运行**，而这里**又配置**为日志记录方式为**标准输出**，则日志将会发送给 `/dev/null` |
| 8    | `databases 16`                                               | 设置**数据库的数量**，默认数据库为0，可以使用SELECT 命令在连接上指定数据库`id` |
| 9    | `save <seconds> <changes>`<br />Redis 默认配置文件中提供了三个条件：<br />**save 900 1**<br />**save 300 10**<br />**save 60 10000**<br />分别表示 900 秒（15 分钟）内有 1 个更改，300 秒（5 分钟）内有 10 个更改以及 60 秒内有 10000 个更改。 | 指定在多长时间内，有多少次更新操作，就将**数据同步**到数据文件，可以多个条件配合 |
| 10   | `rdbcompression yes`                                         | 指定存储至本地数据库时**是否压缩数据**，默认为 yes，Redis 采用 `LZF` 压缩，**如果为了节省 CPU 时间，可以关闭该选项，但会导致数据库文件变的巨大** |
| 11   | `dbfilename dump.rdb`                                        | 指定**本地数据库文件名**，默认值为 `dump.rdb`                |
| 12   | `dir ./`                                                     | 指定本地数据库**存放目录**                                   |
| 13   | `slaveof <masterip> <masterport>`                            | 设置当本机为 `slave` 服务时，设置 `master` 服务的 IP 地址及端口，在 Redis 启动时，它会自动从 `master` 进行数据同步 |
| 14   | `masterauth <master-password>`                               | 当 master 服务设置了密码保护时，**slave 服务连接 master 的密码** |
| 15   | `requirepass foobared`                                       | 设置 Redis 连接密码，如果配置了连接密码，客户端在连接 Redis 时需要通过 AUTH <password> 命令提供密码，默认关闭 |
| 16   | ` maxclients 128`                                            | 设置同一时间最大客户端连接数，默认无限制，Redis 可以同时打开的客户端连接数为 Redis 进程可以打开的最大文件描述符数，如果设置 maxclients 0，表示不作限制。当客户端连接数到达限制时，Redis 会关闭新的连接并向客户端返回 max number of clients reached 错误信息 |
| 17   | `maxmemory <bytes>`                                          | 指定 Redis 最大内存限制，Redis 在启动时会把数据加载到内存中，达到最大内存后，Redis 会先尝试清除已到期或即将到期的 Key，当此方法处理 后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis 新的 vm 机制，会把 Key 存放内存，Value 会存放在 swap 区 |
| 18   | `appendonly no`                                              | 指定是否在每次更新操作后进行日志记录，Redis 在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis 本身同步数据文件是按上面 save 条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为 no |
| 19   | `appendfilename appendonly.aof`                              | 指定更新日志文件名，默认为 appendonly.aof                    |
| 20   | `appendfsync everysec`                                       | 指定更新日志条件，共有 3 个可选值：**no**：表示等操作系统进行数据缓存同步到磁盘（快）**always**：表示每次更新操作后手动调用 fsync() 将数据写到磁盘（慢，安全）**everysec**：表示每秒同步一次（折中，默认值） |
| 21   | `vm-enabled no`                                              | 指定是否启用虚拟内存机制，默认值为 no，简单的介绍一下，VM 机制将数据分页存放，由 Redis 将访问量较少的页即冷数据 swap 到磁盘上，访问多的页面由磁盘自动换出到内存中（在后面的文章我会仔细分析 Redis 的 VM 机制） |
| 22   | `vm-swap-file /tmp/redis.swap`                               | 虚拟内存文件路径，默认值为 /tmp/redis.swap，不可多个 Redis 实例共享 |
| 23   | `vm-max-memory 0`                                            | 将所有大于 vm-max-memory 的数据存入虚拟内存，无论 vm-max-memory 设置多小，所有索引数据都是内存存储的(Redis 的索引数据 就是 keys)，也就是说，当 vm-max-memory 设置为 0 的时候，其实是所有 value 都存在于磁盘。默认值为 0 |
| 24   | `vm-page-size 32`                                            | Redis swap 文件分成了很多的 page，一个对象可以保存在多个 page 上面，但一个 page 上不能被多个对象共享，vm-page-size 是要根据存储的 数据大小来设定的，作者建议如果存储很多小对象，page 大小最好设置为 32 或者 64bytes；如果存储很大大对象，则可以使用更大的 page，如果不确定，就使用默认值 |
| 25   | `vm-pages 134217728`                                         | 设置 swap 文件中的 page 数量，由于页表（一种表示页面空闲或使用的 bitmap）是在放在内存中的，，在磁盘上每 8 个 pages 将消耗 1byte 的内存。 |
| 26   | `vm-max-threads 4`                                           | 设置访问swap文件的线程数,最好不要超过机器的核数,如果设置为0,那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟。默认值为4 |
| 27   | `glueoutputbuf yes`                                          | 设置在向客户端应答时，是否把较小的包合并为一个包发送，默认为开启 |
| 28   | `hash-max-zipmap-entries 64 hash-max-zipmap-value 512`       | 指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法 |
| 29   | `activerehashing yes`                                        | 指定是否激活重置哈希，默认为开启（后面在介绍 Redis 的哈希算法时具体介绍） |
| 30   | `include /path/to/local.conf`                                | 指定包含其它的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件 |

## Redis 数据类型

> Redis支持五种数据类型：`string`（字符串），`hash`（哈希），`list`（列表），`set`（集合）及`zset`(`sorted set`：有序集合)。

```java
 		// 字符串、对象
        redisTemplate.opsForValue().set("KEY","VALUE");
        String value = redisTemplate.opsForValue().get("KEY");


        // 列表，后进先出
        ListOperations<String,String> listOperations = redisTemplate.opsForList();
        // 从左边加
        listOperations.leftPush("KEY","VALUE");
        // 从右边加
        listOperations.rightPush("KEY","VALUE2");
        // 取key为"KEY"的值，第0-1的值
		// 0,-1	 取全部
        List<String> stringList = listOperations.range("KEY", 0, 1);
        // 从左边取
        String leftPop = listOperations.leftPop("KEY");

		
		// 集合
		SetOperations<String,String> setOperations = redisTemplate.opsForSet();
        // value数据唯一
        setOperations.add("KEY","VALUE");
        setOperations.add("KEY","VALUE2");
        Set<String> stringSet = setOperations.members("KEY");


        // 有序集合
        ZSetOperations<String,String> zSetOperations = redisTemplate.opsForZSet();
        // 有序
        zSetOperations.add("KEY","VALUE",1);
        zSetOperations.add("KEY","VALUE2",2);
        zSetOperations.add("KEY","VALUE3",3);
        // 取第0-3
        Set<String> stringSet = zSetOperations.range("KEY", 0, 3);


        // 哈希 （key hashKey value）
		// （hashMap的key，hashMap存的key，hashMa存的value）
        HashOperations<String,String,String> hashOperations = redisTemplate.opsForHash();
        hashOperations.put("KEY","HashKey","VALUE");
		// 先用hashMap的key，拿到hashMap，再用hashMa存的key拿到value
        String value = hashOperations.get("KEY","HashKey");

```

## SpringBoot使用Redis

### 导入依赖

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
        </dependency>
```

### 实体类

> 实现序列化接口

### `application.yml`

```yml
spring: 
    redis:
      host: localhost
      port: 6379
      password:
      timeout: 10s
      database: 0	# 第0个数据库
```

```java
@Autowired
private RedisTemplate redisTemplate;
```

```java
 // 将key和验证码存入redis
redisTemplate.opsForValue().set(verifyKey,captcha);
// 从Redis拿到正确的验证码
String captcha = (String) redisTemplate.opsForValue().get(verifyKey);
```

### 乱码解决

> Key:
>
> \xAC\xED\x00\x05t\x00.captcha_codes:ab6ccfafd05d4084b0526e20aceb542a
>
> value:
>
> ��

原因：

> **spring-data-redis** 的 **RedisTemplate<K, V>模板类** 在操作redis时默认使用JdkSerializationRedisSerializer 来进行[序列化](https://so.csdn.net/so/search?q=序列化&spm=1001.2101.3001.7020)。
>
> spring操作redis是在jedis客户端基础上进行的，而jedis客户端与redis交互的时候协议中定义是用byte类型交互
>
> spring-data-redis中RedisTemplate<K, V>在操作的时候k，v是泛型对象，而不是byte[]类型的
>
> spring会默认采用defaultSerializer = new JdkSerializationRedisSerializer()
>
> JdkSerializationRedisSerializer它使用的编码是ISO-8859-1

解决：

1. 规定redisTemplate的类型(不行)

```java
@Autowired 
private RedisTemplate<String,String> redisTemplate
```

2. 添加 redis 配置类，配置使用的序列化方式

```java
@Configuration
public class RedisConfig {
 
    @Bean(name = "redisTemplate")
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<Object, Object> template = new RedisTemplate<>();
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
 
        template.setConnectionFactory(factory);
        //key序列化方式
        template.setKeySerializer(redisSerializer);
        //value序列化
        template.setValueSerializer(redisSerializer);
        //value hashmap序列化
        template.setHashValueSerializer(redisSerializer);
        //key haspmap序列化
        template.setHashKeySerializer(redisSerializer);
        
        return template;
    }
}
```

```
@Configuration
public class RedisConfig {
    @Autowired
    private RedisTemplate redisTemplate;

    @Bean
    public RedisTemplate redisTemplateInit() {
        //设置序列化Key的实例化对象
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        //设置序列化Value的实例化对象
        redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        return redisTemplate;
    }
}
```

3. 使用 StringRedisTemplate 而不是使用 RedisTemplate

### Redis配置类

```java
package com.zq.vaccinum.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

/**
 * @Author: zq
 * @Date: 2023/08/01/21:00
 * @Description:
 */
@Configuration
public class RedisConfig {
    @Autowired
    private RedisTemplate redisTemplate;

    /*
    * 乱码处理
    * */
    @Bean
    public RedisTemplate redisTemplateInit() {
        //设置序列化Key的实例化对象
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        //设置序列化Value的实例化对象
        redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        return redisTemplate;
    }
}

```

> [Redis之RedisTemplate配置方式(序列和反序列化)（八）_genericfastjsonredisserializer_擦肩而过的博客-CSDN博客](https://blog.csdn.net/zwj1030711290/article/details/127695945)

## bug

```shell
org.springframework.data.redis.serializer.SerializationException: Could not read JSON: Unrecognized field "username" (class com.zq.vaccinum.domain.LoginUser), not marked as ignorable (3 known properties: "token", "user", "authorities"])
```

原因：LoginUser类未定义一些属性，反序列化失败

解决：使用注解

```java
@JsonIgnoreProperties(ignoreUnknown = true)
```

> https://stackoverflow.com/questions/4486787/jackson-with-json-unrecognized-field-not-marked-as-ignorable
