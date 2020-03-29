---
title: "安装"
keywords: "安装, Janusec应用网关"
description: "安装Janusec应用网关"
date: 2018-05-19T10:11:09+08:00
draft: false
weight: 300
---

# 安装
----

### 需求
| 节点                | 操作系统   | 数据库 |
|---------------------|--------------------------------------------------|----------|
| 主节点 | CentOS/RHEL 7, 或 Debian 9, x86_64, 使用 systemd | PostgreSQL 9.3 / 9.4 / 9.5 / 9.6 / 10  |   
| 从节点  | CentOS/RHEL 7, 或 Debian 9, x86_64, 使用 systemd | 不需要 |   


### Step 1: 下载
> $cd ~  
> $wget `https://www.janusec.com/download/janusec-latest.tar.gz`  
> $tar zxf ./janusec-latest.tar.gz  

### Step 2: 安装
请切换到root用户并运行 install.sh , janusec应用网关将安装在目录： `/usr/local/janusec/ ` 

> $su   
> #cd janusec-0.9.7   
> #./install.sh   

选择 `1. Master Node`, 然后安装程序会:   

* 将所需文件复制到 `/usr/local/janusec/`   
* 将服务配置文件复制到系统服务目录   
* 将Janusec应用网关服务设置为自动启动，但首次安装时不会启动，需要在配置完成后手工启动一次.   

##### 步骤 3: 配置 
PostgreSQL没有包含在发布包中，需要自行准备PostgreSQL数据库、数据库、账号，可参考[运维管理](/cn/operation-management/)中的PostgreSQL安装。   
现在我们假设您已经安装好了PostgreSQL，且数据库已创建，用户名和口令已准备好。  
然后编辑 `/usr/local/janusec/config.json` :

##### 主节点 (第一个节点，必选)

可参考[配置文件](/cn/configuration/)说明，其中：  

> "node_role": "master"  ( 固定为 `master` )   


##### 从节点(可选)  
通常多个小型应用只使用一个`主节点`就够了。  
当流量增长后，可使用`从节点`扩展，需要配合您自己的GSLB（全球服务器负载均衡）或支持分区解析的DNS来实现（也就是说，不同地区的用户，会连接到不同的网关节点）。  

可参考[配置文件](/cn/configuration/)说明，其中:  

> "node_role": "slave"  ( 固定为 `slave` )  

并配置"slave_node"中的内容:  
> "node_key": "从管理后台节点管理中复制过来", 
> "sync_addr": "https://gateway.master_node.com:9443/janusec-admin/api"  

说明：  
* 如果使用了从节点，请为主节点单独申请一个域名(例如gateway.master_node.com，用于从节点获取配置更新，该域名只指向主节点)，并且配置一个应用(Application)，Destination可配置为127.0.0.1:9999 （实际不使用）。  
* 如果"master_node" - "admin" - "listen" 为false，"sync_addr"中请勿包含冒号和端口号，如"https://gateway.master_node.com/janusec-admin/api"   

### Step 4: 启动
> #systemctl start janusec  

### Step 5: 测试安装
打开浏览器（比如Chrome）,使用如下地址：

在config.json中，如果 "master_node" - "admin" - "listen" 为false，则管理入口为： 

> http://`您的网关IP地址`/janusec-admin/ ， 这是Janusec应用网关的第一个管理地址   
> https://`您的任意应用域名`/janusec-admin/ (在配置证书和应用之后可以使用)

如果 "master_node" - "admin" - "listen" 为true，则默认管理入口为： 

> http://`您的网关IP地址:9080`/janusec-admin/ ， 这是Janusec应用网关的第一个管理地址   
> https://`您的任意应用域名:9443`/janusec-admin/ (在配置证书和应用之后可以使用)

默认用户名：`admin`   
默认口令：`J@nusec123`   

安全原因，您需要修改默认口令。  

