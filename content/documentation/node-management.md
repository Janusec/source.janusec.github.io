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

Slave Node: `node_key` in `/usr/local/janusec/config.json` are corresponding to the image.  




