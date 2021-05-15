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

| Role          | Operating System   | Database |
|---------------|--------------------------------------------------|----------|
| Primary Node  | Debian 9/10+, or CentOS/RHEL 7/8+, x86_64, with systemd and nftables (Debian 10 is prefered) | PostgreSQL 10/11/12+   |   
| Replica Node  | Debian 9/10+, or CentOS/RHEL 7/8+, x86_64, with systemd and nftables (Debian 10 is prefered) | Not required |  

## Prepare nftables  
----
nftables used for CC defense.    

nftables for Debian 10:    

> apt install nftables   

nftables is not installed for CentOS 7 by default, installation is required:    

> #yum -y install nftables  
> #systemctl enable nftables  
> #systemctl start nftables  

nftables has been installed for CentOS 8, and as backend of firewalld, just enable firewalld：  

> #systemctl enable firewalld  
> #systemctl start firewalld  

Now, you can view the ruleset through:  

> #nft list ruleset  

If the rule is not empty, it may affect the effectiveness of the firewall policy. Assuming that the nftables rule is empty now, then continue.   


### Step 1: Download
> $cd ~  
> $wget `https://www.janusec.com/download/janusec-1.2.2-amd64.tar.gz`  
> $tar zxf ./janusec-1.2.2-amd64.tar.gz  

### Step 2: Install
Switch to root and run install.sh , janusec application gateway will be installed to `/usr/local/janusec/ ` 

> $su   
> #cd janusec-1.2.x-amd64     
> #./install.sh   

Select `1. Primary Node`, then it will:   

* copy files to `/usr/local/janusec/`   
* copy service file to system service directory   
* Enable Janusec Application Gateway as a system service, but not start it for the first time.   

### Step 3: Config
PostgreSQL is not included in release package, you should prepare database name and account.   
Now we assume you have `PostgreSQL` installed already, and database name and account is ready, then edit `/usr/local/janusec/config.json` :

##### Primary Node (The First Node)
> {  
> &nbsp;&nbsp;&nbsp;&nbsp;"node_role": "primary",  
> &nbsp;&nbsp;&nbsp;&nbsp;"primary_node": {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"database": {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"host": "127.0.0.1",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"port": "5432",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"user": "`your_postgresql_user`",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"password": "`your_postgresql_password`",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"dbname": "`janusec`"  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}  
> &nbsp;&nbsp;&nbsp;&nbsp;},  
> &nbsp;&nbsp;&nbsp;&nbsp;"replica_node": {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"node_key": "",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"sync_addr": ""  
> &nbsp;&nbsp;&nbsp;&nbsp;}  
> }  

* "node_role": "primary"  ( fixed `primary` )

##### Replica Node (Optional)
Usually only one **Primary Node** is required for small scale web applications.  
**Replica Nodes** is for large scale web applications, and need GSLB (Global Server Load Balance) of yourselves.  
You must copy the `node_key` in web administration portal if you need replica nodes, and paste into the `config.json` of replica nodes.

> {  
> &nbsp;&nbsp;&nbsp;&nbsp;"node_role": "`replica`",  
> &nbsp;&nbsp;&nbsp;&nbsp;"primary_node": {  
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
> &nbsp;&nbsp;&nbsp;&nbsp;"replica_node": {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"node_key": "`produced_by_web_admin_in_primary_node`",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"sync_addr": "`http://primary_ip/janusec-admin/api`"  
> &nbsp;&nbsp;&nbsp;&nbsp;}  
> }  

* "node_role": "`replica`"  (fixed `replica`)  
* "node_key": "`produced_by_web_admin_in_primary_node`"  (produced by web admin)  
* "sync_addr": "`http://primary_ip/janusec-admin/api`"  (replace with the primary IP address)

### Step 4: Start
> #systemctl start janusec  

### Step 5: Test Installation
Open web browser such as `Chrome`, navigate with address:

> http://`your_primary_ip_address`/janusec-admin/  

This is the first administration address for Janusec Application Gateway.  
Login with default username `admin` and password `J@nusec123` .  
You should change the password for security reasons.

