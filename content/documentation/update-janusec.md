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

Latest version: v1.2.2 (May 15, 2021)   

v1.2.2  (May 15, 2021): Add 2-Factor Authentication for janusec-admin. Mobile App (Google Authenticator or Microsoft Authenticator) is required.    
v1.2.1  (May 09, 2021): Add SMTP test button, and CAPTCHA language init   
v1.2.0  (May 05, 2021): Add 5-second shield and SMTP notifications  
v1.1.0  (Apr 24, 2021): Add Lark authentication, operation log, and WAF optimization    
v1.0.0  (Apr 04, 2021): Add ACME automatic certificate, referring sites statistics and authentication web configuration  
v0.9.16 (Jan 10, 2021): Add IP Address Policy, include Allow and Block   
v0.9.15 (Dec 27, 2020): Improve CC defense, Log cleaning period can be configured  
v0.9.14 (Dec 21, 2020): Add CAS 2.0 authentication, and optimize the UI of CAPTCHA  
v0.9.13 (Nov 22, 2020): Custom static web site when domain name not configured  (example: IP address used), and API interface optimizaiton.    
v0.9.12 (Nov 14, 2020): Add TCP/UDP port forwarding, and admin UI optimizaiton.   
v0.9.11 (Oct 24, 2020): Add health check for backend servers, add CSP  
v0.9.10 (Sep 26, 2020): Add nftables support for CC defense   
v0.9.9 (Jun 19, 2020): Add static files cache   
v0.9.8 (May 17, 2020): Add LDAP Authentication, static website, fastcgi support.  
v0.9.7 (Mar 29, 2020): Add OAuth2 (WeChat Work, DingTalk, Feishu) for applications and admin portal. Admin listen configurable. Web SSH configurable.  
v0.9.6 (Feb 27, 2020): Add domain 301 redirect.  

#### View current version  

The version information is available at admin portal, or:  

> `./janusec --version`  

#### From v0.9.10+ 

Update JANUSEC like this:     

> #`wget https://www.janusec.com/download/janusec-1.2.2-amd64.tar.gz`  
> #`tar zxf ./janusec-1.2.2-amd64.tar.gz`  
> #`cd janusec-1.2.x-amd64`   
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

> #`wget https://www.janusec.com/download/janusec-1.2.2-amd64.tar.gz`  
> #`tar zxf ./janusec-1.2.2-amd64.tar.gz`  
> #`cd /data/janusec-1.0.xx/`  
> #`./install.sh`  
> #`systemctl restart janusec`  

#### From V0.9.5~V0.9.8  

Step 1:  

config.json changed in V0.9.9, so backup and new config.json is required:   

> #`cd /data/`  
> #`wget https://www.janusec.com/download/janusec-1.2.2-amd64.tar.gz`  
> #`tar zxf ./janusec-1.2.2-amd64.tar.gz`  
> #`mv /usr/local/janusec/config.json /usr/local/janusec/config.json.old`  
> #`cp ./janusec-1.0.xx/config.json.primary_bak /usr/local/janusec/config.json`  

Edit `/usr/local/janusec/config.json`, refer to [Configuration File](/documentation/configuration/), set database information.    

> #`vi /usr/local/janusec/config.json`  

Step 2:  

Check nftables started.  

Step 3:  

Install the latest version（config.json will not be overwrote if it exists.）：  

> #`cd /data/janusec-1.0.xx/`  
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
