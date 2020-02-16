---
title: "Diagnose"
keywords: "Janusec, Application Gateway, WAF, Web Application Firewall"
description: "Diagnose for Janusec Application Gateway"
date: 2018-08-05T11:52:52+08:00
draft: false
weight: 1200
---

# Diagnose
---

## Deployment
---

### Operating System

Operating System should be x86_64 and one of the following:  

* CentOS 7  
* RHEL 7  
* Debian 9  

### Service Management

`systemd` is used for Service Management , check :  

> `command -v systemctl`   

(Expect result: /usr/bin/systemctl) 


### PostgreSQL

Use `psql` to check PostgreSQL Connection:   

> psql -h `127.0.0.1` -U `janusec` -W `janusec`  

If not OK, refer to [Operation Management](/documentation/operation-management) , PostgreSQL part.

Check version within PSQL Shell:   

> select version();  

The version must be greater than `9.3`.  

### Ports

> `netstat -anp | grep LISTEN | grep ':\(80\|443\)\s'`  

Janusec will use 80/443 , if other program occupied thest ports, you shoud change it before installation of Janusec.  

### Nodes Sync

In order to sync correctly, requires:  

* Slave node use the correct time, error less than 60 seconds.  
* The `node_key` in `/usr/local/janusec/config.json` is the same with [Node Management](/documentation/node-management).  
  
  
  

## Development

---

### Operating System

Linux is preferred.  
Console debug only for other operating systems.  
The release script (release.sh) support Linux only.  

### PostgreSQL

Different configuration files used, `./config.json` for development , and `/usr/local/janusec/config.json` for deployment .  

### Golang

At least Go 1.12+ .  

### Code

> `go get -u github.com/Janusec/janusec`  

git clone is not preferred.  

