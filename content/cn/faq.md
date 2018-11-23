---
title: "FAQ"
keywords: "FAQ, Janusec Application Gateway"
description: "FAQ of Janusec Application Gateway"
date: 2018-05-12T07:45:49+08:00
draft: false
weight: 1000
---

# FAQ
----

##### Q: Janusec应用网关与其它WAF的主要区别 ?
> A: **Janusec应用网关** 也可称之为**网关式WAF**，它为克服传统WAF的缺点（如需部署Agent、HTTPS支持不好、数字证书私钥泄露等）而设计，将 WAF (Web Application Firewall)集成到到Web应用网关中，提升了用户访问、安全防御、维护管理等方面的体验。    

##### Q: Janusec应用网关是否需要在业务服务器上部署Agent ?
> A: 不需要.  

##### Q: Janusec应用网关是否支持 **HTTPS** ?
> A: 支持,Janusec应用网关天然支持 **HTTPS** .  

##### Q: 证书私钥是否安全 ?
> A: 证书私钥加密存储在数据库中，仅在内存解密使用.  Janusec应用网关不需要使用以文件形式存放的证书，服务器上的明文证书文件可在妥善备份后删除。   

#### Q: 接入Janusec应用网关后，应用中如何获取用户的IP地址 ?
> A: Janusec应用网关将会把通过IP包提取的IP地址（REMOTE_ADDR）附加在`X-Forwarded-For`后面, 因此业务中如需使用用户IP地址，可提取`X-Forwarded-For`中的最后一个IP地址（网关直接向用户提供服务时），或倒数第二个IP地址（网关前面还存在可信任的CDN时）.        

#### Q: 安装遇到问题，该如何排查？
> A: 可参考 [问题诊断](/cn/diagnose/)。  