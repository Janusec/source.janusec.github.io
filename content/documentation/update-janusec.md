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

#### Version 0.9.3 upgrade to 0.9.4  

Install janusec normally, or do the following before installation.  

> drop table nodes;  

and delete the line which include `node_id` in config.json.  

> systemctl restart janusec.service  

#### Version 0.9.2 upgrade to 0.9.3  
PostgreSQL table `applications` need to be updated.   
The installation program will not do this operation, you should execute the following SQL command manually.   

> alter table applications add column hsts_enabled boolean default true;   

and then refer to [Installation](/documentation/installation), install Janusec Application Gateway and restart the `janusec.service`:  

> systemctl restart janusec.service   



#### Version \<=0.9.1 upgrade to 0.9.2   
PostgreSQL table `applications` need to be updated.   
The installation program will not do this operation, you should execute the following SQL command manually.   

> alter table applications add column ip_method bigint default 1;    
> alter table applications add column waf_enabled boolean default true;   

and then refer to [Installation](/documentation/installation), install Janusec Application Gateway and restart the `janusec.service`:  

> systemctl restart janusec.service   




