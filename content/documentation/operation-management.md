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
| Architecture | Primary Node | Replica Node | Description |
|--------------|:-----------:|:----------:|-------------|
| Single-Node  | One         | None       | small scale applications with unified web management |
| Scalable     | One         | Any        | large scale applications with unified web management |
| Multiple Autonomous System | Any    |  Any  |  Any Single or Scalable Architectures, maintained by different teams, each autonomous system has an unified web management portal.  |   


#### Admin Account  

Web Administration address is one of the following:   

When listen=false in config.json :  

> http://`your_primary_node_ip_address`/janusec-admin/    (first use)
> https://`your_application_domain_name`/janusec-admin/  (after certificate configured)  
When listen=true  in config.json :   

> http://`your_primary_node_ip_address:9080`/janusec-admin/    (first use)     
> https://`your_primary_node_domain_name:9443`/janusec-admin/  (after certificate configured)     

When using primary node only, any application domain name can be used for admin.  
But if you have one or more replica nodes, you should apply for a seperate domain name for primary node.   


| Default User  | Default Password |
|:-----:|------|
| `admin` | `J@nusec123` |

> modify password is required!      

#### Ports
| Port  | Description |
|:-----:|------|
|80     | Fixed, Gateway HTTP Entrance, Primary Node and Replica Nodes    |
|443    | Fixed, Gateway HTTPS Entrance, Primary Node and Replica Nodes   |
|9080   | When listen=true in config.json, Primary Node Only |
|9443   | When listen=true in config.json, Primary Node Only |  

#### Process
> `/usr/local/janusec/janusec`  

#### Configuratation File
> `/usr/local/janusec/config.json`   

The PostgreSQL password in `config.json` will be encrypted automatically   

* when length of password \<= 32, will be treated as plain password and will be encrypted.  
* when length of password \> 32, will be treated as encrypted password and will be decrypted in memory.    


#### Service
The service file is managed by `systemd` and located in:

> CentOS/RHEL 7: `/usr/lib/systemd/system/janusec`     
> Debian 9: `/lib/systemd/system/janusec`    

View more information about janusec by:  

> #`systemctl cat janusec`   

or   

> #`systemctl status janusec`  

#### PostgreSQL

PostgreSQL is not included in the release package, you should prepare it before installation, required by the Primary Node.   

Before the installation of the Primary Node, dbname, dbuser and password should be ready.  

Refer to [Appendix 2: PostgreSQL Operations](/documentation/appendix-psql/) .  

##### Example with Debian 10 and PostgreSQL 11

> apt install postgresql  
> su - postgres  
> psql  

> create user janusec with password 'J@nusec123';  
> create database janusec owner janusec;  
> grant all privileges on database janusec to janusec;  
> \q  
> exit  
> psql -h 127.0.0.1 -U janusec -W janusec  


##### Example with CentOS 7 and PostgreSQL 10
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


#### Enable HTTPS for Administration 

At least one certificate and one application configured, you can use the application domain and administration port to access it.  

For Single Node:  

> `https://your_application_domain/janusec-admin/` (when listen=false in config.json)     
> `https://your_application_domain:9443/janusec-admin/` (when listen=true in config.json)  

