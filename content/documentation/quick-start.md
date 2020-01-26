---
title: "Quick Start"
keywords: "Janusec Application Gateway Quick Start, WAF, Web Application Firewall"
description: "Build a single node Janusec Application Gateway quickly."
date: 2018-05-17T22:28:32+08:00
draft: false
weight: 100
---

# Quick Start
----

[快速入门中文版](/cn/quick-start/)   


This document will guide you to install a Single-Node **Janusec Application Gateway**.    


## Requirements  
----

| Role                | Operating System   | Database |
|---------------------|--------------------------------------------------|----------|
| Master Node | CentOS/RHEL 7, or Debian 9, x86_64, with systemd | PostgreSQL 9.3 / 9.4 / 9.5 / 9.6 / 10  |   
| Slave Node  | CentOS/RHEL 7, or Debian 9, x86_64, with systemd | Not required |  


  
## Installation
----
##### Step 1: Download
> $cd ~  
> $wget `https://www.janusec.com/download/janusec-latest.tar.gz`  
> $tar zxf ./janusec-latest.tar.gz  

##### Step 2: Install
Switch to root and run install.sh , janusec application gateway will be installed to `/usr/local/janusec/ ` 

> $su   
> #cd janusec-0.9.4   
> #./install.sh   

Select `1. Master Node`, then it will:   

* copy files to `/usr/local/janusec/`   
* copy service file to system service directory   
* Enable Janusec Application Gateway as a system service, but not start it for the first time.   

##### Step 3: Config 
PostgreSQL is not included in release package, you should prepare database name and account.   
Now we assume you have `PostgreSQL` installed already, and database name and account is ready, then edit `/usr/local/janusec/config.json` :

> {  
> &nbsp;&nbsp;&nbsp;&nbsp;"node_role": "master",  
> &nbsp;&nbsp;&nbsp;&nbsp;"master_node": {  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"admin_http_listen": ":9080",  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"admin_https_listen": ":9443",  
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



##### Step 4: Start and Test Installation
> #systemctl start janusec.service  

Open web browser such as `Chrome`, navigate with address:

> http://`your_ip_address`:9080/  

This is the administration address for Janusec Application Gateway.  
Login with default username `admin` and password `J@nusec123` .


## Certificate (required for `HTTPS`)
----
If you only use HTTP, skip this step.  
Open http://`your_ip_address`:9080/ and add a new certificate.
If you don't have a certificate, you can get a free certificate from `Let's Encrypt` or let Janusec produce a self-signed certificate( only for test).

## Application (required)
----
Open http://`your_ip_address`:9080/ and add a new application.
Fill in application name, actual IP:Port etc.

## DNS or Hosts (required)
----
Modify your DNS settings, let `your_domain_name` point to the Gateway for production, or modify you local hosts `C:\Windows\System32\drivers\etc\hosts` ( not the Gateway) for test.

## Validation
----
Open http://`your_domain_name`/ or https://`your_domain_name`/ .  

## WAF Validation
----
Test cases:  

> `http://your_domain_name/.svn/entries`   
> `http://your_domain_name/test?id=1 and 1=1`  

Block information:  
![WAF](/images/waf2.png "WAF of Janusec Application Gateway")  

