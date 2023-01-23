---
title: "迁移到JANUSEC应用网关"
keywords: "Janusec, Application Gateway, WAF, Web Application Firewall, JANUSEC应用网关"
description: "迁移到JANUSEC应用网关"
date: 2018-06-06T21:56:05+08:00
draft: false
weight: 650
---

# 迁移到JANUSEC应用网关  
----

本文适用场景：将已经发布的Web应用迁移到通过JANUSEC应用网关发布。  

#### 步骤 1: 安装并配置JANUSEC应用网关  
参考 [安装](/cn/installation/), 安装JANUSEC应用网关并配置好证书、Web应用.    

#### 步骤 2: 修改Hosts文件测试   
修改本地电脑的 `C:\Windows\System32\drivers\etc\hosts` (不是网关) ，将域名临时指向网关IP，用于测试.   

> `the_gateway_ip`  `your_domain_name`    

然后可打开浏览器测试访问  `https://your_domain_name` .  


#### 步骤 3: 修改DNS  
如果测试通过，可修改DNS指向，将正式生产环境的域名指向JANUSEC应用网关，并删除您本地添加的hosts记录。   


#### 步骤 4: 提升安全 ( 可选 )   
后端真实服务器在接入JANUSEC应用网关之后，没有必要再监听外网地址了，可修改为只监听内网地址（如`10.10.10.10:80`），不再直接暴露在互联网，降低安全风险。  

