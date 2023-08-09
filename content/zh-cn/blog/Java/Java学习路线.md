# Java

1. JavaSE
2. JavaEE
3. SSM
4. Spring Boot
5. Spring Cloud

## JavaSE

> Java标准版(Java Standard Edition)
>
> 使用于桌面应用程序和小型服务器应用程序的Java平台
>
> 是Java技术的核心和基础，包含了Java语言核心库、Java虚拟机（JVM）和Java开发工具包（JDK）

1. 开发环境搭建(JDK安装、集成开发环境IDEA使用)
2. Java基础
3. Java进阶
4. 数据结构和算法

### 面向对象(OOP)

### 反射机制(Reflection)

### 注解(Annotation)

### 异常(Exception)

## JavaEE 

> Java企业版(Java Enterprise Edition)
>
> 使用于企业级应用程序开发的Java平台
>
> JavaEE扩展了JavaSE，包含`Java Servlet、JavaServer Pages（JSP）、Java Messaging Service（JMS）、Java Persistence API（JPA）`等
>
> JavaEE还提供了企业级容器，如**Web容器**等

### Web项目

**Web项目结构：**

`src`：存放Java代码

`web`：存放静态资源(html、css、js)和jsp

`WEB-INF`：不能够被浏览器直接访问

`WEB-INF/lib`：存放jar包

`WEB-INF/web.xml`：web项目的核心配置文件

**三层开发结构：**

`dao/mapper`：**数据访问层**，增删改查，**不涉及业务逻辑，只是达到按某个条件获得指定数据的要求。**向数据库发送SQL语句

`entity：实体类(Java对象)

`service`：处理业务逻辑，**需要先写一个接口(接口声明抽象方法)，然后再去实现(实现类实现具体方法)**

`servlet/controller`：负责请求响应的控制



#### Tomcat

**Tomcat的文件目录：**

`bin`：可执行文件 

`conf`：配置文件

`lib`：tomcat依赖的包

`logs`：日志文件

`temp`：临时文件

`webapps`：存放web项目的地方

`work`：运行时的数据

##### Servlet

##### Jsp

##### EL表达式

##### Jstl标签库

## SSM

> Spring+SpringMVC+MyBatis

## Spring Boot

`src/main/java/`结构

> 存放项目Java源代码

```text
|_annotation：放置项目自定义注解
|_aspect：放置切面代码
|_config：放置配置类
|_constant：放置常量、枚举等定义
   |__consist：存放常量定义
   |__enums：存放枚举定义
|_controller：放置控制器代码
|_filter：放置一些过滤、拦截相关的代码
|_mapper：放置数据访问层代码接口
|_model：放置数据模型代码
   |__entity：放置数据库实体对象定义
   |__dto：存放数据传输对象定义
   |__vo：存放显示层对象定义
|_service：放置具体的业务逻辑代码（接口和实现分离）
   |__intf：存放业务逻辑接口定义
   |__impl：存放业务逻辑实际实现
|_utils：放置工具类和辅助代码
```

`/src/main/resources`结构

> 放静态配置文件和页面静态资源等东西

```text
|_mapper：存放mybatis的XML映射文件（如果是mybatis项目）
|_static：存放网页静态资源，比如下面的js/css/img
   |__js：
   |__css：
   |__img：
   |__font：
   |__等等
|_template：存放网页模板，比如thymeleaf/freemarker模板等
   |__header
   |__sidebar
   |__bottom
   |__XXX.html等等
|_application.yml       基本配置文件
|_application-dev.yml   开发环境配置文件
|_application-test.yml  测试环境配置文件
|_application-prod.yml  生产环境配置文件
```

`DTO/VO/DO`等**数据模型定义**的区分：

- `DO（Data Object）`：与数据库表结构一一对应，通过DAO层向上传输数据源对象。**服务层与DAO层之间数据传输的对象**

- `DTO（Data Transfer Object）`：数据传输对象，Service或Manager向外传输的对象。**控制层与服务层之间数据传输的对象**

- `BO（Business Object）`：业务对象。由Service层输出的封装业务逻辑的对象。

- `AO（Application Object）`：应用对象。在Web层与Service层之间抽象的复用对象模型，极为贴近展示层，复用度不高。

- `VO（View Object）`：显示层对象，通常是Web向模板渲染引擎层传输的对象。

- `Query`：数据查询对象，各层接收上层的查询请求。注意超过2个参数的查询封装，禁止使用Map类来传输。

  

  注意事项：

  1、`Contorller`层参数传递建议不要使用`HashMap`，建议使用**数据模型**定义

  2、`Controller`层里可以做**参数校验、异常抛出**等操作，但建议不要放太多业务逻辑，业务逻辑尽量放到`Service`层代码中去做

  3、`Service`层做实际业务逻辑，可以按照功能模块做好定义和区分，相互可以调用

  4、功能模块`Service`之间引用时，建议不要渗透到`DAO`层（或者`mapper`层），基于`Service`层进行调用和复用比较合理

  5、业务逻辑层`Service`和数据库`DAO`层的操作对象不要混用。`Controller`层的数据对象不要直接渗透到`DAO`层（或者`mapper`层）；同理数据表实体对象`Entity`也不要直接传到`Controller`层进行输出或展示。

## Spring Cloud



## 其它

### JavaME

> Java Micro Edition
>
> 用于嵌入式设备和移动设备上的Java平台
>
> 包括了Java虚拟机和一系列的API，如`Java ME Connected Limited Device Configuration（CLDC）、Mobile Information Device Profile（MIDP）`等。
>
> JavaME支持多种移动设备，如智能手机、平板电脑、数字电视等。

