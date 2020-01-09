---
title: ubuntu-install-常用工具最新版
categories:
  - 个人工具
tags:
  - 个人工具
date: '2020-01-09 02:54:11'
top: false
comments: true
---

# ubuntu install docker-compose

```bash
# 1. remove the old version:

## If installed via apt-get
sudo apt-get remove docker-compose
## If installed via curl
sudo rm /usr/local/bin/docker-compose
## If installed via pip
pip uninstall docker-compose

# 2. install latest docker-compose

VERSION=$(curl --silent https://api.github.com/repos/docker/compose/releases/latest | jq .name -r)
DESTINATION=/usr/local/bin/docker-compose
sudo curl -L https://github.com/docker/compose/releases/download/${VERSION}/docker-compose-$(uname -s)-$(uname -m) -o $DESTINATION
sudo chmod 755 $DESTINATION
``` 

# ubuntu install git-client

```bash
sudo apt-add-repository ppa:git-core/ppa
sudo apt-get update
sudo apt-get install git

# if `add-apt-repository` not found
## ubuntu 14.04
sudo apt-get install software-properties-common
## ubuntu 13.10 or earlier
sudo apt-get install python-software-properties
``` 
