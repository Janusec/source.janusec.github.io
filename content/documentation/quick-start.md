---
title: "Quick Start"
keywords: "Janusec Application Gateway Quick Start, WAF, Web Application Firewall"
description: "Build a single node Janusec Application Gateway quickly."
date: 2018-05-17T22:28:32+08:00
draft: false
weight: 100
---

# Quick Start
----

[快速入门中文版](/cn/quick-start/)   


This document will guide you to install a Single-Node **Janusec Application Gateway**.    


## 0 Requirements  
----

| Role          | Operating System   | Database |
|---------------|-------------------------------------------------------------------------------------------------|-----------------------------------|
| Primary Node  | Debian 9/10/11+, or CentOS/RHEL 7/8+, x86_64, with systemd and nftables (Debian 10 is prefered) | SQLite3 or PostgreSQL 10/11/12+   |   
| Replica Node  | Debian 9/10/11+, or CentOS/RHEL 7/8+, x86_64, with systemd and nftables (Debian 10 is prefered) | Not required |  


## 1 Prepare nftables  
----

nftables used for CC defense.    
You can view the ruleset through:  

> #nft list ruleset  

If the rule is not empty, it may affect the effectiveness of the firewall policy.    
Assuming that the nftables rule is empty now, then continue.   


##### 1.1 Debian 11  


In Debian 11, nftables have been installed and enabled by default and managed through the UFW service. The status can be viewed through the following command:   

> #`ufw status numbered`   

If port 80 or 443 is not allowed or cannot be accessed, you may need to modify it:  

When primary_node - admin - `listen` is `false` in config file `/usr/local/janusec/config.json`：  

> #`ufw allow 80,443/tcp`   

When primary_node - admin - `listen` is `true` in config file `/usr/local/janusec/config.json`：        

> #`ufw allow 80,443,9080,9443/tcp`    


##### 1.2 Debian 10   

nftables for Debian 10:    

> apt install nftables   

##### 1.3 CenOS 8  

nftables has been installed for CentOS 8.   

##### 1.4 CenOS 7  

nftables is not installed for CentOS 7 by default, installation is required:    

> #yum -y install nftables  
> #systemctl enable nftables  
> #systemctl start nftables  


## 2 Installation
----
##### Step 1: Download  

The latest version is available at https://github.com/Janusec/janusec/releases .  

> $cd ~  
> $wget `https://www.janusec.com/download/janusec-1.4.2-amd64.tar.gz`  
> $tar zxf ./janusec-1.4.2-amd64.tar.gz  

##### Step 2: Install
Switch to root and run install.sh , janusec application gateway will be installed to `/usr/local/janusec/ ` 

> $su   
> #cd janusec-1.4.x-amd64     
> #./install.sh   

Select `1. Primary Node`, then it will:   

* copy files to `/usr/local/janusec/`   
* copy service file to system service directory   
* Enable Janusec Application Gateway as a system service, but not start it for the first time.   

##### Step 3: Config  

Starting from v1.4.0, it supports two types of databases, SQLite and PostgreSQL, which can be selected according to the situation.   

If you choose SQLite, you can ignore the fields under "database" in the configuration file below.
If you choose PostgreSQL, as it is not included in the release package, you need to prepare the PostgreSQL database, username, and password yourself. The installation steps for PostgreSQL can refer to [Appendix 2: PostgreSQL Operations](/documentation/appendix-psql/) . If you have already installed PostgreSQL, created the database, and prepared the username and password, you can continue with the following actions.   

Edit `/usr/local/janusec/config.json` (use `//` as comment, please delete them before using it):

```
{
    "node_role": "primary",            // "primary" for primary node, "replica" for replica nodes
    "primary_node": {                  // keep empty for replica nodes
        "admin": {                    // Administrator portal
            "listen": true,           // Listen on new ports for admin portal
            "listen_http": ":9080",   // Format :port or Interal_IP:Port，when listen is true, http://IP:9080/janusec-admin/ is available
            "listen_https": ":9443",  // Format :port or Interal_IP:Port，when listen is true, https://any_application_domain:9443/janusec-admin/ is available
            "portal": "https://your_gate_domain.com:9443/janusec-admin/"   // Please skip this item when OAuth not used. It is for admin portal OAuth callback, if listen is false in config.json, remove colon and port number
        },
        "database_type": "sqlite",    // sqlite or postgres
        "database": {                 // PostgreSQL 10/11/12+
            "host": "127.0.0.1",      // PostgreSQL IP Address
            "port": "5432",           // PostgreSQL Port, 5432
            "user": "postgres",       // PostgreSQL user
            "password": "123456",     // PostgreSQL password, less than 32bit
            "dbname": "janusec"       // PostgreSQL database name
        }
    },
    "replica_node": {      // for replica nodes
        ...
    }
}
```

More detailed configuration, see [Configuration File](/documentation/configuration/)  


##### Step 4: Start and Test Installation
> #systemctl start janusec  

Open web browser such as `Chrome`, when listen=false in config.json, navigate with address:

> http://`your_ip_address`/janusec-admin/  

when listen=true in config.json:  

> http://`your_ip_address`:9080/janusec-admin/  

This is the administration address for Janusec Application Gateway.  
Login with default username `admin` and password `J@nusec123` .


## 3 Certificate (optional)
----
If you only use HTTP, skip this step.  
Open http://`your_ip_address`/janusec-admin/ and add a new certificate.
If you don't have a certificate, you can get a free certificate from `Let's Encrypt`, or let Janusec produce a self-signed certificate( only for test), or skip certificate configuration and select `ACME Automatic Certificate` under `Application Management`.

## 4 Application (required)
----
Open http://`your_ip_address`/janusec-admin/ and add a new application.
Fill in application name, actual IP:Port etc.

## 5 DNS or Hosts (required)
----
Modify your DNS settings, let `your_domain_name` point to the Gateway for production, or modify you local hosts `C:\Windows\System32\drivers\etc\hosts` ( not the Gateway) for test.   
To use ACME automated certificates, the domain name must be a real internet accessible domain name, otherwise the certification authority will not pass the callback verification.  


## 6 Validation
----
Open http://`your_domain_name`/ or https://`your_domain_name`/ .  

## 7 WAF Validation
----
Test cases:  

> `http://your_domain_name/.svn/entries`   
> `http://your_domain_name/test?id=1 and 1=1`  

Block information:  
![WAF](/images/waf2.png "WAF of Janusec Application Gateway")  


## 8 Firewall nftables Validation  
----

> #nft list table inet janusec  

or:    

> #nft list table inet janusec -a  

The result is like this:  

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

If you need to test the effect of CC defense, please check the CC protection rules in WAF management at first.  