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

Latest version: v0.9.6 (Feb 27, 2020)  

#### Version 0.9.5 upgrade to 0.9.6  

The table `domains` need to be modified:  

> `psql -h 127.0.0.1 -U janusec -W janusec`  
> `alter table domains add column redirect boolean default false, add column location varchar(256) default null;`  

then install the latest version.  

#### Version 0.9.4 upgrade to 0.9.5 (Feb 16, 2020)  

0.9.5 changed the service type, so before installation:  

> systemctl stop janusec.service  

stop the service, and then install the latest version.

If:  

> systemctl restart janusec  

not work, just kill it.  
