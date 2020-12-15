---
title: Docker-理解容器中的uid和gid
categories:
  - Devops
tags:
  - Devops
  - draft
date: '2020-06-28 09:36:19'
top: false
comments: true
---

# 重要
问题：[安全漏洞 CVE-2019-11245 ](https://nvd.nist.gov/vuln/detail/CVE-2019-11245)
容器中的进程默认以 root 用户权限运行，
Docker默认不启用user namespace, 所有的容器共用一个内核，所以内核控制的 uid 和 gid 则仍然只有一套。
如果容器内使用root用户，则容器内的进程与宿主机的root具有相同权限。会有很大的安全隐患，一旦容器有权限访问宿主机资源，则将具备宿主机root相同权限。

解决方法：

+ 1. 为容器中的进程指定一个具有合适权限的用户，而不要使用默认的 root 用户。
+ 2. 应用 Linux 的 user namespace 技术，配置 docker 开启 user namespace 隔离用户。

# 1. 配置合适的用户
此处只讨论为进程指定合适用户，指定合适用户的方法有以下两种：

+ 在 `Dockerfile` 中指定用户身份
+ 在 `Pod Security Policies` 中指定用户身份

> `Pod Security Policies`中的配置优先级更高，可以覆盖`Dockerfile`中的参数。

## 1.1 pod securityContext 中 user只接受uid
[Issue with mustrunasnonroot implementation PR:56503 #59819](https://github.com/kubernetes/kubernetes/issues/59819)

## 1.2 uid说明

uid: 范围为0~65535（Ubuntu中为65533），0~999留给系统用户，普通用户为1000~65533. 

进程如果不声明uid，启动时以登录用户uid启动进程；进程可以声明任一存在或不存在的uid启动进程。
创建用户时若不指定uid, 默认就是直接从已存在的uid中找到最大的那个加1。

综上，
+ 每个uid不一定有对应的用户
+ 每个用户一定有自己的uid
+ 每个进程必定有uid
+ 进程uid不指定，则与启动命令的用户uid一致
+ 创建用户uid不指定，则每多一个用户，uid会max+1递增。

容器中的进程uid\gid与宿主机的一致



## 1.3 容器uid与宿主机冲突产生问题
因为一个原因：

当pod设置`runAsNoneroot`，容器uid与宿主机uid一样，但username不一致时。会触发报错 
```Error: container has runAsNonRoot and image has non-numeric user (kong), cannot verify user is non-root```
原因待定位，现猜测 pod设置`runAsNoneroot`会获取容器uid,此时的user会变成宿主机的用户名，之后根据宿主机用户名在容器内获取uid时

尽量要避免容器内的uid与宿主机的uid重复。所以建议在指定uid时，使用20000~65533的数值。

# 2. docker 开启`user namespace`隔离用户
[隔离 docker 容器中的用户](https://www.cnblogs.com/sparkdev/p/9614326.html)

# 3. 使用securityContext设置挂卷文件权限
当设置runAsNoneRoot后，往往会带来权限问题。比如有些私有云挂卷后，权限默认给的755权限。此时普通用户没有写权限，导致无法使用。所以需要

# Reference
容器这种uid说明

[理解 docker 容器中的 uid 和 gid](https://www.cnblogs.com/sparkdev/p/9614164.html)

[隔离 docker 容器中的用户](https://www.cnblogs.com/sparkdev/p/9614326.html)

[为pod设置权限和AccessControl](https://medium.com/kubernetes-tutorials/defining-privileges-and-access-control-settings-for-pods-and-containers-in-kubernetes-2cef08fc62b7)

理解 k8s 中 SecurityContext

[k8s文档--为pod配置安全性上下文](https://kubernetes.io/zh/docs/tasks/configure-pod-container/security-context/)

[源码剖析--SecurityContext](https://developer.aliyun.com/article/777651)

为非root用户启动的pod挂卷

[Add ability to mount volume as user other than root](https://github.com/moby/moby/issues/2259)

[Volumes are created in container with root ownership and strict permissions](https://github.com/kubernetes/kubernetes/issues/2630)

[stack overflow k8s 设置挂卷的userGroup和文件权限](https://stackoverflow.com/questions/43544370/kubernetes-how-to-set-volumemount-user-group-and-file-permissions)

