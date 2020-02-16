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

最新版本: v0.9.5 (2020.02.16)

#### Version 0.9.4 upgrade to 0.9.5  

0.9.5变更了服务类型，建议升级前执行如下操作：  

> `systemctl stop janusec`  

然后正常安装即可。

如果:

> `systemctl restart janusec`  

无效  ，可以kill其进程。


