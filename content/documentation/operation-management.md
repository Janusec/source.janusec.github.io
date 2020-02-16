---
title: "Operation Management"
keywords: "Janusec, Application Gateway, WAF, Web Application Firewall"
description: "Operation Management of Janusec Application Gateway"
date: 2018-05-25T21:04:25+08:00
draft: false
weight: 600
---

# Operation Management  
----  

#### Deployment Architecture
| Architecture | Master Node | Slave Node | Description |
|--------------|:-----------:|:----------:|-------------|
| Single-Node  | One         | None       | small scale applications with unified web management |
| Scalable     | One         | Any        | large scale applications with unified web management |
| Multiple Autonomous System | Any    |  Any  |  Any Single or Scalable Architectures, maintained by different teams, each autonomous system has an unified web management portal.  |   


#### Admin Account  
Web Administration address is one of the following:   

> http://`your_master_node_ip_address`/janusec-admin/    (first use)
> https://`your_application_domain_name`/janusec-admin/  (after certificate configured)  

| Default User  | Default Password |
|:-----:|------|
| `admin` | `J@nusec123` |

> modify password is required!      

#### Ports
| Port  | Description |
|:-----:|------|
|80     | Fixed, Gateway HTTP Entrance, Master Node and Slave Nodes    |
|443    | Fixed, Gateway HTTPS Entrance, Master Node and Slave Nodes   |

#### Process
> `/usr/local/janusec/janusec`  

#### Configuratation File
> `/usr/local/janusec/config.json`   

The PostgreSQL password in `config.json` will be encrypted automatically   

* when length of password \<= 32, will be treated as plain password and will be encrypted.  
* when length of password \> 32, will be treated as encrypted password and will be decrypted in memory.    


#### Service
The service file is managed by `systemd` and located in:

> CentOS/RHEL 7: `/usr/lib/systemd/system/janusec.service`     
> Debian 9: `/lib/systemd/system/janusec.service`    

View more information about janusec.service by:  

> #`systemctl cat janusec.service`   

or   

> #`systemctl status janusec.service`  

#### PostgreSQL
PostgreSQL ( 9.3, 9.4, 9.5, 9.6, or 10 ) is not included in the release package, you should prepare it before installation, required by the Master Node.   

Before the installation of the Master Node, dbname, dbuser and password should be ready.  

##### Example with CentOS 7 and PostgreSQL 10
Refer to https://wiki.postgresql.org/wiki/YUM_Installation

> `yum install https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-centos10-10-2.noarch.rpm`  

if the link outdated, copy the right link from `https://yum.postgresql.org/repopackages.php`     

> #`yum install postgresql10-server`   
> #`/usr/pgsql-10/bin/postgresql-10-setup initdb`   
> #`systemctl restart postgresql-10.service`
> #`su - postgres`  
> -bash-4.2$ `psql`   


PostgreSQL Shell   

> postgres=\# create user `janusec` with password '`your_password`';  
> postgres=\# create database janusec owner janusec;   
> postgres=\# grant all privileges on database janusec to janusec;  
> postgres=\# \q   
> exit  

PostgreSQL Authentication  

> #`vi /var/lib/pgsql/10/data/pg_hba.conf`  

Modified Authentication Method in pg_hba.conf:     

> `host    all    all    127.0.0.1/32   md5`     

Restart PostgreSQL Service  

> systemctl restart postgresql-10.service    


#### Enable HTTPS for Administration 
At least one certificate and one application configured, you can use the application domain and administration port to access it.  

For Single Node:  

> `https://your_application_domain/janusec-admin/`   

