---
title: "附录2: PostgreSQL操作"
keywords: "PostgreSQL, Janusec, Application Gateway, WAF, Web Application Firewall"
description: "PostgreSQL常见操作"
date: 2022-01-01T14:01:34+08:00
draft: false
weight: 1400
---

# 附录2: PostgreSQL操作
---

## 1 PostgreSQL安装  

PostgreSQL ( 10/11/12+ ) 没有包含在发布包中，在安装主节点之前，您需要自行安装并准备数据库、用户名、口令.    
下面简述 PostgreSQL 的安装步骤。   

### 1.1 在Debian 10中部署PostgreSQL 11

> apt install postgresql  
> su - postgres  
> psql  

> create user janusec with password &#39;`J@nusec123`&#39;;  
> create database janusec owner janusec;  
> grant all privileges on database janusec to janusec;  
> \q  
> exit  
> psql -h 127.0.0.1 -U janusec -W janusec  


### 1.2 在 CentOS 7 中部署 PostgreSQL 10
主要参考 https://wiki.postgresql.org/wiki/YUM_Installation
首先添加源，方便通过yum安装。

> `yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm`  

如果该链接失效，可通过  `https://yum.postgresql.org/repopackages.php` 获取最新地址。   
添加成功后，可通过YUM安装。         

> #`yum install postgresql10-server`   
> #`/usr/pgsql-10/bin/postgresql-10-setup initdb`   
> #`systemctl restart postgresql-10.service`  
> #`su - postgres`  
> -bash-4.2$ `psql`   


接下来，在PostgreSQL的指令控制台继续操作：      

> postgres=\# create user `janusec` with password &#39;`J@nusec123`&#39;;  
> postgres=\# create database janusec owner janusec;   
> postgres=\# grant all privileges on database janusec to janusec;  
> postgres=\# \q   
> exit  

修改PostgreSQL认证方式     

> #`vi /var/lib/pgsql/10/data/pg_hba.conf`  

修改 pg_hba.conf 中这一行:     

> `host    all    all    127.0.0.1/32   md5`     

将PostgreSQL设置为开机启动，并重启 PostgreSQL 服务   

> systemctl enable postgresql-10   
> systemctl restart postgresql-10     

## 2 常用操作  

### 2.1 进入PSQL命令控制台  

> psql -h `127.0.0.1` -U `janusec` -W `janusec`   

其中， `-h` 后面跟PostgreSQL数据库的地址，`-U` 后面跟数据库用户名，`-W` 表示提示输入数据库口令，最后面的 `janusec` 表示数据库名。  

### 2.2 常用命令    

* `\l` : 列出所有数据库名  
* `\c db_name` : 切换到数据库db_name  
* `\d` 或 `\dt` : 列出当前数据库的所有表  
* `\du` : 查看所有用户角色  
* `\d table_name` : 查看表table_name的信息  
* `select * from table_name` : 查看表格所有内容  
* `\q` : 退出  
* `\h` : 查看所有关键字  
* `\?` : 获取帮助  