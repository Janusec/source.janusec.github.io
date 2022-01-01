---
title: "Appendix 2: PostgreSQL Operations"
keywords: "PostgreSQL, Janusec, Application Gateway, WAF, Web Application Firewall"
description: "PostgreSQL常见操作"
date: 2022-01-01T14:01:34+08:00
draft: false
weight: 1400
---

# Appendix 2: PostgreSQL Operations
---

## 1 PostgreSQL Installation  

PostgreSQL is not included in the release package, you should prepare it before installation, required by the Primary Node.   

Before the installation of the Primary Node, dbname, dbuser and password should be ready.  

### 1.1 Example with Debian 10 and PostgreSQL 11

> apt install postgresql  
> su - postgres  
> psql  

> create user janusec with password 'J@nusec123';  
> create database janusec owner janusec;  
> grant all privileges on database janusec to janusec;  
> \q  
> exit  
> psql -h 127.0.0.1 -U janusec -W janusec  


### 1.2 Example with CentOS 7 and PostgreSQL 10

Refer to https://wiki.postgresql.org/wiki/YUM_Installation

> `yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm`  

if the link outdated, copy the right link from `https://yum.postgresql.org/repopackages.php`     

> #`yum install postgresql10-server`   
> #`/usr/pgsql-10/bin/postgresql-10-setup initdb`   
> #`systemctl restart postgresql-10.service`  
> #`su - postgres`  
> -bash-4.2$ `psql`   


PostgreSQL Shell   

> postgres=\# create user `janusec` with password &#39;`your_password`&#39;;  
> postgres=\# create database janusec owner janusec;   
> postgres=\# grant all privileges on database janusec to janusec;  
> postgres=\# \q   
> exit  

PostgreSQL Authentication  

> #`vi /var/lib/pgsql/10/data/pg_hba.conf`  

Modified Authentication Method in pg_hba.conf:     

> `host    all    all    127.0.0.1/32   md5`     

Set PostgreSQL stard when boot, and restart PostgreSQL Service  

> systemctl enable postgresql-10   
> systemctl restart postgresql-10.service    



## 2 Common Operations  

### 2.1 PSQL Command Console  

> psql -h `127.0.0.1` -U `janusec` -W `janusec`   

其中， `-h` followed by the address of the PostgreSQL database, `-U` followed by the database username, `-W` prompts for password, the last `janusec` represents the database name.  

### 2.2 Common Commands    

* `\l` : list all databases    
* `\c db_name` : switch to database db_name  
* `\d` or `\dt` : list all tables of current database  
* `\du` : list all roles    
* `\d table_name` : view the information of table_name  
* `select * from table_name` : view all rows of table_name  
* `\q` : quit  
* `\h` : list all keywords  
* `\?` : help  
  