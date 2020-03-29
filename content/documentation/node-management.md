---
title: "Node Management"
keywords: "Janusec Application Gateway, Slave Node, Master Node"
description: "Node Management of Janusec Application Gateway"
date: 2018-05-20T22:38:35+08:00
draft: false
weight: 520
---

# Node Management  
----
![Application Gateway](/images/gateway2.png "Janusec Application Gateway Nodes")  
#### Node Type
* Master Node, there must have one and only one Master Node, and a PostgreSQL is required.  
* Slave Node, optional, no database required.

#### Single Node Architecture  
* One Master Node.  
* No DNS Load Balance required.   
* For small scale web applications.   

#### Multiple Nodes Architecture  
* One `Master Node` and multiple `Slave Nodes` Architecture.   
* Only the Master Node requires a PostgreSQL database. 
* GSLB or DNS Load Balance required.   
* For large scale web applications.   
 

#### Slave Node   
> Open web administration portal, and manage slave nodes.   

![Slave Node](/images/node1.png "Slave Node of Janusec Application Gateway")  

Slave Node: `node_key` in `/usr/local/janusec/config.json` are corresponding to the image, like the following:    

```
{
	"node_role": "slave",
	...
	"slave_node": {		
		"node_key": "8c4609...5a5fa9",
		"sync_addr": "http://192.168.100.107:9080/janusec-admin/api"
	}	
}
```

For security reasons, you should apply for a seperate domain name for the master node, and configure an application, as follow:  

> Application Name: JANUSEC  
> Destination: 127.0.0.1:9999 (It will not be accessed)  
> Domain: domain name for the master node    
> Certificate: certificate available to above domain name  

Then "sync_addr" can be configured with https, example: `https://your_gate_domain:9443/janusec-admin/api`   

When listen=true in config.json, "sync_addr" should has colon and port number.    
When listen=false, remove colon and port number.   
