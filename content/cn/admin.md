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

当配置文件config.json中listen=false时，管理入口包括如下：

> http://`your_master_node_ip_address`/janusec-admin/    (首次使用)    
> https://`your_master_node_domain_name`/janusec-admin/  (证书配置后可用)   

当配置文件config.json中listen=true时，地址：  

> http://`your_master_node_ip_address:9080`/janusec-admin/    (首次使用)    
> https://`your_master_node_domain_name:9443`/janusec-admin/  (证书配置后可用) 


#### 数字证书管理
参考 [证书管理](/cn/certificate-management)  

#### 应用管理
参考 [应用管理](/cn/application-management/)

#### 节点管理
参考 [节点管理](/cn/node-management/)

#### WAF管理
参考 [WAF管理](/cn/waf-management/)
