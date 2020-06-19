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

When listen=false in config.json :  

> http://`your_primary_node_ip_address`/janusec-admin/    (first use)
> https://`your_application_domain_name`/janusec-admin/  (after certificate configured)  
When listen=true  in config.json :   

> http://`your_primary_node_ip_address:9080`/janusec-admin/    (first use)     
> https://`your_primary_node_domain_name:9443`/janusec-admin/  (after certificate configured)     

When using primary node only, any application domain name can be used for admin.  
But if you have one or more replica nodes, you should apply for a seperate domain name for primary node.   


#### Digital Certificate Management
see [Certificate Management](/documentation/certificate-management)  

#### Application Management
see [Application Management](/documentation/application-management/)

#### Node Management
see [Node Management](/documentation/node-management/)

#### WAF Management
see [WAF Management](/documentation/waf-management/)
