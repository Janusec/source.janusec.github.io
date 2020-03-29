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

#### 需求  
如果您需要 `https` 支持，至少需要一个数字证书，参见[证书管理](/cn/certificate-management)。   

#### 配置项说明  

|   配置项                   |  说明                         |
|---------------------------|------------------------------|
| Application Name          | 应用名称，网站名称              |  
| Backen or Internal Scheme | 后端使用的scheme，http或https，默认选http即可 |
| Destination               | 后端网站的IP:Port，网关将请求转发到该处，可以配置多个，用于负载均衡 |
| Domain name               | 使用的域名，点击下方的加号可以增加域名  |
| Certificate               | 使用的证书，需要能够用于上面的域名 |
| Redirect to               | 默认不勾选，用于将不常用域名跳转到常用域名 |
| Client IP for WAF         | 网关获取IP的方式，默认REMOTE_ADDR(从IP包提取)，只有当流量来自可信任的其他设施(如CDN)时，才需要修改为其他方式 |
| Redirect HTTP to HTTPS    | 通过Location告诉浏览器跳转到HTTPS   |
| Enable HSTS for HTTPS     | 告诉浏览器在接下来的一年内自动使用HTTPS   |
| Enable WAF                | 默认勾选，启用WAF                      |
| Enable OAuth2             | 启用OAuth2统一身份认证（需要配置文件中同时启用），默认不勾选 |
| Session Expire Seconds    | OAuth2认证通过后，有效会话时间(秒)，默认7200秒，即2小时    |
| Application Owner         | 应用的负责人(默认为当前用户)              |
| Description               | 应用的描述(可选)                        |


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
