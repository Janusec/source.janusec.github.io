---
title: "OAuth2身份认证"
keywords: "Janusec, Application Gateway, WAF, Web Application Firewall, Web应用防火墙, OAuth2"
description: "JANUSEC应用网关身份认证管理"
date: 2020-03-28T23:04:25+08:00
draft: false
weight: 650
---

# OAuth2统一身份认证    
----  

### 简介  

Janusec Application Gateway支持如下身份认证：  

* LDAP  
* CAS 2.0  
* 企业微信扫码登录  
* 钉钉扫码登录  
* 飞书扫码登录   
* Lark扫码登录  

并同时作用于管理后台，以及员工访问内网应用。   

### 配置  

如需启用，请在Web设置界面开启。  

### 应用如何获取用户身份  

Janusec认证通过后，会在HTTP请求的头部添加两行：  

> X-Auth-Token: `Access-Token`        
> X-Auth-User: `UserID`  


应用不需要修改即可使用身份认证功能，也可以通过X-Auth-User获取用户身份（企业微信/钉钉/飞书），或者借助Access-Token（企业微信/飞书）获取进一步的信息。  

### LDAP配置  

登录界面显示:  

> "display_name": "Login with LDAP",   

LDAP登录入口(配置一个常用的应用域名作为网关的访问域名，并需要修改下面的http/https及域名，后面的路径不变，也不带端口号):  

> "entrance": "`https://your_domain.com`/ldap/login",  


LDAP服务器地址（格式采用 域名:端口 ，如果启用TLS，请注意修改端口号）：  

> "address": "ldap_domain:389"  

LDAP DN区分名称（需要根据实际DN修改，{uid}请保持不变）：  

> "dn": "uid={uid},ou=People,dc=janusec,dc=com",  

如果是Active Directory，则请将`uid={uid}`修改为`CN={uid}`，其他根据实际修改。  

是否启用TLS加密传输（如果启用，请一并检查LDAP服务器端口，默认636）：  

> "using_tls": false   

### CAS 2.0  


// 显示在登录界面
> "display_name": "Login with CAS 2.0",  

// CAS服务器入口，使用/cas结尾   
> "entrance": "https://cas_server/cas",  

// 回调地址，使用网关域名，以/oauth/cas2结尾，不带端口号
> "callback": "http://gate.janusec.com/oauth/cas2"  


### 企业微信配置  


登录界面显示:  

> "display_name": "Login with WeChat Work",   

回调地址(需要同时在该应用"开发者接口"一栏配置"授权回调域"，配置一个常用的应用域名作为网关的访问域名，并需要修改下面的http/https及域名，后面的路径不变，也不带端口号):  

> "callback": "`https://your_domain.com`/oauth/wxwork",  
 

企业ID (在https://work.weixin.qq.com/wework_admin/frame#profile 下方可看到企业ID信息):  

> "corpid": "wwd03be1f8",  

AgentID (在https://work.weixin.qq.com/wework_admin/frame#apps 创建名为JANUSEC的自建应用即可看到):  

> "agentid": "1000002",   

corpsecret (即上述自建应用的Secret，请据实修改):  

> "corpsecret": "BgZtz_hssdZV5em-AyGhOgLlm18rU_NdZI"   

### 钉钉配置  


登录界面显示:  

> "display_name": "Login with DingTalk",   

回调地址(配置一个常用的应用域名作为网关的访问域名，并需要修改下面的http/https及域名，后面的路径不变，也不带端口号):  

> "callback": "`https://your_domain.com`/oauth/dingtalk",  

请在钉钉开放平台注册一个自建应用（H5微应用），获取下面的信息。

> "appid": "dingoa8xvc",  （请复制使用H5微应用的`AppKey`，`这里容易出错!!!`）  
> "appsecret": "crrALdXUIj4T0zBekYh4u9sU_T1GZT"   

appsecret即上述自建应用的Secret，请据实修改。    

然后在"登录与分享"菜单下面添加上述callback回调地址。  


### 飞书配置  

需要在飞书开放平台注册Web应用(比如名称JANUSEC)并经企业管理员审核通过。  
需要在飞书开放平台后台配置"安全设置"-"重定向URL"，配置为"https://your_domain.com/oauth/feishu"   


登录界面显示:  

> "display_name": "Login with Feishu",   

回调地址(配置一个常用的应用域名作为网关的访问域名，并需要修改下面的http/https及域名，后面的路径不变，也不带端口号):  

> "callback": "`https://your_domain.com`/oauth/feishu",  

请在飞书开放平台注册一个自建应用，获取下面的信息。

> "appid": "cli_9ef21d00e",  
> "appsecret": "ihUBspRAG1PtNdDLUZ"     

### Lark配置

首先在[Lark Developer](https://open.larksuite.com/)配置应用，其他配置跟飞书配置基本一致。  

First register a Web application at [Lark Developer](https://open.larksuite.com/), and it should be approved by enterprise administrtor.   

And, configure under menu "Security Settings"-"Redirect URLs", Add "https://your_domain.com/oauth/lark"   

Second configure under JANUSEC:  

Display name on UI:  

> "display_name": "Login with Lark",   

Callback address, change the scheme (http/https) and one of your application domain name, do not change `/oauth/feishu`, do not use port number.  

> "callback": "`https://your_domain.com`/oauth/lark",  

Get the application information under Lark Developer:  

> "appid": "cli_9ef21d00e",  
> "appsecret": "ihUBspRAG1PtNdDLUZ"   


### 退出登录  

后端网站只需要添加一个退出链接 /oauth/logout ，用户点击后即可实现退出效果。  


### 多节点注意事项  

后台管理入口如果使用OAuth2，需要在`config.json`中配置`portal`，这里的域名应配置为任意已配置的应用的域名之一。  

但需要注意的是，DNS解析应确保其对所有应用的解析，在同一个办公场地，均指向同一个网关。如果员工访问的应用，跟回调域名指向不同的网关地址，则OAuth2将不可用。  



