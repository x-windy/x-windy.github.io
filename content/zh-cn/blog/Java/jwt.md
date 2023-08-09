---
title: "JWT"
description: ""
author: "XW"
draft: false
---

# JWT(JSON Web Token)

> [JSON Web Tokens - jwt.io](https://jwt.io/)
>
> JWT是一种**加密后的数据载体**，可在各应用之间进行数据传输
>
> Token(令牌)
>
> **Token的目的是为了减轻服务器的压力，减少频繁的查询数据库，使服务器更加健壮。**

## 什么是 JSON Web Token？

JSON Web Token（JWT） 是一种开放标准 （[RFC 7519](https://tools.ietf.org/html/rfc7519)），它定义了一种紧凑且独立的方式，用于在各方之间以 **JSON 对象**的形式**安全地**传输信息。此信息可以验证和信任，因为它是经过**数字签名**的。JWT 可以使用密钥（使用 **HMAC** 算法）或使用 **RSA** 或 **ECDSA** 的公钥/私钥对进行签名。

> JWT涵盖用户身份信息，每次访问时，服务器校验信息合法性



## 何时应使用 JSON Web Token？

以下是 JSON Web Token有用的一些方案：

- **授权**：这是使用 JWT 的最常见方案。**用户登录**后，每个后续请求都将包含 JWT，允许用户访问使用该令牌允许的路由、服务和资源。单点登录是当今广泛使用 JWT 的一项功能，因为它的开销很小，并且能够跨不同域轻松使用。
- **信息交换**： JSON Web Token是在各方之间安全传输信息的好方法。由于 JWT 可以签名（例如，使用公钥/私钥对），因此您可以确定发件人是他们所说的人。此外，由于签名是使用标头和有效负载计算的，因此您还可以验证内容是否未被篡改。

## JSON Web Token结构

> 一个JWT通常有HEADER(头)，PAYLOAD(有效载荷)和SIGNATURE(签名)三部分组成，之间通过`.`连接

- Header	头(算法和令牌类型)
- Payload   有效载荷(数据)
- Signature 签名

```
Header.Payload.Signature
xxxxx.yyyyy.zzzzz
```

**eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`.`eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ`.`SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c**

### Header 头

JWT的头部包含两部分的信息：

> 1. 令牌的类型（即 `JWT`）
> 2. 使用的加密算法，例如 `HMAC SHA256` (默认`HS256`)或 `RSA`。

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

`alg`：表示签名的算法

`typ`：标识令牌(token)的类型，JWT令牌统一写为`JWT`

使用`Base64`加密后，构成JWT的第一部分`Header`

```

```



### Payload 有效载荷

> 用于存放实际需要传递的有效信息

**标准载荷**：建议但不强制使用，对JWT信息作补充

| 标准载荷             | 介绍                       |
| -------------------- | -------------------------- |
| iss(issuer)          | jwt签发者                  |
| exp(expiration time) | 过期时间，必须大于签发时间 |
| sub(subject)         | 主题，用来做什么           |
| aud(audience)        | 给谁用，接收jwt的一方      |
| nbf(Not Before)      | 生效时间                   |
| jti(JWT ID)          | JWT的唯一身份标识          |

自定义载荷：一般添加用户的相关信息 或 其它业务需要的必要信息，不建议添加**敏感信息**(如用户密码)，该部分可在客户端解密

**受篡改保护，但任何人都可以读取**

```json
{
  "sub": "alluser",
  "iss": "zq"
  "user_info": [
    {
    	"id": "1",
	},
    {
    	"name": "John Doe",
	},
	{
         "admin": true
    }

    ]
}
```

使用Base64加密后，构成JWT第二部分

```

```



### Signature 签名

> 对**前两部分Header、Payload进行签名**，得到一个签名，防止数据篡改
>
> 需要指定一个**密钥(secret)**-256-bit，密钥只有服务器知道。(不能泄露)

使用Header指定的 HMAC SHA256 算法签名：

```java
signature = HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),secret)
```

使用Base64加密后，构成JWT的第三部分



## [JWT Token在线编码生成](https://tooltt.com/jwt-encode/)

### 完整JSON

```
{
  "header": {
    "alg": "HS256",
    "typ": "JWT"
  },
  "payload": {
    "data": [
      {
        "id": "1"
      },
      {
        "name": "zq"
      },
      {
        "age": "20"
      }
    ],
    "iat": 1690988484,
    "exp": 1691078398,
    "aud": "qz",
    "iss": "zq",
    "sub": "all"
  }
}
```

### JWT

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkYXRhIjpbeyJpZCI6IjEifSx7Im5hbWUiOiJ6cSJ9LHsiYWdlIjoiMjAifV0sImlhdCI6MTY5MDk4ODQ4NCwiZXhwIjoxNjkxMDc4Mzk4LCJhdWQiOiJxeiIsImlzcyI6InpxIiwic3ViIjoiYWxsIn0.abOPHo6ytyaCglKfXgiDPQkcFDdM90FpgX20ktvpBf4
```

## JWT特点

### 无状态

JWT不需要再服务端存取任何状态，客户端而已携带JWT访问服务端，从而使服务端变得无状态，这样，服务端就可以轻松地实现**扩展和负载均衡**。(分布式多台服务器)，只需要校验，不需要存储

### 可自定义

JWT的有效载荷部分可以自定义，可以存储任何JSON格式的数据。可实现自定义的功能，如用户喜好、配置信息等等。

### 扩展性强

JWT有一套标准规范，很容易再不同平台和语言之间共享和解析。开发人员还可以根据需要自定义声明(claims)来实现更加灵活的功能。

### 调试性好

JWT内容以Base64编码后的字符串形式存在，非常容易进行调试和分析。

### 安全性取决于密钥管理

注意密钥的管理，包含生成、存储、更新、分发等等。

### 无法撤销

JWT是无状态的，一旦JWT被签发，就无法撤销(除非过期)。如果用户在使用JWT认证期间被注销或禁用，那么服务端就无法阻止该用户继续使用之前签发的JWT。

因此，开发人员需要设计额外的机制来**撤销JWT**，例如使用黑名单、白名单、**加到缓存Redis**(无状态->有状态) 或者 设置 短期有效期等

### 需要缓存到客户端

JWT包含用户信息和授权信息，需要客户端缓存到本地，意味着JWT有被窃取的风险

### 载荷大小由限制

JWT需要传输到客户端，一般不建议超过**1KB**(10个kv对以内)，不然会影响性能



## JWT的优缺点

### 优点

无状态性：不需要存储在服务器上，可以实现无状态的身份验证和授权(分布式)

可扩展性：载荷可自定义，可根据需要添加任意信息

可靠性：使用数字签名来保证安全性

平台性：支持多编程语言和操作系统

高效性：不需要查数据库

### 缺点

安全性取决于密钥管理

无法撤销令牌

需要传输到客户端

载荷大小限制

## JWT应用场景

### 一次性验证

应用场景

> 用户注册成功后，发送一份激活邮箱或者其它业务需要邮箱激活等操作。
>
> 用户注册账户（填写用户名、密码、邮箱）后，会发送一封邮件给用户的邮箱。邮件里面有个链接，用户点击后，邮箱就被验证了。用户如果没有进行验证的话，是不允许一些操作的，如发表文章（防止恶意注册用户）。

原因：

时效性：让该链接具有时效性(2小时内激活)

不可篡改性：防止篡改以激活其它账户

邮箱注册

![[后端开发 - 用户邮箱验证实现思路 - 个人文章 - SegmentFault 思否](https://segmentfault.com/a/1190000014522351)](jwt.assets/bV855x.png)

### RESTful API的无状态认证

做RESTful api的身份凭证：当用户身份校验成功后，客户端每次接口访问都带上JWT，服务端校验JWT合法性(是否篡改/是否过期等)

### 信息交换

项目间/服务间 之间安全传输信息的好方式，使用公钥（Public Key）与私钥（Private Key），可以确定发件人是他们自称的人。

> 公钥（Public Key）与私钥（[Private](https://so.csdn.net/so/search?q=Private&spm=1001.2101.3001.7020) Key）是通过一种算法得到的一个密钥对（即一个公钥和一个私钥），**公钥**是密钥对中**公开的部分**，**私钥**则是**非公开的部分**。公钥通常用于**加密会话密钥、验证数字签名，或加密可以用相应的私钥解密**的数据。
>
> 公钥和私钥是成对出现的，我们会保留有自己的私钥，同时公开自己的公钥。一个很典型的例子是GitHub的使用。我们通常不会使用账号密码来管理自己的项目，而是通过将自己的**公钥**上传到GitHub的里，而自己的电脑里则保留有相对应的**私钥**，从而达到免密码提交代码。
>
> 当然私钥和公钥对是唯一的，而你也可以随时重新生成自己的公钥和私钥密码对，但当你从新生成密钥对并覆盖了就有的密钥时，你之前的公钥就作废了。
>
> 简单来说就是：
> 公钥加密，私钥解密，
> 私钥签名，公钥验证。
>
> [公钥私钥加密和SHA256_sha256密钥_卖鱼的小白菜的博客-CSDN博客](https://blog.csdn.net/u010039377/article/details/79751124)



### JWT令牌登录

> JWT令牌存储在客户端，容易泄露并被伪造身份搞破环
>
> JWT被签发，就无法撤销，当破坏在进行时，后端无法马上禁止

可以通过监控异常JWT访问，设置黑名单+强制下线等方式避免



## JWT实战案例

### JWT工具类

```java
package com.zq.vaccinum.util;

import com.zq.vaccinum.constant.Constants;
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.io.Decoders;
import io.jsonwebtoken.security.Keys;

import java.util.Date;
import java.util.Map;

/**
 * JwtToken工具类
 * @Author: zq
 * @Date: 2023/08/03/0:51
 * @Description:
 */
public class JwtTokenUtil {

    /**
     * 生成token令牌
     * @param claims 数据声明(载荷信息)
     * @return  token
     */
    public static String createToken(Map<String,Object> claims){
        String token = Jwts.builder()
                // 使用默认头
                .setClaims(claims) // 存放自定义载荷
                .setExpiration(getExpirationDate()) // 失效时间
                .signWith(Keys.hmacShaKeyFor(Decoders.BASE64.decode(Constants.JWT_SECRET))) // 签名
                .compact();
        return token;
    }

    /**
     * 解析token中的数据声明(载荷信息)
     * @param token 令牌
     * @return  数据声明
     */
    public static Claims parseToken(String token){
        return Jwts.parserBuilder()
                .setSigningKey(Decoders.BASE64.decode(Constants.JWT_SECRET))    // 签名
                .build()
                .parseClaimsJws(token)  // 接收的token令牌
                .getBody();
    }
    /**
     * 生成Token失效截至时间
     * @return
     */
    private static Date getExpirationDate(){
        // 当前系统时间+失效时间
        return new Date(System.currentTimeMillis()+ Constants.JWT_EXPIRATION);
    }

    /**
     * 从Token获取用户名
     * @param token
     * @return
     */
    public static String getUsernameFromToken(String token){
        // parseToken()解析Token获取claims
        return parseToken(token).getSubject();
    }

    /**
     * 判断token是否失效
     * @param token
     * @return
     */
    public static boolean isTokenExpired(String token){
        // 判断当前时间是否在时效截止时间内(截止时间是否在当前时间之前)
        return getExpirationDate().before(new Date());
    }

    /**
     * 验证token是否有效(是否修改null,是否过期)
     * @param token
     * @return
     */
    public static boolean validateToken(String token){
        return parseToken(token)!=null&&isTokenExpired(token);
    }

    /**
     * 刷新token
     * @param token
     * @return
     */
    public static String refreshToken(String token){
        // 解析token
        Claims claims = parseToken(token);
        // 重置创建时间
        claims.put(Constants.JWT_CREATE_TIME,new Date());
        // 生成新的token
        return createToken(claims);
    }

    /**
     * 从载体信息中根据key拿值
     * @param claims  载体信息
     * @param key  键
     * @return  值
     */
    public static String getValueByKey(Claims claims,String key){
        return claims.get(key)!=null?claims.get(key).toString():null;
    }

}

```

### 邮件激活

1. /regist注册接口，请求成功后，发送激活链接，链接参数为jwt

1. /active?jwt=激活，接收参数为jwt

[JSON Web Token Libraries - jwt.io](https://jwt.io/libraries?language=Java)

### 令牌登录

1. /login登录成功后，返回JWT给客户端
2. 设计**登录检查拦截器**，当访问**其它接口资源**时进行登录拦截

## JWT指南

### Redis校验实现令牌泄露保护

在**Redis缓存一份**，判定泄露后(每次进行)，移除Redis中的JWT，当接口再次被发起请求时，强制用户重新进行身份验证

### 临检JWT限制敏感操

> 一些涉及敏感数据变动时，执行临检操作(不用Redis)。

在涉及到：**新增、修改、删除、上传、下载**等敏感操作时(支付)，强制检查用户身份，如**手机验证码、扫描二维码**等手段，确认操作者是用户本人

### 异常JWT监控：超频识别与限制

> 当JWT令牌被盗取，一般会出现**高频次的系统访问**。

监控用户端在单位时间内的请求次数，当单位时间内的请求**超出预定阈值**时，则判定该用户的JWT令牌异常。(限制访问、冻结、IP限制、封号)

### 地域检查杜绝JWT泄露可能

> 一般用户活动范围是固定的，意味着JWT客户端访问IP相对固定，JWT泄露后，可能会出现异地登录的情况。

对JWT进行异地访问检查，有效时间内，IP频繁变动可判断JWT泄露。(账号异地登录，邮箱，手机提醒)

### 客户端区分检查防止JWT泄露

> 对于APP产品来说，一般客户端是固定的，基本为移动设备(APP，平板)，可以结合设备机器码进行绑定。

将**JWT与机器码绑定**，存储于服务端，当客户端发起请求时，通过检查客户端的机器码于服务端机器码是否匹配判断JWT是否泄露。(判断JWT可能被盗，机器码不匹配，重新登录)

### JWT令牌保护：限时、限数、限频

> 进行泄露识别，做好泄露后补救保证系统安全

对客户端进行合理限制，限制每个客户端的JWT令牌数量、访问频率、JWT令牌时效等，以降低JWT令牌泄露的风险。

## 面试题

> 什么是JWT？解释一下它的结构

JWT是一种**开放标准**，用于在网络上安全地传输信息。它由三部分组成：**头部、载荷和签名**。

头部包含令牌的元数据，载荷包含实际的信息(用户ID、角色等)，签名用于验证令牌是否被篡改

> JWT的优点是什么? 它与传统的session-based身份验证相比有什么优缺点?

JWT的优点包括**无状态、可扩展、跨语言、易于实现和良好的安全性**。相比之下，传统的`session-based`身份验证需要在**服务端维护会话状态**，使得服务端的**负载更高**，并且不**适用于分布式系统**。

> 在WT的结构中，分别有哪些部分? 每个部分的作用是什么?

JWT的结构由三部分组成:头部、载荷和签名。头部包合令牌类型和算法，载荷包含实际的信息，签名由头部、载荷和密钥生成。

> JWT如何工作? 从开始到验证过程的完整流程是怎样的?

JWT的工作流程分为三个步骤:**生成令牌、发送令牌、验证令牌。**

在生成令牌时，服务端使用密钥对头部和载荷进行**签名**。

在发送令牌时，将令牌**发送**给客户端。

在验证令牌时，客户端从令牌中解析出**头部和载荷**，并使用**相同的密钥验证签名**。

> 什么是JWT的签名? 为什么需要对]WT进行签名? 如何验证JWT的签名?

JWT的签名是由**头部、载荷和密钥**生成的，用于验证令牌**是否被算改**。

签名使用**HMAC算法**或**RSA算法**生成。

在验证JWT的签名时，客户端使用相同的**密钥和算法**生成**签名**，并将生成的签名与令牌中的签名**进行比较**。

> 什么是IWT的令牌刷新? 为什么需要这个功能?

令牌刷新是一种**机制**，用于解决**JWT过期**后需要重新登录的问题。在令牌刷新中，服务端生成新的]WT，并将其发送给客户端。客户端使用新的WT替换旧的JWT，从而延长令牌的有效期。

> JWT是否加密?如果是，加密的部分是哪些? 如果不是，那么它如何保证数据安全性?

JWT本身并不加密，但可以在载荷中包含敏感信息。为了保护这些信息，可以使用`]WE (SON Web Encryption)`对载荷进行加密。如果不加密，则需要在生成JWT时确保**不在载荷中包含敏感信息**。

> 在JWT中，如何处理Token过期的问题? 有哪些方法可以处理?

JWT过期后，客户端需要重新获取新的WT。可以通过在]WT中包含**过期时间**或使用`refresh token`等机制来解决过期问题。

> JWT和OAuth2有什么关系? 它们之间有什么区别?

`JWT`和`OAuth2`都是用于**身份验证和授权**的**开放标准**。JWT是一种**身份验证机制**，而OAuth2是一种**授权机制**。JWT用于在**不同的系统中安全地传输信息**，OAuth2用于**授权第三方应用程序访问受保护的资源**。

> JWT在什么场景下使用较为合适? 它的局限性是什么?

JWT在**单体应用或微服务架构**中的使用比较合适。它的局限性包括**无法撤销、令牌较大、无法处理并发**等问题。

在需要针对每次请求进行**访问控制**或**需要撤销令牌**的情况下，JWT可能**不是最佳选择**。

场景应用：**(邮箱激活、令牌登录)**









