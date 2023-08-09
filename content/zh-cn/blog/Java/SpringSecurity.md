---
title: "SpringSecurity"
description: ""
author: "XW"
draft: false
---

##  SpringSecurity

### 权限管理

> 权限管理是管理系统中，针对不同用户对不同**资源**的访问权限进行控制的**过程**。
>
> 只有**授权用户**才能访问敏感信息或执行的任务，从而保护系统的安全性和完整性

权限管理通常包含的步骤：

1. 确认用户身份：用户登录，通过用户认证信息来验证用户身份。
2. 为用户分配角色：为每个用户分配一个或多个角色，以确定他们能够访问哪些资源
3. 为用户分配权限：为每个(用户/角色)分配一个或多个权限，以确定他们能够访问哪些资源。RBAC模型中，将权限分配给角色
4. 管理权限：对资源进行权限控制，访问资源的用户必须具备需要的权限才能访问

### 权限管理相关概念

#### 资源

> 可以是软件系统中任意存在的实体，如：访问目录、文件、数据库中的数据、方法等
>
> 在B/S模式中，如：处理请求的方法(Servlet、Controller)、图片、HTML、JS、CSS等

#### 权限(privilege)

> B/S模式中，访问某一个URL的权限，在表中储存方式：

| ID   | 权限         | 资源名称 | URL         |
| ---- | ------------ | -------- | ----------- |
| 1    | 用户管理权限 | 用户查询 | /selectUser |
| 2    | 用户管理权限 | 用户修改 | /updateUser |
| 3    | 用户管理权限 | 用户删除 | /deleteUser |

> 资源通常是同URL进行去控制权限，权限对应的某些资源，一个权限可以对应一个资源，也可以对应多个资源

用户-角色-权限-资源

#### 主体(principal)

> 用户，用户拥有账号、密码、角色、权限等基本信息
>
> 一个用户具备多个权限

#### 认证(authentication)

> 认证是权限管理系统判断用户(主体)身份是否合法的过程
>
> 用户登录过程

常见认证方式：账号密码认证、扫码认证、手机短信验证码认证、指纹识别等

#### 授权(authorization)

> 授权是将访问软件系统资源的**权力**授予**主体**的过程
>
> 授权通过后，用户具备系统中访问特定功能的能力
>
> 通常，**先认证(登录)再授权**，授权一般是为用户授予访问当前资源的权限

用户登录后，在session中保存用户权限、角色等信息。

用户在访问的时候，服务端获取用户的**权限列表(角色)**判断是否具有访问当前资源的权限（授权）。

#### 权限管理

> 权限管理是针对主题、全新啊、资源进行统一管理的方式

用户的权限：系统超级管理员分配 或 升级(达到预期的某种条件)

### 权限管理的实现

#### 认证流程

1. 账号密码
2. 查询用户
3. **用户是否存在**
4. **密码是否正确**
5. 查询用户具备的权限信息
6. 保存用户的登录状态
7. 登录成功

#### 访问控制

8. 用户访问`/userDetail`接口
9. 从请求中获取用户唯一标识
10. 从session中查询用户
11. **用户是否存在**(唯一标识、令牌)
12. **用户是否具备访问当前资源的权限**(权限列表)
13. 放行访问
14. 返回访问的资源

### RBAC模型

#### 传统权限模型

用户和权限为多对多关系，用中间表维护

| 用户ID | 权限ID |
| ------ | ------ |

缺点：操作维护麻烦

#### RBAC模型

> 基于角色的访问控制(Role-Based Access Control)
>
> 是一套成熟的权限模型
>
> 可以使用权限框架整合RBAC模型实现

将权限赋予角色，再将角色赋予用户

让维护权限关系的行为变的更加简单

> 在RBAC模型设计中，根据权限的复杂程度，可分为**RBAC0、RBAC1、RBAC2、RBAC3**四个级别。

RBAC0是基础，也叫RBAC模型，RBAC1、RBAC2、RBAC3都是RBAC0为基础的升级。

传统权限模型：

用户——权限

RBAC权限模型：

用户——角色——权限

##### RBAC0

> 用户n——m角色n——m权限(多对多关系)

**用户表—中间表—角色表—中间表—权限表**

##### RBAC1

> 在角色中引入继承的概念，将角色分成几个等级，每个等级权限不同，从而实现更细粒度的权限管理

