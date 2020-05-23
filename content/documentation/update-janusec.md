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

Latest version: v0.9.8 (May 17, 2020)   
v0.9.8 (May 17, 2020): Add LDAP Authentication, static website, fastcgi support.  
v0.9.7 (Mar 29, 2020): Add OAuth2 (WeChat Work, DingTalk, Feishu) for applications and admin portal. Admin listen configurable. Web SSH configurable.  
v0.9.6 (Feb 27, 2020): Add domain 301 redirect.  

#### View current version  

The version information is available at admin portal, or:  

> `./janusec --version`  


#### Version 0.9.5, 0.9.6, 0.9.7 upgrade to 0.9.8  

config.json changed in V0.9.8, so backup and new config.json is required:   

> #`cd /data/`  
> #`wget https://www.janusec.com/download/janusec-latest.tar.gz`  
> #`tar zxf ./janusec-latest.tar.gz`  
> #`mv /usr/local/janusec/config.json /usr/local/janusec/config.json.old`  
> #`cp ./janusec-0.9.8/config.json.master_bak /usr/local/janusec/config.json`  

Edit `/usr/local/janusec/config.json`, refer to [Configuration File](/documentation/configuration/), set database information.    

> #`vi /usr/local/janusec/config.json`  

then install the latest version（config.json will not be overwrote if it exists.）：  

> #`cd /data/janusec-0.9.8/`  
> #`./install.sh`  
> #`systemctl restart janusec`  

#### Version 0.9.4 or earlier upgrade to 0.9.8  

0.9.5 changed the service type, so before installation:  

> #`systemctl stop janusec`  

stop the service. Next, and then repeat above upgrade process.  

If:  

> #`systemctl restart janusec`  

not work, just kill it.  
