---
title: "Application Management"
keywords: "Janusec Application Gateway, WAF, Web Application Firewall"
description: "Application Management of Janusec Application Gateway"
date: 2018-05-20T21:58:35+08:00
draft: false
weight: 510
---

# Application Management
----

#### Requirements  
At least one digital certificate is required if you need `https` support.    
[Certificate Management](/documentation/certificate-management)

#### Add or Edit Application  
Open web administration portal and navigate to `Application Management`.  
![Add or Edit an Application](/images/application1.png "Application Management of Janusec Application Gateway")  

**Note**:   

* Backend or Internal Scheme, default `http` and thus no certificates required in your real servers.    
* Destination, `IP:Port` format, port is required even if the port is `80`.   
* Multiple destinations for backend load balancing.   
* Client IP for WAF, default `REMOTE_ADDR`, Janusec WAF will get IP address from IP package, but if you are using Janusec Application Gateway behind a trustable CDN of third parties, usually the last IP of `X-Forwarded-For` should be taken, please refer to the documentation of CDN, and select relevant option, for example: the option `X_Forwarded_For` used for the last IP of `X-Forwarded-For` within the request header.   
* Janusec Application Gateway will add or append the IP address of the client or CDN to the end of `X-Forwarded-For`, so your real servers behind Janusec Application Gateway should always use the last IP or the penultimate IP of `X-Forwarded-For` as client IP address.    
