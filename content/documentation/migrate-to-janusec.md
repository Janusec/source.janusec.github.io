---
title: "Migrate to Janusec"
keywords: "Janusec, Application Gateway, WAF, Web Application Firewall"
description: "Migrate to Janusec Application Gateway"
date: 2018-06-06T21:56:05+08:00
draft: false
weight: 650
---

# Migrate to Janusec Application Gateway  
----

#### Step 1: Install and Configure Janusec Application Gateway  
Refer to [Installation](/documentation/installation/), install janusec application gateway and configure digital certificate, application.   

#### Step 2: Hosts Test  
After configuration, modify you local hosts file `C:\Windows\System32\drivers\etc\hosts` ( not the Gateway) for test.   

> `the_gateway_ip`  `your_domain_name`    

Then, open web browser and navigate to `https://your_domain_name`.  



#### Step 3: Modify DNS  
If test OK, modify your DNS setting for production, let domain name point to the gateway, and restore your local hosts.   


#### Step 4: Enhance Security ( Optional )   
Modify the listening address of your real servers, from external or all IP address (example: `0.0.0.0:80`)  to  internal IP address (example: `10.10.10.10:80`).  

