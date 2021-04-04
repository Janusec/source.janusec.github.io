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

* Debian 9/10+ (Debian 10+ is preferred)  
* CentOS 7/8+  
* RHEL 7/8+  


### Time

Please make sure the time is correct, time zone may be any.

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

The version must be greater than `10`.  

### Ports

> `netstat -anp | grep LISTEN | grep ':\(80\|443\)\s'`  

Janusec will use 80/443 , if other program occupied thest ports, you shoud change it before installation of Janusec.   

If listen=true in config.json, Janusec will use 9080/9443 also, used for internal management.  

> `netstat -anp | grep LISTEN | grep ':\(9080\|9443\)\s'`  

### Nodes Sync

In order to sync correctly, requires:  

* Replica node use the correct time, error less than 60 seconds.  
* The `node_key` in `/usr/local/janusec/config.json` is the same with [Node Management](/documentation/node-management).  
  
### Log

Log file is under /usr/local/janusec/log/  

### nftables

Please make sure nftables works well and there are no redundant rules that affect JANUSEC. Refer to [Installation](/documentation/installation/).  

After JANUSEC started, the rules is like this:  
 
```
[root@CentOS8]# nft list table inet janusec -a
table inet janusec { # handle 20
	set blocklist { # handle 2
		type ipv4_addr
		flags timeout
	}

	chain input { # handle 1
		type filter hook input priority 0; policy accept;
		@nh,96,32 @blocklist drop # handle 3
	}
}

```

If your IP was blocked during the test, the firewall rules can be cleared (the follow-up will still be triggered normally):

> nft flush ruleset  

Or reduce the block time on the WAF/CC configuration.

### More Information

If all above are OK, you can stop the janusec service, and switch to run it under console, to view more output:  

> #`systemctl stop janusec`  
> #`cd /usr/local/janusec`  
> #`./janusec`  

如果发现有错误输出，可通过QQ群（776900157）反馈。  
If error found, you can sent to the bottom email, or submit an issue on https://github.com/Janusec/janusec/issues  
  

## Development

---

### Operating System

Linux is preferred.  
Console debug only for other operating systems.  
The release script (release.sh) support Linux only.  

### PostgreSQL

Different configuration files used, `./config.json` for development , and `/usr/local/janusec/config.json` for deployment .  

### Golang

At least Go 1.16+ .  

### Code

> git clone https://github.com/Janusec/janusec.git   

