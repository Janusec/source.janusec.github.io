---
title: "Update Janusec"
keywords: "Janusec, Application Gateway, WAF, Web Application Firewall"
description: "Update to new version of Janusec Application Gateway"
date: 2018-06-10T20:11:45+08:00
draft: false
weight: 660
---

# Update to Latest Version   
----

Latest version: v0.9.5 (Feb 16, 2020)

#### Version 0.9.4 upgrade to 0.9.5  

0.9.5 changed the service type, so before installation:  

> systemctl stop janusec.service  

stop the service, and then install the latest version.

If:  

> systemctl restart janusec  

not work, just kill it.  
