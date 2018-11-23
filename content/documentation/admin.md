---
title: "Administration"
keywords: "Administration, Janusec Application Gateway, WAF, Web Application Firewall"
description: "Administration of Janusec Application Gateway"
date: 2018-05-19T10:11:09+08:00
draft: false
weight: 400
---

# Web Administration
----

#### Administration Portal
----
http://`janusec_application_gateway_master_node_ip`:9080/

For security reasons, use internal IP address is preferred.  
You can modify the config file of the master node:  `/usr/local/janusec/config.json` : 

> "admin_http_listen": "`10.10.10.10`:9080",  
> "admin_https_listen": "`10.10.10.10`:9443",  

#### Digital Certificate Management
see [Certificate Management](/documentation/certificate-management)  

#### Application Management
see [Application Management](/documentation/application-management/)

#### Node Management
see [Node Management](/documentation/node-management/)

#### WAF Management
see [WAF Management](/documentation/waf-management/)
