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

> 本文仅供旧版本升级参考，新安装请忽略。  

最新版本: v0.9.6 (2020.02.27)

### V0.9.5升级到V0.9.6  

需要手工修改表格domains，添加2个字段：  

> `psql -h 127.0.0.1 -U janusec -W janusec`  
> `alter table domains add column redirect boolean default false, add column location varchar(256) default null;`  

然后正常安装即可。

### V0.9.4升级到V0.9.6  

0.9.5变更了服务类型，建议升级前执行如下操作：  

> `systemctl stop janusec`  

0.9.6需要手工修改表格domains，添加2个字段：  

> `psql -h 127.0.0.1 -U janusec -W janusec`  
> `alter table domains add column redirect boolean default false, add column location varchar(256) default null;`  

然后正常安装即可。

如果:

> `systemctl restart janusec`  

无效  ，可以kill其进程。


