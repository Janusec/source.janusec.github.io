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

Latest version: v0.9.11 (Oct 24, 2020)   
v0.9.11 (Oct 24, 2020)： Add health check for backend servers, add CSP
v0.9.10 (Sep 26, 2020): Add nftables support for CC defense  
v0.9.9 (Jun 19, 2020): Add static files cache  
v0.9.8 (May 17, 2020): Add LDAP Authentication, static website, fastcgi support.  
v0.9.7 (Mar 29, 2020): Add OAuth2 (WeChat Work, DingTalk, Feishu) for applications and admin portal. Admin listen configurable. Web SSH configurable.  
v0.9.6 (Feb 27, 2020): Add domain 301 redirect.  

#### View current version  

The version information is available at admin portal, or:  

> `./janusec --version`  

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

> #`wget https://www.janusec.com/download/janusec-latest.tar.gz`  
> #`tar zxf ./janusec-latest.tar.gz`  
> #`cd /data/janusec-0.9.11/`  
> #`./install.sh`  
> #`systemctl restart janusec`  

#### From V0.9.5~V0.9.8  

config.json changed in V0.9.9, so backup and new config.json is required:   

> #`cd /data/`  
> #`wget https://www.janusec.com/download/janusec-latest.tar.gz`  
> #`tar zxf ./janusec-latest.tar.gz`  
> #`mv /usr/local/janusec/config.json /usr/local/janusec/config.json.old`  
> #`cp ./janusec-0.9.11/config.json.primary_bak /usr/local/janusec/config.json`  

Edit `/usr/local/janusec/config.json`, refer to [Configuration File](/documentation/configuration/), set database information.    

> #`vi /usr/local/janusec/config.json`  

then install the latest version（config.json will not be overwrote if it exists.）：  

> #`cd /data/janusec-0.9.11/`  
> #`./install.sh`  
> #`systemctl restart janusec`  

#### From Version 0.9.4 or earlier  

0.9.5 changed the service type, so before installation:  

> #`systemctl stop janusec`  

stop the service.   

Next, backup /usr/local/janusec/config.json or record the password in it.   
Delete /usr/local/janusec/config.json , then install script will copy a new config.json .  
Install latest Janusec, and check config.json to fill in the password.  

If:  

> #`systemctl restart janusec`  

not work, just kill it.  
