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
> #cd janusec-0.9.5   
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
> {  
> &nbsp;&nbsp;&nbsp;&nbsp;"node_role": "master",  
> &nbsp;&nbsp;&nbsp;&nbsp;"master_node": {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"database": {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"host": "127.0.0.1",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"port": "5432",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"user": "`your_postgresql_user`",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"password": "`your_postgresql_password`",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"dbname": "`janusec`"  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}  
> &nbsp;&nbsp;&nbsp;&nbsp;},  
> &nbsp;&nbsp;&nbsp;&nbsp;"slave_node": {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"node_key": "",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"sync_addr": ""  
> &nbsp;&nbsp;&nbsp;&nbsp;}  
> }  

* "node_role": "master"  ( 固定为 `master` )  


##### 从节点(可选)  
通常多个小型应用只使用一个`主节点`就够了。  
当流量增长后，可使用`从节点`扩展，需要配合您自己的GSLB（全球服务器负载均衡）或支持分区解析的DNS来实现（也就是说，不同地区的用户，会连接到不同的网关节点）。  
需要先在统一的Web管理中添加节点，获取对应的`node_key` 信息，然后才能安装从节点。  


> {  
> &nbsp;&nbsp;&nbsp;&nbsp;"node_role": "`slave`",  
> &nbsp;&nbsp;&nbsp;&nbsp;"master_node": {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"admin_http_listen": "",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"admin_https_listen": "",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"database": {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"host": "",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"port": "",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"user": "",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"password": "",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"dbname": ""  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}  
> &nbsp;&nbsp;&nbsp;&nbsp;},  
> &nbsp;&nbsp;&nbsp;&nbsp;"slave_node": {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"node_key": "`produced_by_web_admin_in_master_node`",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"sync_addr": "`http://master_ip/janusec-admin/api`"  
> &nbsp;&nbsp;&nbsp;&nbsp;}  
> }  

* "node_role": "`slave`"  (固定为 `slave`)  
* "node_key": "`produced_by_web_admin_in_master_node`"  (在Web管理界面生成)  
* "sync_addr": "`http://master_ip/janusec-admin/api`"  (需替换为主节点IP地址)   

### Step 4: 启动
> #systemctl start janusec.service  

### Step 5: 测试安装
打开浏览器（比如Chrome）,使用如下地址：

> http://`您的网关IP地址`/janusec-admin/  

这是Janusec应用网关的第一个管理地址（后面可启用安全的管理地址）。  
默认用户名：`admin`   
默认口令：`J@nusec123`   

安全原因，您需要修改默认口令。  