用户n——m角色(等级1、等级2、等级3)n——m权限(多对多关系)

部门经理细分：经理、副经理

进行细粒度的拆分控制

##### RBAC2

> 仅是对用户、角色和权限三者增加了一些限制。
>
> 可分为两类：**静态职责分离SSD**(Static Separation of Duty) 和 **动态职责分离DSD**(Dynamic Separation of Duty)。

**静态职责分离SSD：**

- 互斥角色限制：同一个用户在**两个互斥角色**中只能选择**一个**，例如:在为员工分类角色时，**不可以**把**财务**和**出纳**分配给同一个人。
- 基数限制：一个用户拥有的**角色**是**有限的**，一个角色拥有的**权限**也是**有限的**。这很好理解，**根据实际业务决定**，想想给一个用户无限添加角色、权限也未必是合理的。
- 先决条件限制：用户想要获得**更高级的角色**，首先必须拥有**低级角色**，这样做的好处就是，**不需要**为高级角色**重新分配**一遍**低级角色**具备的所有权限，例如: 赋予用户部门经理角色，**预先赋予该用户一线员工、小组长等角色**。

**动态职责分离DSD：**

- 动态的限制用户拥有的角色，比如，一个用户可以拥有多个角色，但**运行时只能激活一个角色**

  例如：曾经我在某机构做**课程研究员**，登录公司的教学管理平台后，左侧菜单显示的是**课程研究员的权限列表**。当我被派往学院授课，于是我就具备了**讲师的角色**，讲师角色拥有的权限与**课程研究员角色**拥有的权限列表是**不一样的。**

  并且我如果以**课程研究员**的角色**去操作讲师角色特有的权限**，是不是也**不合适**?

  于是在项目的右上角有一个**切换角色的选项**，切换成不同的角色，左侧菜单**显示对应的菜单列表**

#####  统一模型RBAC3

> **RBAC3 = RBAC1 + RBAC2**
>
> 既有角色分层，也包含可以曾各种限制

只在系统对权限要求非常复杂时，才考虑使用此权限模型

场景：OA办公自动化系统、生产制造

#### 基于RBAC的延展——用户组

> 直接给用户组分配角色，再吧用户加入用户组。(Linux用户组)
>
> 用户除了拥有自身的权限外，还拥有了所属用户组的所有权限

用户——用户组——角色——权限

### RBAC实现

- 手动编码
- shiro框架
- springSecurity框架

#### RBAC数据模型

<center>用户表user</center>

| 字段名称 | 字段     | 类型         | 备注     |
| -------- | -------- | ------------ | -------- |
| 用户ID   | id       | int          | 主键     |
| 用户名   | username | varchar(20)  | not null |
| 密码     | password | varchar(100) | not null |
| 电子邮箱 | email    | varchar(30)  | not null |

<center>角色表roles</center>

| 字段名称 | 字段 | 类型        | 备注     |
| -------- | ---- | ----------- | -------- |
| 角色ID   | id   | int         | 外键     |
| 角色名称 | name | varchar(20) | not null |

<center>权限表permissions</center>

| 字段名称 | 字段        | 类型        | 备注     |
| -------- | ----------- | ----------- | -------- |
| 权限ID   | id          | int         | 主键     |
| 权限名称 | name        | varchar(20) | not null |
| 权限描述 | description | varchar(20) |          |

<center>角色——权限关联表role_permissions</center>

| 字段名称 | 字段          | 类型 | 备注                |
| -------- | ------------- | ---- | ------------------- |
| 角色ID   | role_id       | int  | 外键role(id)        |
| 权限ID   | permission_id | int  | 外键permissions(id) |

<center>用户——角色关联表user_roles</center>

| 字段名称 | 字段    | 类型 | 备注          |
| -------- | ------- | ---- | ------------- |
| 用户ID   | user_id | int  | 外键user(id)  |
| 角色ID   | role_id | int  | 外键roles(id) |

menu

| 字段名称 | 字段      | 类型    | 备注 |
| -------- | --------- | ------- | ---- |
| 菜单ID   | id        | int     |      |
| 菜单名   | menu_name | varchar |      |

#### 登录应用

