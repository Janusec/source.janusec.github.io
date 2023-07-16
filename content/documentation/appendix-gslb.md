---
title: "Appendix 7: GSLB"
keywords: "Janusec Application Gateway, GSLB"
description: "Implementing Global Server Load Balance through Janusec Application Gateway"
date: 2023-07-16T15:18:32+08:00
draft: false
weight: 1900
---

# GSLB (Global Server Load Balance)      
----

## 1 GSLB Introduction      

GSLB (Global Server Load Balance) can automatically schedule traffic through custom DNS servers based on the user's IP address.  

The professional plus edition of Janussec Application Gateway has built-in DNS server and GSLB scheduling function on the primary node, while the open source edition does not include this feature.  
   
## 2 Enable GSLB  

Refer to the following steps to enable GSLB (Global Server Load Balance) if you have multiple gateway nodes deployed.   

Assuming you have an application that provides services to internet users through: `https://demo.example.com`, and the customized domain name server is `ns01.example.com` (use the DNS server provided by the primary node of Janusec Application Gateway):  

1. Step 1: Enable firewall policy, example for Debian 11: #`ufw allow 53`  
2. Step 2: Make sure port 53 is not occupied by other applications, typically in Debian 11 it was occupied, stop it: #`systemctl stop systemd-resolved` and #`systemctl disable systemd-resolved`, then check with: #`netstat -antulp | grep :53`  
3. Step 3: Enable DNS Server in Settings - Advanced, and then restart service: #`systemctl restart janusec`  
4. Step 4: Add an `A` or CNAME record `ns01` with value ip address points to this gateway, and a `NS` record `demo` with value `ns01.example.com.` at the authoritative name server, not on this gateway. Individual domain name holders should modify it at the domain name registrar.  
5. Step 5: Add an `A` record `demo` on the gateway, and enable `Resolve to an available gateway node for load balance automatically`  
6. Step 6 (Important): Add `DNS Hostnames` (aka. `Glue Records`) for your DNS server at the domain name registrar, if not, your own dns server will not accepted by other DNS servers. This record takes approximately 24 to 48 hours to take effect.  
7. Step 7: Configure application in Application and make sure the backend source servers are available to all nodes of Janusec Application Gateway.  
8. Step 8: Open with web browser, or under command shell: `nslookup demo.example.com`, or `dig demo.example.com A`  

