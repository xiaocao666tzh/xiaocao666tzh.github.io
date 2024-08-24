---
title: Navidrome API 文档
date: 2024-08-24 16:04:00
categories: Technology
tags:
  - 技术
  - Navidrome
  - API
  - 文档
---
# 前言
**本文将保持更新，尽可能将官方没有写到的API收录进来。**
最近再整跟Navidrome有关的项目，据Navidrome的文档说，其API与Subsonic兼容，给的API文档也非常不详细（至少我看的一头雾水），实际测试下来发现许多接口与Subsonic并不相同并且官方文档并未提及，比如创建用户的接口，还是在翻了[issue](https://github.com/navidrome/navidrome/issues/2192)才找到。  
**注：本文主要做备份作用，并非一个完整详细的文档！尤其是错误信息，应该都只包含基本的错误，不一定包含所有错误。**
## 接口
### 获取token
#### 地址
/auth/login
#### 请求方式
post，附加参数以json格式发出
#### 参数
| 名称 | 类型 | 示例 |
| ------------ | ------------ | ------------ |
| username | string | admin |
| password | string | password |
#### 返回值
    {
        "id": "xxx",
        "isAdmin": true(or false),
        "lastFMApiKey": "xxx",
        "name": "xxx",
        "subsonicSalt": "xxx",
        "subsonicToken": "xxx",
        "token": "xxx",
        "username": "xxx"
    }
#### 错误信息
| code | msg | 含义 |
| ------------ | ------------ | ------------ |
| 401 | Invalid username or password | 用户名或密码错误 |
| 422 | invalid request payload | 无效的请求（应该是post过去的信息错了） |