1. 用户登录成功后
2. 查询用户角色
3. 查询用户权限
4. 缓存用户数据到`Session`或`Redis`
5. 用户访问资源，发送token
6. 从redis查询token并获取用户数据
7. 判断访问资源是否受限**(需不需要登录后才能访问)**
8. 受限则判断用户是否具备访问权限，具备则**响应资源**，不具备，**拒绝访问**

#### Spring Security3.X

> 是一个功能强大且高度可定制的**身份认证和访问控制框架**，用于Java应用程序提供**身份认证**和**授权**
>
> 它是用于保护基于Spring程序的安全框架

##### 入门

[Spring Security :: Spring Security](https://docs.spring.io/spring-security/reference/)

> 开发环境：**JDK17以上、SpringBoot3.X、SpringSecurity6.X**
>
> 

#### Spring Security2.X

原理：一个过滤器链

监听器》过滤器链》dispatcherservlet(前置过滤器》mapperHandle》后置拦截器》最终拦截器)

> 请求
>
> 1. UsernamePasswordAuthenticationFilter
>
> 2. ExceptionTranslationFilter
>
> 3. FilterSecurityInterceptor
>
> API
>
> 1. FilterSecurityInterceptor
>
> 2. ExceptionTranslationFilter
>
> 3. UsernamePasswordAuthenticationFilter
>
> 响应

`UsernamePasswordAuthenticationFilter`：负责处理我们在登录页面填写了用户名密码后的登录请求。(认证)

`ExceptionTranslationFilter`：处理过滤链抛出的任何`AccessDeniedException`和`AutheenticationException`

`FilterSecurityInterceptor`：负责权限校验的过滤器

##### 认证流程

1. `UsernamePasswordAuthenticationFilter`(重写，登录控制器调用`authenticate`方法)

2. `ProviderManager`(`AuthenticationManager`接口)的`authenticate`方法进行用户认证

3. `DaoAuthenticationProvider` 调用`loadUserByUsername`方法
4. `InMenoryUserDetailsManager`(`UserDetailsService`接口)(`UserDetailsServiceImpl`实现接口重写，从数据库查询)`loadUserByUsername`方法获取用户信息和用户权限



概念：

`Authentication`接口: 它的实现类，表示当前访问系统的用户，封装了用户相关信息。

`AuthenticationManager`接口: 定义了认证`Authentication`的方法

`UserDetailsService`接口: 加载用户特定数据的核心接口。里面定义了一个根据用户名查询用户信息的方法。

`UserDetails`接口: 提供核心用户信息，通过`UserDetailsService`根据用户名获取处理的用户信息要封装成`UserDetails`对象返回。然后将这些信息封装到`Authentication`对象中





Jwt认证过滤器

1. 获取token

2. 解析token

3. 获取`userid`，从`redis`中获取用户信息(key:userid value:用户信息)

4. 封装`Autehntication`对象存入`SecurityContextHolder`

   

其它过滤器

从`SecurityContextHolder`中获取当前请求用户信息



##### 实战

登录

1. 自定义登录接口，调用ProviderManager的方法进行认证(登录)
2. 自定义UserDetailsService，查询数据库

校验

1. 自定义Jwt认证过滤
2. 获取token
3. 解析token获取userid
4. 从redis获取用户信息
5. 存入SecurityContextHolder



重写`UserDetails`实现类

> `UserDetailsService`接口`loadUserByUsername`方法的返回对象
>
> 由`DaoAuthenticationProvider`调用

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class LoginUser implements UserDetails {

    /**
     * 用户ID
     */
    private Long userId;

    /**
     * 用户唯一标识
     */
    private String token;

    /**
     * 登录时间
     */
    private Long loginTime;

    /**
     * 过期时间
     */
    private Long expireTime;

    /**
     * 登录IP地址
     */
    private String ipaddr;

    /**
     * 登录地点
     */
    private String loginLocation;

    /**
     * 浏览器类型
     */
    private String browser;

    /**
     * 操作系统
     */
    private String os;

    /**
     * 权限列表
     */
    private Set<String> permissions;

    /**
     * 用户信息
     */
    private User user;

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return null;
    }

    @Override
    public String getPassword() {
        return user.getPassword();
    }

    @Override
    public String getUsername() {
        return user.getUsername();
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
```

重写`UserDetailsService`实现类

> 原`SpringSecurity`的实现类是从内存中查询用户信息

```java
@Service
public class UserDetailsServiceImpl implements UserDetailsService {
    @Autowired
    private UserMapper userMapper;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(User::getUsername,username);
        // 查询用户信息
        User user = userMapper.selectOne(queryWrapper);
        if (Objects.isNull(user)){
            throw new UsernameNotFoundException("用户名或者密码错误");
        }
        //TODO 查询用户的权限信息

        // 吧数据封装成UserDetails返回
        LoginUser loginUser = new LoginUser();
        loginUser.setUser(user);
        return loginUser;
    }
}

```

##### 配置类

```java

/**
 * SpringSecurity配置类
 * @Author: zq
 * @Date: 2023/08/04/2:14
 * @Description:
 */
@Configuration
@EnableWebSecurity // 添加 security 过滤器
@EnableGlobalMethodSecurity(prePostEnabled = true)	// 启用方法级别的权限认证
public class SpringSecurityConfig {
    /*
    * 用户密码加密工具，注入容器
    * $2a(bcrypt加密版本号)$10(cost的值) 加密方式+ 盐(22位) + 密文
    * $2a$10xxxxx/xxxxx/xxxx
    * */

    @Autowired
    private AuthenticationConfiguration authenticationConfiguration;

    @Autowired
    private JwtAuthFilter jwtAuthFilter;

    // 认证异常处理
    @Autowired
    private AuthenticationEntryPoint authenticationEntryPoint;
    // 授权异常处理
    @Autowired
    private AccessDeniedHandler accessDeniedHandler;

    /**
     * 获取AuthenticationManager（认证管理器），登录时认证使用
     * @return
     * @throws Exception
     */
    @Bean
    public AuthenticationManager getAuthenticationManagerBean() throws Exception {
        return authenticationConfiguration.getAuthenticationManager();
    }

    /**
     * 用户密码加密方式配置
     * @return
     */
    @Bean
    public PasswordEncoder getPasswordEncoderBean(){
        return new BCryptPasswordEncoder();
    }

    /**
     * 配置类
     * @param http
     * @return
     */
    @Bean
    SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http
                // 基于 token，不需要 csrf
                .csrf().disable()
                // 基于 token，不需要 session
                .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                .and()
                // 下面开始设置权限
                .authorizeRequests(authorize -> authorize
                        // 请求,允许匿名访问(登录后不能访问)
                        .antMatchers("/user/login").anonymous()
                        .antMatchers("/user/register").anonymous()
                        .antMatchers("/captcha/*").anonymous()
//                        .antMatchers("/vaccinum-type/list").permitAll()
                        // 其他地址的访问均需验证权限
                        .anyRequest().authenticated()
                )
                // 在 UsernamePasswordAuthenticationFilter.class 过滤器之前
                // 添加 JWT 过滤器，JWT 过滤器在用户名密码认证过滤器之前
                .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class)
//                // 认证用户时用户信息加载配置，注入springAuthUserService
//                .userDetailsService(userDetailsService)
                .exceptionHandling()
                .authenticationEntryPoint(authenticationEntryPoint)
                .accessDeniedHandler(accessDeniedHandler)
                .and()
                .cors()     // 允许跨域访问
                .and()
                .build();
    }
    /**
     * 配置跨源访问(CORS)
     * @return
     */
