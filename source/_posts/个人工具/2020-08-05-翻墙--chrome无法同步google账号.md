---
title: 翻墙--chrome无法同步Google账号
categories:
  - 个人工具
tags:
  - 个人工具
  - v2ray
  - chrome
date: '2020-08-05 05:53:29'
top: false
comments: true
---

# 重要
最重要的事: 

1. Qv2ray开代理
本人使用Qv2ray
+ 设置系统代理
+ 勾选SOCKS设置，并填写端口`1088`，UDP本地IP`127.0.0.1`。

2. 设置docker代理配置
```bash
sudo mkdir -p /etc/systemd/system/docker.service.d/
sudo vim /etc/systemd/system/docker.service.d/http-proxy.conf
```
将以下内容填入文件`http-proxy.conf`
```conf
[Service]
Environment="ALL_PROXY=socks5://127.0.0.1:1088"
NO_PROXY=localhost,127.0.0.1,reg.chebai.org,hub.icos.city,icosdop.service.rd,icos.city
```

3. 重启docker服务
```bash
root@:~# systemctl daemon-reload
root@:~# systemctl restart docker
```

4. 查看配置
```bash
systemctl show --property=Environment docker
Environment=ALL_PROXY=socks5://127.0.0.1:1080 NO_PROXY=localhost,127.0.0.1,reg.chebai.org,hub.icos.city,icosdop.service.rd,icos.city
```

5. docker pull 谷歌仓库镜像
```bash
root@:~# docker pull gcr.io/google_containers/pause-amd64:3.0
3.0: Pulling from google_containers/pause-amd64
a3ed95caeb02: Pull complete 
f11233434377: Pull complete 
Digest: sha256:163ac025575b775d1c0f9bf0bdd0f086883171eb475b5068e7defa4ca9e76516
Status: Downloaded newer image for gcr.io/google_containers/pause-amd64:3.0
root@:~# docker images
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
stephenlu/pause-amd64                  3.0                 78ba6fae6829        3 weeks ago         747 kB
gcr.io/google_containers/pause-amd64   3.0                 99e59f495ffa        20 months ago       747 kB
```

6. minikube启动k8s集群
```bash
minikube start 
```

## 使用国内proxy启动
```bash
minikube start --registry-mirror=https://registry.docker-cn.com
# 或
minikube start --vm-driver=none --registry-mirror=https://registry.docker-cn.com --image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers
```

# Reference

[docker使用代理pull gcr仓库镜像](https://blog.csdn.net/StephenLu0422/article/details/78924694)
[docker官方文档设置HTTP/HTTPS Proxy](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)

# 遗留问题
docker 配置 no_proxy 无法使用 通配模式。


