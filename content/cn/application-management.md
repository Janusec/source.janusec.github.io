---
title: "应用管理"
keywords: "JANUSEC应用网关, WAF, Web Application Firewall"
description: "JANUSEC应用网关的应用管理"
date: 2018-05-20T21:58:35+08:00
draft: false
weight: 510
---

# 应用管理
----

#### 1 添加或编辑应用  

打开Web管理门户并导航至`应用管理`。   

![添加或编辑应用](/images/application2.png "JANUSEC应用网关的应用管理")  

#### 2 Scheme（HTTP/HTTPS）说明   

后端scheme（从**应用网关**到**后端真实服务器**）通常在内网，默认选择 `http`即可 ，不需要证书。  

备注：   

* 从**用户浏览器**到**应用网关**这一段使用的scheme，通常使用HTTPS（需要数字证书），通过勾选下方的“将HTTP请求重定向到HTTPS”来实现。如果已经拥有数字证书，可在[证书管理](/cn/certificate-management) 录入。如果您希望JANUSEC自行申请免费的数字证书，可在下方`域名配置`中选择使用`ACME自动证书`（免费证书），并确保域名的真实DNS已经指向JANUSEC应用网关，用于证书颁发机构验证域名所有权，不用在证书管理中录入（注意：自动证书暂不支持使用通配型域名；自动证书不支持多节点部署；自动证书不支持本地hosts域名）。   


#### 3 路由说明  

JANUSEC应用网关支持4种路由模式：  
* Reverse_Proxy: 反向代理模式（默认使用该模式），需要有后端服务器（格式IP:Port，端口号不可省略），网关会将请求转发到后端的服务，支持多个后端服务器；   
* Local_FastCGI: 本机FastCGI模式，用于运行PHP或Python，不需要后端服务器；    
* Static_WebSite: 静态网站模式，应用网关直接作为Web服务器，用于静态内容的发布，不需要后端服务器，需要指定默认文件名（如index.html）及静态内容所在目录；  
* K8S_Ingress: K8S Ingress Controller模式，需要配置K8S Pods API（如 http://127.0.0.1:8080/api/v1/namespaces/default/pods ）和Pod监听的端口（如80），网关通过该API获取Pod地址列表，并将HTTP请求转发给Pod 。   

备注:  如果后端服务器或K8S Pod运行的不是HTTP服务，可使用 **端口转发** 功能，通过TCP/UDP四层转发（四层转发没有WAF/CC等安全功能） 。      


#### 4 用户IP地址  

应用网关默认使用`REMOTE_ADDR`获取用户IP地址。   

备注：

* `REMOTE_ADDR`表示直接从IP报文获取IP地址，实际获取的是上一跳的IP地址；    
* `X-Forwarded-For`表示从HTTP请求头部的`X-Forwarded-For`字段获取IP地址，实际获取的是上一跳传递过来的用户IP地址（可能伪造）；   
* JANUSEC应用网关会将`REMOTE_ADDR`获取的IP附加在HTTP头部的`X-Forwarded-For`后面；   
* 用户直接访问应用网关时，应用网关应使用`REMOTE_ADDR`获取用户IP地址，后端业务如需处理用户IP地址，可提取HTTP头部`X-Forwarded-For`中的最后一个IP地址；  
* 当用户先经过CDN再访问应用网关时，应用网关应根据CDN厂商的说明选择适当的方式获取用户真实IP（通常选择`X-Forwarded-For`），后端业务如需处理用户IP地址，可提取HTTP头部`X-Forwarded-For`倒数第二个IP地址.     

