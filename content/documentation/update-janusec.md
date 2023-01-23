---
title: "Update Janusec"
keywords: "Janusec, Application Gateway, WAF, Web Application Firewall"
description: "Update to new version of Janusec Application Gateway"
date: 2018-06-10T20:11:45+08:00
draft: false
weight: 660
---

# Update to Latest Version   
----

> This article is only for upgrade, not for new installation.  

Latest version: v1.3.0 (Jan 23, 2023)   


#### View current version  

The version information is available at admin portal, or:  

> `./janusec --version`  

#### From v0.9.10+ 

Update JANUSEC like this:     

> #`wget https://www.janusec.com/download/janusec-1.3.0-amd64.tar.gz`  
> #`tar zxf ./janusec-1.3.0-amd64.tar.gz`  
> #`cd janusec-1.3.x-amd64`   
> #`./install.sh`  
> #`systemctl restart janusec`  

#### From V0.9.9  

First, nftables is required for CC defense from v0.9.10.    

nftables is not installed for CentOS 7 by default, installation is required:    

> #yum -y install nftables  
> #systemctl enable nftables  
> #systemctl start nftables  

nftables has been installed for CentOS 8, and as backend of firewalld, just enable firewalld ：  

> #systemctl enable firewalld  
> #systemctl start firewalld  

Then, update JANUSEC like this:  

> #`wget https://www.janusec.com/download/janusec-1.3.0-amd64.tar.gz`  
> #`tar zxf ./janusec-1.3.0-amd64.tar.gz`  
> #`cd /data/janusec-1.3.x/`  
> #`./install.sh`  
> #`systemctl restart janusec`  

#### From V0.9.5~V0.9.8  

Step 1:  

config.json changed in V0.9.9, so backup and new config.json is required:   

> #`cd /data/`  
> #`wget https://www.janusec.com/download/janusec-1.3.0-amd64.tar.gz`  
> #`tar zxf ./janusec-1.3.0-amd64.tar.gz`  
> #`mv /usr/local/janusec/config.json /usr/local/janusec/config.json.old`  
> #`cp ./janusec-1.3.x/config.json.primary_bak /usr/local/janusec/config.json`  

Edit `/usr/local/janusec/config.json`, refer to [Configuration File](/documentation/configuration/), set database information.    

> #`vi /usr/local/janusec/config.json`  

Step 2:  

Check nftables started.  

Step 3:  

Install the latest version（config.json will not be overwrote if it exists.）：  

> #`cd /data/janusec-1.3.x/`  
> #`./install.sh`  
> #`systemctl restart janusec`  

#### From Version 0.9.4 or earlier  

0.9.5 changed the service type, so before installation:  

Step 1:  

> #`systemctl stop janusec`  

stop the service.   

Step 2:  
Check nftables started.  

Step 3:  

Backup /usr/local/janusec/config.json or record the password in it.   
Delete /usr/local/janusec/config.json , then install script will copy a new config.json .  
Install latest Janusec, and check config.json to fill in the password.  

If:  

> #`systemctl restart janusec`  

not work, just kill it.  
