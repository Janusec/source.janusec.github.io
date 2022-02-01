---
title: "Appendix 3: High Availability Configuration"
keywords: "High Availability, High Availability Configuration, Janusec, Application Gateway, WAF, Web Application Firewall"
description: "Janusec Application Gateway High Availability Configuration"
date: 2020-04-09T14:16:34+08:00
draft: false
weight: 1500
---

# Appendix 3: High Availability Configuration
---

## 1 Architecture Description

JANUSEC Application Gateway itself supports the installation of one primary node and multiple replica nodes for load balancing.  
If you need to guarantee the high availability of a single node, you can build a high availability node with keepalived.  
The recommended architecture for high availability deployment is:   

1. deploy a master node dedicated to backend management, not open to users  
2. deploy two or more replica nodes (Replica) and install keepalived on each replica node   

The following two replica nodes, assuming IP addresses `192.168.56.101` and `192.168.56.102`, use keepalived to build a virtual node, and let the domain name points to the IP address of the virtual node `192.168.56.103` .  

Note that only one gateway server in a group of keepalived nodes is working at the same moment, and if this one fails, it is automatically migrated to another server.   
If you need more than one virtual node to work at the same time, create a new set of keepalived gateway nodes.   

## 2 Install keepalived

Using Debian as an example, keepalived can be installed using the following command:    

> apt install keepalived  

## 3 Configure keepalived  

Starting from v1.2.8, the configuration file `keepalived.conf` for keepalived is provided under the JANUSEC installation directory `/usr/local/janusec/` and this file can be modified.  

* `interface eth0` in the `vrrp_instance VI_01` section, change `eth0` to the actual internal NIC name used, usually `eth0`, `eth1`, `enp0s8`, etc. The IP address corresponds to `192.168.56.101` or `192.168.56.102` above `  

* The virtual IP address in the `virtual_ipaddress` section is `192.168.56.103` (modified according to actual needs)  

After the modification, copy `keepalived.conf` to `/etc/keepalived/` directory, and then start the keepalived service:   

> systemctl restart keepalived   

Note: Both servers need to have keepalived installed.  


## 4 Validation

To see if the keepalived service is running properly.  

> systemctl status keepalived   

Check the IP addresses of the two servers separately.   

> ip addr show `eth0`    

eth0 is the same as the interface in the configuration file above.  
If the configuration is successful, you can see on one of the servers that the virtual IP has been successfully added.   

1: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000  
    link/ether 08:00:27:6d:69:45 brd ff:ff:ff:ff:ff:ff  
    inet 192.168.56.101/24 brd 192.168.56.255 scope global eth0  
       valid_lft forever preferred_lft forever  
    `inet 192.168.56.103/32 scope global eth0`  
       valid_lft forever preferred_lft forever  
    inet6 fe80::a00:27ff:fe6d:6945/64 scope link   
       valid_lft forever preferred_lft forever  


You can then simulate a gateway failure by stopping the gateway service on this server:   

> systemctl stop janusec   

Then re-check the IP addresses of the two servers, if it works properly, then `192.168.56.103` will be migrated to the other server.  

After testing, start the gateway service that was manually stopped:  

> systemctl start janusec  