//    @Bean
//    CorsConfigurationSource corsConfigurationSource() {
//        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
//        source.registerCorsConfiguration("/**", new CorsConfiguration().applyPermitDefaultValues());
//        return source;
//    }
}

```

```java
@Configuration
public class SpringSecurityConfig extends WebSecurityConfigurerAdapter{
    /*
    * 用户密码加密工具，注入容器
    * */
    @Bean
    public PasswordEncoder getPasswordEncoder(){
        return new BCryptPasswordEncoder();
    }
}
```

##### 密码加密

SpringSeurity默认使用PasswordEncoder，要求数据库密码格式：{id}passowrd，根据id判断密码的加密方式。{noop}明文存储

SpringSeurity推荐使用`BCryptPasswordEncoder`加密方式

```
/**
 * SpringSecurity配置类
 * @Author: zq
 * @Date: 2023/08/04/2:14
 * @Description:
 */
@Configuration
@EnableWebSecurity
public class SpringSecurityConfig {
    /*
    * 用户密码工具
    * */
    @Bean
    public PasswordEncoder getPasswordEncoder(){
        return new BCryptPasswordEncoder();
    }
}
```

##### JWT验证拦截器

```
/**
 * 访问验证拦截器
 * @Author: zq
 * @Date: 2023/08/04/2:14
 * @Description:
 */
