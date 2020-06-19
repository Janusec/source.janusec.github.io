---
title: "节点管理"
keywords: "Janusec应用网关, 主节点, 副本节点"
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
* 副本节点，可选，0个或多个，不需要数据库。  

#### 单节点架构  
* 一个主节点，没有副本节点.  
* 不需要GSLB或DNS负载均衡.   
* 适用于一个或多个小型Web应用.   

#### 多节点架构  
* 一个主节点，多个副本节点.   
* 主节点需要使用PostgreSQL. 
* 需要GSLB或DNS负载均衡（不同地区的用户查询同一个域名，获取到不同的IP地址）.   
* 适用于一个或多个大型Web业务.   
 

#### 副本节点   
> 在统一的Web管理界面,可管理所有的副本节点.   

![副本节点](/images/node1.png "Janusec应用网关的副本节点")  

副本节点配置文件`/usr/local/janusec/config.json`中的`node_key`，需要据此配置，类似：    
```
{
	"node_role": "replica",
	...
	"replica_node": {		
		"node_key": "8c4609...5a5fa9",
		"sync_addr": "http://192.168.100.107:9080/janusec-admin/api"
	}	
}
```

安全起见，启动副本节点时，应给主节点单独申请一个域名，并配置一个应用，建议配置如下：  

> Application Name: JANUSEC   
> Destination: 127.0.0.1:9999 (不会实际访问)  
> Domain: 主节点专用域名  
> Certificate: 可用于主节点专用域名的证书  

这样"sync_addr"可以使用https加密传输通道，如`https://your_gate_domain:9443/janusec-admin/api`。   

当管理入口开启独立监听(配置文件config.json中listen=true)时，"sync_addr"应该带上冒号和端口号；  
当(listen=false)时,则应去掉冒号和端口号。  
