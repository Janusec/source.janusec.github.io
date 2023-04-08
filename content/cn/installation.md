---
title: "安装"
keywords: "安装, JANUSEC应用网关"
description: "安装JANUSEC应用网关"
date: 2018-05-19T10:11:09+08:00
draft: false
weight: 300
---

# 安装
----

### 需求

| 节点      | 操作系统   | 数据库 |
|----------|--------------------------------------------------------------------------------|-----------------------|
| 主节点    | Debian 9/10/11+, CentOS/RHEL 7/8+, 首选Debian 10, x86_64, 使用 systemd和nftables   | PostgreSQL 10/11/12+  |   
| 副本节点  | Debian 9/10/11+, CentOS/RHEL 7/8+, 首选Debian 10, x86_64, 使用 systemd和nftables   | 不需要 |  



## 准备主机防火墙nftables  
----
nftables用于拦截CC攻击，减轻应用网关压力。  

Debian 10安装nftables：  

> apt install nftables   

CentOS 7默认没有安装nftables，需要手工安装并启动：  

> #yum -y install nftables  
> #systemctl enable nftables  
> #systemctl start nftables  

CentOS 8已内置nftables，并作为firewalld的后端，只需要手工启动firewalld：  

> #systemctl enable firewalld  
> #systemctl start firewalld  

现在，可以通过如下指令查看规则：  

> #nft list ruleset  

如果规则不是空的，可能会影响防火墙策略的生效。假定现在nftables的规则是空的，然后继续。  


### 步骤1: 下载
> $cd ~  
> $wget `https://www.janusec.com/download/janusec-1.4.0-amd64.tar.gz`  
> $tar zxf ./janusec-1.4.0-amd64.tar.gz  

### 步骤2: 安装
请切换到root用户并运行 install.sh , JANUSEC应用网关将安装在目录： `/usr/local/janusec/ ` 

> $su   
> #cd janusec-1.4.x-amd64   
> #./install.sh   

选择 `1. Primary Node`, 然后安装程序会:   

* 将所需文件复制到 `/usr/local/janusec/`   
* 将服务配置文件复制到系统服务目录   
* 将JANUSEC应用网关服务设置为自动启动，但首次安装时不会启动，需要在配置完成后手工启动一次.   

### 步骤3: 配置 
PostgreSQL没有包含在发布包中，需要自行准备PostgreSQL数据库、数据库、账号，可参考[运维管理](/cn/operation-management/)中的PostgreSQL安装。   
现在我们假设您已经安装好了PostgreSQL，且数据库已创建，用户名和口令已准备好。  
然后编辑 `/usr/local/janusec/config.json` :

##### 主节点 (第一个节点，必选)

可参考[配置文件](/cn/configuration/)说明，其中：  

> "node_role": "primary"  ( 固定为 `primary` )   


##### 副本节点(可选)  
通常多个小型应用只使用一个`主节点`就够了。  
当流量增长后，可使用`副本节点`扩展，需要配合您自己的GSLB（全球服务器负载均衡）或支持分区解析的DNS来实现（也就是说，不同地区的用户，会连接到不同的网关节点）。  

可参考[配置文件](/cn/configuration/)说明，其中:  

> "node_role": "replica"  ( 固定为 `replica` )  

并配置"replica_node"中的内容:  

> "node_key": "从管理后台节点管理中复制过来",   
> "sync_addr": "https://gateway.primary_node.com:9443/janusec-admin/api"  

说明：  
* 如果使用了副本节点，请为主节点单独申请一个域名(例如gateway.primary_node.com，用于副本节点获取配置更新，该域名只指向主节点)，并且配置一个应用(Application)，Destination可配置为127.0.0.1:9999 （实际不使用）。  
* 如果"primary_node" - "admin" - "listen" 为false，"sync_addr"中请勿包含冒号和端口号，如"https://gateway.primary_node.com/janusec-admin/api"   

### 步骤4: 启动
> #systemctl start janusec  

### 步骤5: 测试安装
打开浏览器（比如Chrome）,使用如下地址：

在config.json中，如果 "primary_node" - "admin" - "listen" 为false，则管理入口为： 

> http://`您的网关IP地址`/janusec-admin/ ， 这是JANUSEC应用网关的第一个管理地址   
> https://`您的任意应用域名`/janusec-admin/ (在配置证书和应用之后可以使用)

如果 "primary_node" - "admin" - "listen" 为true，则默认管理入口为： 

> http://`您的网关IP地址:9080`/janusec-admin/ ， 这是JANUSEC应用网关的第一个管理地址   
> https://`您的任意应用域名:9443`/janusec-admin/ (在配置证书和应用之后可以使用)

默认用户名：`admin`   
默认口令：`J@nusec123`   

安全原因，您需要修改默认口令。  

