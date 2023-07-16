---
title: "附录7: GSLB"
keywords: "JANUSEC应用网关, GSLB, 全局负载均衡"
description: "通过JANUSEC应用网关实现GSLB全局负载均衡"
date: 2023-07-16T15:18:32+08:00
draft: false
weight: 1900
---

# GSLB (全局负载均衡)      
----

## 1 GSLB简介    

GSLB (全局负载均衡) 可以根据用户的IP地址，通过自定义DNS服务器实现对流量的自动调度。  

Janusec Application Gateway 的增强体验版在主节点内置了DNS服务器和GSLB调度功能，而开源版本不含此功能。   
   
## 2 启用GSLB  

如果部署了多个网关节点，参阅以下步骤启用GSLB（全局负载均衡）。  
假设您为互联网用户提供服务的Web应用网站为 `https://demo.example.com` ，自定义域名服务器为 `ns01.example.com` （使用Janusec Application Gateway主节点自带的DNS服务器） ：   

1. 步骤1：检查开通服务器的防火墙策略（`TCP/UDP 53`），以Debian 11为例： #`ufw allow 53`  
2. 步骤2：确保端口53没有被其他应用程序占用，通常在Debian 11中53端口已经被占用，先停用： #`systemctl stop systemd-resolved` and #`systemctl disable systemd-resolved`, then check with: #`netstat -antulp | grep :53`  
3. 步骤3：在 全局设置-高级，启用DNS服务器并重启服务： #`systemctl restart janusec`  
4. 步骤4：在权威DNS服务器处（小型企业或个人域名通常为域名注册商），为网关域名服务器添加`A`记录（或CNAME记录），名称为`ns01`，值为本网关的IP地址；添加`NS`记录，名称为`demo`，值为`ns01.example.com.`  
5. 步骤5：在应用网关，创建`A`记录，名称为 `demo` ，勾选`自动解析到可用的网关节点`   
6. 步骤6（重要）：在域名注册商处，创建`DNS Hostnames`（也就是`Glue Records`），名称为`ns01`，值为本网关的IP地址；此记录大约需要24到48小时生效；如果此记录缺失，此DNS服务器将不被其他DNS服务器认可  
7. 步骤7：检查应用配置，确保后端源服务器对所有网关节点网络可达  
8. 步骤8：使用浏览器或命令行测试验证： `nslookup demo.example.com`, or `dig demo.example.com A`   

