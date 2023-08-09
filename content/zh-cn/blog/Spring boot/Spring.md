---
title: "SSM"
description: ""
author: "XW"
draft: false
---

# SSM基础知识

> Spring、SpringMVC、Mybatis

## Spring

> Ioc/DI、AOP、ORM
>
> 控制反转/依赖注入、面向切面、对象关系映射
>
> [Spring基础知识汇总 Java开发必看 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/basic-knowledge-summary-of-spring.html)

### Spring优点

- 低侵入式设计，代码的污染极低。
- 独立于各种应用服务器，基于Spring框架的应用，可以真正实现**Write Once，Run Anywhere**的承诺。**(一次编写，随处运行)**
- Spring的**IoC容器** (控制反转 Inversion of Control) 降低了业务对象替换的复杂性，提高了组件之间的**解耦**。**(自动装配)**
- Spring的**AOP（面向切面 Aspect Oriented Programming）**支持允许将一些通用任务如安全、事务、日志等进行集中式管理，从而提供了更好的复用。
- Spring的**ORM** **(对象关系映射（Object Relational Mapping) **和**DAO**提供了与第三方持久层框架的良好整合，并简化了底层的数据库访问。
- Spring的高度开放性，并不强制应用完全依赖于Spring，开发者可自由选用Spring框架的部分或全部。

### Spring框架的组成结构

![SPRING](Spring.assets/673670c9a34075831373b711cb8f21b7.png)

### Spring的核心机制

#### 管理Bean

> 通过**Spring容器**来访问容器中的`Bean`
>
> `ApplicationContext`是**Spring容器**最常用的接口

`ApplicationContext`接口的实现类

- `ClassPathXmlApplicationContext`: 从**类加载路径**下搜索**配置文件**，并根据配置文件来创建**Spring容器**。
- `FileSystemXmlApplicationContext`: 从**文件系统**的相对路径或绝对路径下去搜索**配置文件**，并根据配置文件来创建**Spring容器**。

```java
public class BeanTest{
    public static void main(String args[]) throws Exception{
        ApplicationContext ctx = new ClassPathXmlApplicationContext("beans.xml");
        Person p = ctx.getBean("person", Person.class);
        p.say();
    }
}
```

#### 依赖注入(Dependency Injection)

> Ioc/DI（控制反转/依赖注入）

Spring框架的核心功能有两个：

- Spring容器作为超级大工厂，**负责创建、管理**所有的**Java对象**，这些Java对象被称为`Bean`。
- Spring容器管理容器中**Bean之间的依赖关系**，Spring使用一种被称为"**依赖注入**"的方式来管理Bean之间的依赖关系。

使用**依赖注入**，不仅可以为Bean注入**普通的属性值**，还可以注入**其他Bean**的引用。依赖注入是一种优秀的**解耦方式**，其可以让Bean以配置文件组织在一起，而不是以硬编码的方式耦合在一起。

> **当某个Java对象（调用者）需要调用另一个Java对象（被依赖对象）的方法时**，在传统模式下通常有两种做法：

1. 原始做法: 调用者**主动**创建被依赖对象**(new 对象)**，然后再调用被依赖对象的方法。
2. 简单工厂模式: 调用者先找到被依赖对象的工厂，然后**主动**通过**工厂**去获取被依赖对象**(Builder模式 见Effective Java阅读笔记第2条)**，最后再调用被依赖对象的方法。

> **主动创建**会导致调用者与被依赖对象实现类的**硬编码耦合**，非常不利于项目升级的维护
>
> 使用Spring后:
>
> - 调用者获取被依赖对象的方式由原来的**主动**获取，变成了**被动**接受。**(IoC控制反转)**
>
> - Spring容器负责将**被依赖对象**赋值给**调用者的成员变量**——相当于为调用者注入它依赖的实例。**(DI依赖注入)**

##### 设值注入

> 设值注入是指**IoC容器**通过**成员变量**的`setter`方法来注入被依赖对象(Spring依赖注入大量使用)
>
> JavaBean模式

##### 构造注入

> 利用**构造器**来设置依赖关系的方式，被称为构造注入。
>
> Spring在底层以**反射方式**执行带**指定参数的构造器**，当执行带参数的构造器时，就可利用构造器参数对成员变量执行初始化——这就是构造注入的本质。
>
> 构造器模式

**注意：**
建议采用**设值注入**为主，**构造注入**为辅的注入策略。对于**依赖关系无须变化**的注入，尽量采用构造注入；而其他依赖关系的注入，则考虑采用设值注入。

#### Spring容器中的Bean

> **对于开发者来说**，开发者使用Spring框架主要是做两件事：
>
> ①开发Bean；②配置Bean。
>
> **对于Spring框架来说**，它要做的就是根据**配置文件**来创建**Bean实例**，**并调用Bean实例**的方法完成"依赖注入"——这就是所谓`IoC`的本质。

##### 容器中Bean的作用域

Spring支持如下五种作用域：

1. `singleton`: **单例模式**，在整个Spring IoC容器中，**singleton作用域**的Bean将**只生成一个实例**。（静态）
2. `prototype`: 每次通过容器的`getBean()`方法获取**prototype作用域**的Bean时，都将**产生一个新的Bean实例**。(动态)
3. `request`: **对于一次HTTP请求**，**request作用域**的Bean将**只生成一个实例**，这意味着，在**同一次**HTTP请求**内**，程序每次请求该Bean，得到的总是**同一个实例**。只有在**Web应用**中使用Spring时，该作用域才真正有效。(web应用中生效)
4. `session`：该作用域将 bean 的定义限制为 **HTTP 会话**。 只在web-aware Spring ApplicationContext的上下文中有效。
5. `global session`: 每个**全局的HTTP Session**对应**一个Bean实例**。在典型的情况下，仅在使用portlet context的时候有效，同样**只在Web应用中有效**。

> 如果不指定Bean的作用域，Spring**默认使用singleton作用域**。
>
> prototype作用域的Bean的创建、销毁代价比较大。
>
> 而singleton作用域的Bean实例一旦创建成果，就可以重复使用。**因此，应该尽量避免将Bean设置成prototype作用域。**

