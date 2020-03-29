---
title: "Installation"
keywords: "Installation, Janusec Application Gateway"
description: "Installation of Janusec Application Gateway"
date: 2018-05-19T10:11:09+08:00
draft: false
weight: 300
---

# Installation
----

### Requirements
| Role                | Operating System   | Database |
|---------------------|--------------------------------------------------|----------|
| Master Node | CentOS/RHEL 7, or Debian 9, x86_64, with systemd | PostgreSQL 9.3 / 9.4 / 9.5 / 9.6 / 10  |   
| Slave Node  | CentOS/RHEL 7, or Debian 9, x86_64, with systemd | Not required |  


### Step 1: Download
> $cd ~  
> $wget `https://www.janusec.com/download/janusec-latest.tar.gz`  
> $tar zxf ./janusec-latest.tar.gz  

### Step 2: Install
Switch to root and run install.sh , janusec application gateway will be installed to `/usr/local/janusec/ ` 

> $su   
> #cd janusec-0.9.7   
> #./install.sh   

Select `1. Master Node`, then it will:   

* copy files to `/usr/local/janusec/`   
* copy service file to system service directory   
* Enable Janusec Application Gateway as a system service, but not start it for the first time.   

### Step 3: Config
PostgreSQL is not included in release package, you should prepare database name and account.   
Now we assume you have `PostgreSQL` installed already, and database name and account is ready, then edit `/usr/local/janusec/config.json` :

##### Master Node (The First Node)
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

* "node_role": "master"  ( fixed `master` )

##### Slave Node (Optional)
Usually only one **Master Node** is required for small scale web applications.  
**Slave Nodes** is for large scale web applications, and need GSLB (Global Server Load Balance) of yourselves.  
You must copy the `node_key` in web administration portal if you need slave nodes, and paste into the `config.json` of slave nodes.

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

* "node_role": "`slave`"  (fixed `slave`)  
* "node_key": "`produced_by_web_admin_in_master_node`"  (produced by web admin)  
* "sync_addr": "`http://master_ip/janusec-admin/api`"  (replace with the master IP address)

### Step 4: Start
> #systemctl start janusec  

### Step 5: Test Installation
Open web browser such as `Chrome`, navigate with address:

> http://`your_master_ip_address`/janusec-admin/  

This is the first administration address for Janusec Application Gateway.  
Login with default username `admin` and password `J@nusec123` .  
You should change the password for security reasons.

