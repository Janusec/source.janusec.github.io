---
title: "升级Janusec"
keywords: "Janusec, Application Gateway, WAF, Web Application Firewall"
description: "升级到新版本Janusec应用网关"
date: 2018-06-10T20:11:45+08:00
draft: false
weight: 660
---

# 升级到新版本Janusec应用网关   
----

> 本文仅供旧版本升级参考，新安装请忽略。  

最新版本: v0.9.8 (2020.05.17)  

版本历史:   
v0.9.8 (2020.05.17): 增加LDAP认证，增加静态网站、FastCGI网站支持。  
v0.9.7 (2020.03.29): 为各应用及管理入口增加OAuth2统一登录（可选企业微信、钉钉、飞书）；可限制管理入口开放范围（例如只开放内网）；Web SSH安全运维支持禁用或开启。  
v0.9.6 (2020.02.27): 增加域名重定向。  

### 查看当前版本  

当前版本信息可通过管理入口查看，或者:  

> `./janusec --version`  


### V0.9.5, 0.9.6, 0.9.7 升级到V0.9.8  

V0.9.8修改了配置文件格式，请备份config.json，并复制一份新的config.json，假设您下载的安装包在`/data/janusec-latest.tar.gz`，升级步骤参考：  

> #`cd /data/`  
> #`wget https://www.janusec.com/download/janusec-latest.tar.gz`  
> #`tar zxf ./janusec-latest.tar.gz`  
> #`mv /usr/local/janusec/config.json /usr/local/janusec/config.json.old`  
> #`cp ./janusec-0.9.8/config.json.master_bak /usr/local/janusec/config.json`  

需要编辑新的`/usr/local/janusec/config.json`，参考[配置文件](/cn/configuration/)，重新设置数据库账号口令等信息。  

> #`vi /usr/local/janusec/config.json`  

然后正常覆盖安装即可（已存在config.json的情况下不会覆盖config.json）：  

> #`cd /data/janusec-0.9.8/`  
> #`./install.sh`  
> #`systemctl restart janusec`  


### V0.9.4或更早版本升级到V0.9.8  

由于升级过程中变更了服务类型(0.9.5)，建议升级前执行如下操作：  

> #`systemctl stop janusec`  

然后再参考上述V0.9.7升级到V0.9.8的过程。

如果:

> `systemctl restart janusec`  

无效  ，可以kill其进程。

