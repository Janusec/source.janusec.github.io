---
title: "升级Janusec"
keywords: "Janusec, Application Gateway, WAF, Web Application Firewall"
description: "升级到新版本Janusec应用网关"
date: 2018-06-10T20:11:45+08:00
draft: false
weight: 660
---

# 升级到新版本Janusec应用网关   
----

#### Version 0.9.3 upgrade to 0.9.4  

可正常安装，建议升级前执行如下操作：  

> drop table nodes;  

删除配置文件`config.json`中含有 `node_id` 的这一行.  

> systemctl restart janusec.service  

#### Version 0.9.2 upgrade to 0.9.3  
PostgreSQL 表 `applications` 需要升级.   
安装程序不会自动升级，需要手工执行SQL指令：   

> alter table applications add column hsts_enabled boolean default true;   

然后参考 [安装](/cn/installation) 安装Janusec应用网关, 并重启 `janusec.service`:  

> systemctl restart janusec.service   



#### Version \<=0.9.1 upgrade to 0.9.2   
PostgreSQL 表 `applications` 需要升级.   
安装程序不会自动升级，需要手工执行SQL指令：  

> alter table applications add column ip_method bigint default 1;    
> alter table applications add column waf_enabled boolean default true;   

然后参考 [安装](/cn/installation) 安装Janusec应用网关, 并重启 `janusec.service`:   

> systemctl restart janusec.service   




