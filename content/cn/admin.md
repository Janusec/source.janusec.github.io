---
title: "管理"
keywords: "管理, Janusec, 应用网关, WAF, Web应用防火墙"
description: "管理Janusec应用网关"
date: 2018-05-19T10:11:09+08:00
draft: false
weight: 400
---

# Web化管理
----

#### 管理入口
----
第一个管理入口：  
http://`主节点IP地址`:9080/

安全原因，推荐使用内部IP地址`IP:Port`，可在配置文件（`/usr/local/janusec/config.json`）中修改。  

> "admin_http_listen": "`10.10.10.10`:9080",  
> "admin_https_listen": "`10.10.10.10`:9443",  

#### 数字证书管理
参考 [证书管理](/cn/certificate-management)  

#### 应用管理
参考 [应用管理](/cn/application-management/)

#### 节点管理
参考 [节点管理](/cn/node-management/)

#### WAF管理
参考 [WAF管理](/cn/waf-management/)
