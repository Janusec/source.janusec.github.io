---
title: "OAuth2 Authentication"
keywords: "Janusec, Application Gateway, WAF, Web Application Firewall, Web应用防火墙, OAuth2"
description: "Janusec Authentication"
date: 2020-03-28T23:04:25+08:00
draft: false
weight: 650
---

# OAuth2 Authentication      
----  

### Introduction  

Janusec Application Gateway supports the following authentication：  

* LDAP  
* CAS 2.0  
* WeWork  
* DingTalk  
* Feishu   
* Lark  

Used for janusec-admin, and internal applications, for employees.  

### Configuration  

Enable authentication in Web admin UI. 

### How to Get UserID in Applications    

Janusec will add the following headers if authentication passed.     

> X-Auth-Token: `Access-Token`    
> X-Auth-User: `UserID`  

The application can use the identity authentication function without modification, and can also obtain user identity (WeWork/Dingtalk/Feishu/Lark) through X-Auth-User, or obtain further information with Access-Token (WeWork/Feishu/Lark).    

### LDAP  


登录界面显示:  

> "display_name": "Login with LDAP",   

LDAP登录入口(配置一个常用的应用域名作为网关的访问域名，并需要修改下面的http/https及域名，后面的路径不变，也不带端口号):  

> "entrance": "`https://your_domain.com`/ldap/login",  


LDAP服务器地址(格式采用 域名:端口 ，如果启用TLS，请注意修改端口号)：  

> "address": "ldap_domain:389"  

LDAP DN区分名称(需要根据实际DN修改，{uid}请保持不变)：  

> "dn": "uid={uid},ou=People,dc=janusec,dc=com",  

如果是Active Directory，则请将`uid={uid}`修改为`CN={uid}`，其他根据实际修改。   

是否启用TLS加密传输(如果启用，请一并检查LDAP服务器端口，默认636)：  

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

需要在[飞书开放平台](https://open.feishu.cn/)注册应用(比如名称JANUSEC)并经企业管理员审核通过。  
需要在飞书开放平台后台配置"安全设置"-"重定向URL"，配置为"https://your_domain.com/oauth/feishu"   


登录界面显示:  

> "display_name": "Login with Feishu",   

回调地址(配置一个常用的应用域名作为网关的访问域名，并需要修改下面的http/https及域名，后面的路径不变，也不带端口号):  

> "callback": "`https://your_domain.com`/oauth/feishu",  

请在飞书开放平台注册一个自建应用，获取下面的信息。

> "appid": "cli_9ef21d00e", (Please use AppKey of the H5 Micro Application)    
> "appsecret": "ihUBspRAG1PtNdDLUZ"     

### Lark  

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

### Logout   

Just add one link `/oauth/logout` on your application.    


### Note for multiple nodes  

 
If the background management entrance uses OAuth2, you need to configure `portal` in `config.json`, where the domain name should be configured as one of the domain names of any configured application.  

But it should be noted that DNS resolution should ensure that its resolution for all applications, in the same office site, all point to the same gateway. If the application accessed by the employee points to a different gateway address than the callback domain name, OAuth2 will not be available.   


