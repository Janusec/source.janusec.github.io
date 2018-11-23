---
title: "运维管理"
keywords: "Janusec, Application Gateway, WAF, Web Application Firewall, Web应用防火墙"
description: "Janusec应用网关运维管理"
date: 2018-05-25T21:04:25+08:00
draft: false
weight: 600
---

# 运维管理  
----  

#### 部署架构
| 架构         | 主节点       | 从节点     | 描述 |
|--------------|:-----------:|:----------:|-------------|
| 单节点       | 一个         | 无         | 小规模Web应用，统一Web管理  |
| 可扩展       | 一个         | 任意       | 大规模Web应用，统一Web管理  |
| 多个自治系统  | 任意        |  任意      |  部署多套，每个自治系统内部统一Web管理.  |   


#### 管理账号  
统一的Web管理地址包括如下：

> http://`your_master_node_ip_address`:9080/    (首次使用)    
> https://`your_master_node_domain_name`:9443/  (证书配置后可用)     
> https://`your_janusec_admin_domain_name`/     (将Web管理应用当作普通应用在应用网关注册，不启用WAF)     

| 默认用户 | 默认口令 |
|:-----:|------|
| `admin` | `J@nusec123` |

> 需要修改口令后才能继续管理功能!      

#### 端口
| 端口  | 描述 |
|:-----:|------|
|80     | 固定的网关HTTP入口，主节点和从节点均开启     |
|443    | 固定的网关HTTPS入口，主节点和从节点均开启    |
|`9080` | 可修改, HTTP Web 管理入口, 仅主节点开启     |
|`9443` | 可修改, HTTPS  Web 管理入口,  仅主节点开启  |  


> 默认的 `9080` 及 `9443` 端口可在配置文件 `/usr/local/janusec/config.json` 中修改。     

#### 进程
> `/usr/local/janusec/janusec`  

#### 配置文件
> `/usr/local/janusec/config.json`   

`config.json` 中的 PostgreSQL 口令，将自动修改为加密存储，以32字节为界。    

* 当口令长度 \<= 32, 视为明文口令，将自动加密替换.  
* 当口令长度 \> 32, 视为已加密，仅在内存解密.     


#### 服务
使用 `systemd` 管理Janusec服务，位置:

> CentOS/RHEL 7: `/usr/lib/systemd/system/janusec.service`     
> Debian 9: `/lib/systemd/system/janusec.service`    

查看更多信息，可执行：    

> #`systemctl cat janusec.service`   

或者     

> #`systemctl status janusec.service`  

#### PostgreSQL
PostgreSQL ( 9.3, 9.4, 9.5, 9.6, 或 10 ) 没有包含在发布包中，在安装主节点之前，您需要自行安装并准备数据库、用户名、口令.    
下面以 CentOS7 和 PostgreSQL 为例，简述 PostgreSQL 的安装步骤。   

##### 在 CentOS 7 中部署 PostgreSQL 10
主要参考 https://wiki.postgresql.org/wiki/YUM_Installation
首先添加源，方便通过yum安装。

> `yum install https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-centos10-10-2.noarch.rpm`  

如果该链接失效，可通过  `https://yum.postgresql.org/repopackages.php` 获取最新地址。   
添加成功后，可通过YUM安装。         

> #`yum install postgresql10-server`   
> #`/usr/pgsql-10/bin/postgresql-10-setup initdb`   
> #`systemctl restart postgresql-10.service`  
> #`su - postgres`  
> -bash-4.2$ `psql`   


接下来，在PostgreSQL的指令控制台继续操作：      

> postgres=\# create user `janusec` with password '`your_password`';  
> postgres=\# create database janusec owner janusec;   
> postgres=\# grant all privileges on database janusec to janusec;  
> postgres=\# \q   
> exit  

修改PostgreSQL认证方式     

> #`vi /var/lib/pgsql/10/data/pg_hba.conf`  

修改 pg_hba.conf 中这一行:     

> `host    all    all    127.0.0.1/32   md5`     

重启 PostgreSQL 服务   

> systemctl restart postgresql-10.service    


#### 为Web管理启用 HTTPS  
如果已配置证书，可使用任一Web应用的域名，以及管理端口（默认9443）进行访问：  

> `https://your_application_domain:9443/`   

也可为Janusec Web管理系统分配一个域名（如： `janusec.your_domain_name.com`），并将Web管理配置为Janusec应用网关的一个应用（不需要启用WAF）, 然后可通过如下地址访问:   

> `https://janusec.your_domain_name.com/`   


