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

Used for janusec-admin, and internal applications, for employees.  

### Configuration  

Enable authentication in config.json :   

```
"oauth": {
            "enabled": true,
            "provider": "wxwork",
            ...
}
```

provider, `ldap` means LDAP authentication，`cas2` means CAS 2.0,`wxwork` means WxWork, `dingtalk` means DingTalk, `feishu` means Feishu.    

### How to Get UserID in Applications    

Janusec will add the following headers if authentication passed.     

> X-Auth-Token: `Access-Token`  (Note: 0.9.15 uses `X-Auth-Token` instead previous `Authorization`)     
> X-Auth-User: `UserID`  

The application can be used without modification.   

### LDAP  

在配置文件的`"ldap"`字段：  

登录界面显示:  

> "display_name": "Login with LDAP",   

LDAP登录入口(配置一个常用的应用域名作为网关的访问域名，并需要修改下面的http/https及域名，后面的路径不变，也不带端口号):  

> "entrance": "`https://your_domain.com`/ldap/login",  


LDAP服务器地址(格式采用 域名:端口 ，如果启用TLS，请注意修改端口号)：  

> "address": "ldap_domain:389"  

LDAP DN区分名称(需要根据实际DN修改，{uid}请保持不变)：  

> "dn": "uid={uid},ou=People,dc=janusec,dc=com",

是否启用TLS加密传输(如果启用，请一并检查LDAP服务器端口，默认636)：  

> "using_tls": false   


### CAS 2.0  

在配置文件的`"cas2"`字段：  

// 显示在登录界面
> "display_name": "Login with CAS 2.0",  

// CAS服务器入口，使用/cas结尾   
> "entrance": "https://cas_server/cas",  

// 回调地址，使用网关域名，以/oauth/cas2结尾，不带端口号
> "callback": "http://gate.janusec.com/oauth/cas2"  


### 企业微信配置  

在配置文件的`"wxwork"`字段：  

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

在配置文件的`"dingtalk"`字段：  

登录界面显示:  

> "display_name": "Login with DingTalk",   

回调地址(配置一个常用的应用域名作为网关的访问域名，并需要修改下面的http/https及域名，后面的路径不变，也不带端口号):  

> "callback": "`https://your_domain.com`/oauth/dingtalk",  

请在钉钉开放平台注册一个自建应用，获取下面的信息。

> "appid": "dingoa8xvc",   
> "appsecret": "crrALdXUIj4T0zBekYh4u9sU_T1GZT"   

appsecret即上述自建应用的Secret，请据实修改。    

### 飞书配置  

需要在飞书开放平台注册应用(比如名称JANUSEC)并经企业管理员审核通过。  
需要在飞书开放平台后台配置"安全域名"-"重定向URL"，配置为"https://your_domain.com/oauth/feishu"   

在配置文件的`"feishu"`字段：  

登录界面显示:  

> "display_name": "Login with Feishu",   

回调地址(配置一个常用的应用域名作为网关的访问域名，并需要修改下面的http/https及域名，后面的路径不变，也不带端口号):  

> "callback": "`https://your_domain.com`/oauth/dingtalk",  

请在飞书开放平台注册一个自建应用，获取下面的信息。

> "appid": "cli_9ef21d00e",  
> "appsecret": "ihUBspRAG1PtNdDLUZ"     


### Logout   

Just add one link `/oauth/logout` on your application.    


### 多节点注意事项  

OAuth2需要使用回调域名(在config.json中callback配置)，这里请选定并配置一个域名，可以是任意已配置应用中的域名(不包括单独为主节点申请的域名)，假设为www.your_domain.com 。  

但需要注意的是，DNS解析应确保其对所有应用的解析，在同一个办公场地，均指向同一个网关。如果员工访问的应用，跟回调域名指向不同的网关地址，则OAuth2将不可用。  