@Component
public class JwtAuthFilter extends OncePerRequestFilter {

    @Autowired
    private RedisTemplate redisTemplate;

    /*
    * 对用户访问资源的HTTP请求头处理
    * */
    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain) throws IOException, ServletException {
        // 获取请求头的token
        String token = request.getHeader("token");
//        if (!request.getMethod().equals("OPTIONS")) {
//            token = request.getHeader("token");
//            if (!StringUtils.hasText(token)) {
//                token = request.getParameter("token");
//                chain.doFilter(request, response);
//                return;
//            }
//        }
        // token为空,放行=>UsernamePasswordAuthenticationFilter,进行登录验证
        if (!StringUtils.hasText(token)) {
            chain.doFilter(request, response);
            return;
        }


        Claims claims = JwtTokenUtil.parseToken(token);
        // 从token里获取Token Key
        String tokenKey = (String) claims.get(Constants.LOGIN_USER_KEY);
        System.out.println("tokenKey:" + tokenKey);
        // 根据Token Key从redis获取用户信息
        String redisTokenKey = Constants.LOGIN_TOKENS + tokenKey;
        System.out.println("redisTokenKey:" + tokenKey);
        LoginUser loginUser = (LoginUser) redisTemplate.opsForValue().get(redisTokenKey);
//        LoginUser loginUser = redisCache.getCacheObject(redisTokenKey);
        // 如果redis找不到，说明用户未登录
        if (Objects.isNull(loginUser)){
            throw new UnauthenticatedException("用户未登录");
        }
        // 获取权限信息
        Collection<? extends GrantedAuthority> authorities = loginUser.getAuthorities();
        // 用户主体，凭证，是否已验证(默认true)
        UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(loginUser, null, authorities);
        // 授权失败，token为空
        if (authenticationToken == null){
            chain.doFilter(request,response);
            return;
        }
        // 如果由授权，放到权限上下文中
        // 将权限信息封装到Authentication中
        SecurityContextHolder.getContext().setAuthentication(authenticationToken);
        chain.doFilter(request,response);
    }

}
```

##### 登录服务

```java
   try {
			// 调用authenticate方法进行认证
            // 认证主体(用户名)、凭证(密码)
            UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(username,password);
            // AuthenticationManager进行用户认证，该方法会去调用UserDetailsServiceImpl.loadUserByUsername方法
            Authentication authenticate = authenticationManager.authenticate(authenticationToken);
            loginUser = (LoginUser) authenticate.getPrincipal();
            // 认证未通过，会抛出异常
        }catch (Exception e){
            if (e instanceof BadCredentialsException){
                throw new UserPasswordNotMatchException();
            }
        }
```

##### 登录接口

[Spring Security 5.7 最新配置细节（直接就能用），WebSecurityConfigurerAdapter 已废弃_websecurityconfigureradapter作废_江帅帅的博客-CSDN博客](https://blog.csdn.net/qq_41340258/article/details/125138902)

```
// 接口权限设置
@PreAuthorize("hasAuthority('sys:user:list')")
hasAnyAuthority()
hasRole()  // ROLE_ + d
```

##### 异常处理

认证或者授权的过程出现异常会被：`ExceptionTranslationFilter`捕获，在`ExceptionTranslationFilter`会判断是认证失败还是授权失败出现的异常

认证过程中出现的异常：`AuthenticationException`然后调用`AuthenticationEntryPoint`对象的方法去进行异常处理

授权过程中出现的异常：`AccessDeniedException`然后去调用`AccessDeniedHandler`对象的方法去进行异常处理



自定义实现类

实现`AuthenticationEntryPoint`和`AccessDeniedHandler`两个接口

```

/**
 * 认证失败异常处理
 * @Author: zq
 * @Date: 2023/08/08/13:35
 * @Description:
 */
@Component
public class AuthenticationEntryPointImpl implements AuthenticationEntryPoint {
    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
        JsonResult jsonResult = new JsonResult(Constants.UNAUTHORIZED,"用户认证未通过请重新登录");
        String json = new ObjectMapper().writeValueAsString(jsonResult);
        WebUtils.renderString(response,json);
    }
}

```

配置给SpringSecurity



