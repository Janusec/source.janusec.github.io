---
title: "Node Management"
keywords: "Janusec Application Gateway, Replica Node, Primary Node"
description: "Node Management of Janusec Application Gateway"
date: 2018-05-20T22:38:35+08:00
draft: false
weight: 520
---

# Node Management  
----
![Application Gateway](/images/gateway2.png "Janusec Application Gateway Nodes")  
#### Node Type
* Primary Node, there must have one and only one Primary Node, and a PostgreSQL is required.  
* Replica Node, optional, no database required.

#### Single Node Architecture  
* One Primary Node.  
* No DNS Load Balance required.   
* For small scale web applications.   

#### Multiple Nodes Architecture  
* One `Primary Node` and multiple `Replica Nodes` Architecture.   
* Only the Primary Node requires a PostgreSQL database. 
* GSLB or DNS Load Balance required.   
* For large scale web applications.   
 

#### Replica Node   
> Open web administration portal, and manage replica nodes.   

![Replica Node](/images/node1.png "Replica Node of Janusec Application Gateway")  

Replica Node: `node_key` in `/usr/local/janusec/config.json` are corresponding to the image, like the following:    

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

For security reasons, you should apply for a seperate domain name for the primary node, and configure an application, as follow:  

> Application Name: JANUSEC  
> Destination: 127.0.0.1:9999 (It will not be accessed)  
> Domain: domain name for the primary node    
> Certificate: certificate available to above domain name  

Then "sync_addr" can be configured with https, example: `https://your_gate_domain:9443/janusec-admin/api`   

When listen=true in config.json, "sync_addr" should has colon and port number.    
When listen=false, remove colon and port number.   
