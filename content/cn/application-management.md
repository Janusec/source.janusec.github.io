---
title: "应用管理"
keywords: "Janusec应用网关, WAF, Web Application Firewall"
description: "Janusec应用网关的应用管理"
date: 2018-05-20T21:58:35+08:00
draft: false
weight: 510
---

# 应用管理
----

#### HTTPS需求  

如果已经拥有数字证书，可在[证书管理](/cn/certificate-management) 录入。  
如果您希望JANUSEC自行申请免费的数字证书，可在下方`域名配置`中选择使用`ACME自动证书`（免费证书），并确保域名的DNS已经指向JANUSEC应用网关，用于证书颁发机构验证域名所有权，不用在证书管理中录入（注意：自动证书暂不支持使用通配型域名）。  
   


#### 添加或编辑应用
打开Web管理门户并导航至`Application Management`。  
![添加或编辑应用](/images/application1.png "Janusec应用网关的应用管理")  

**注意**:   

* 后端（从网关到真实服务器）通常在内网，默认使用 `http` ，不需要证书；    
* `Destination`（后端目的地）,使用 `IP:Port` 格式，其中端口不可省略（即使是80端口）；
* 多个`Destination`用于后端负载均衡；   
* 网关获取用户IP，默认使用`REMOTE_ADDR`（从IP报文获取）；
* 当网关的前面还存在可信任的CDN时，`REMOTE_ADDR`获取的是CDN的地址，可参考CDN厂商的说明文档，通常CDN厂商会将用户IP附加在`X-Forwarded-For`后面，这时网关可配置成  `X_Forwarded_For` （网关将使用该字段的最后一个IP地址）；   
* Janusec应用网关将会把`REMOTE_ADDR`获取的IP附加在`X-Forwarded-For`后面, 因此业务中如需使用用户IP地址，可提取`X-Forwarded-For`中的最后一个IP地址（网关直接向用户提供服务时），或倒数第二个IP地址（网关前面还存在可信任的CDN时）.    
