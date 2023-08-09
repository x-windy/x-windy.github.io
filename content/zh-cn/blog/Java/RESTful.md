---
title: "RESTful"
description: ""
author: "XW"
draft: false
---

## RESTful

> 7个HTTP方法：GET/POST/PUT/DELETE/PATCH/HEAD/OPTIONS

### 接口规范

> 增/删/改/查
>
> PSOT/DELETE/PUT/GET
>
> > 安全：会不会出现重读、幻读、脏读（数据对不对、数量对不对）
> >
> > 幂等：在操作成功的前提下，会不会对数据库造成额外不好的影响

`GET` 查询

> 安全且幂等

`POST` 插入

> 不安全且不幂等
>
> 不幂等：可能因网络原因，导致插入多条重复记录，但ID不一样

`PUT` 修改(更新)

> 不安全但幂等
>
> 幂等：多次操作执行后只存在一个有效结果

`DELETE` 删除

> 不安全但幂等

## CURL命令

> Command Line URL viewer

```
# 默认GET请求
curl http://www.baidu.com/
curl -XGET [PATH]
# POST请求
curl -XPOST [PATH]
# 整个请求过程都能看见(请求头、请求体、响应头)
curl -v -XPOST [PATH]
```

##  
