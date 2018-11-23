---
title: "节点管理"
keywords: "Janusec应用网关, 主节点, 从节点"
description: "Janusec应用网关的节点管理"
date: 2018-05-20T22:38:35+08:00
draft: false
weight: 520
---

# 节点管理  
----
![应用网关](/images/gateway2.png "Janusec应用网关节点")   

#### 节点类型
* 主节点, 有且只有一个，并需要使用PostgreSQL.  
* 从节点，可选，0个或多个，不需要数据库。  

#### 单节点架构  
* 一个主节点，没有从节点.  
* 不需要GSLB或DNS负载均衡.   
* 适用于一个或多个小型Web应用.   

#### 多节点架构  
* 一个主节点，多个从节点.   
* 主节点需要使用PostgreSQL. 
* 需要GSLB或DNS负载均衡（不同地区的用户查询同一个域名，获取到不同的IP地址）.   
* 适用于一个或多个大型Web业务.   
 

#### 从节点   
> 在统一的Web管理界面,可管理所有的从节点.   

![从节点](/images/node1.png "Janusec应用网关的从节点")  

从节点配置文件`/usr/local/janusec/config.json`中的`node_key`，需要据此配置.  




