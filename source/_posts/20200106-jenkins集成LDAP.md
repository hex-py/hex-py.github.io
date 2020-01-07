---
title: jenkins集成LDAP
categories:
  - Devops
tags:
  - Devops
date: '2020-01-06 10:52:28'
top: false
comments: true
---

## 1. ldap端配置用户和组
> 注意， 将用户加入组，一定是在用户条目上右键`View\Edit Group Membership``，选择要加入的组。
#### 1.1 添加用户
```bash

```
#### 1.2 添加组

#### 1.3 将用户加入组


## 2. 配置jenkins-ldap
> 注意: Jenkins一旦集成LDAP认证就无法使用本地认证。因此在保存ldap配置之前多测试下ldap连接，否则配置错误就无法登录jenkins，参考后面，`解决错误配置ldap，导致无法登录问题`。

## 备注
#### 备注1: 解决错误配置ldap，导致无法登录问题
