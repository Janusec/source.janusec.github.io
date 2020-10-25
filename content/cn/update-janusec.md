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

最新版本: v0.9.11 (2020.10.24)  

版本历史:   
v0.9.11 (2020.10.24)： 增加增加后端服务器健康检查，增加CSP
v0.9.10 (2020.09.26): 增加nftables拦截CC高频请求，启用go mod   
v0.9.9 (2020.06.19): 增加静态文件缓存  
v0.9.8 (2020.05.17): 增加LDAP认证，增加静态网站、FastCGI网站支持。  
v0.9.7 (2020.03.29): 为各应用及管理入口增加OAuth2统一登录（可选企业微信、钉钉、飞书）；可限制管理入口开放范围（例如只开放内网）；Web SSH安全运维支持禁用或开启。  
v0.9.6 (2020.02.27): 增加域名重定向。  

### 查看当前版本  

当前版本信息可通过管理入口查看，或者:  

> `./janusec --version`  

### 从V0.9.9升级  

从V0.9.10开始，启用nftables，用于拦截CC攻击，减轻应用网关压力。  

CentOS 7默认没有安装nftables，需要手工安装并启动：  

> #yum -y install nftables  
> #systemctl enable nftables  
> #systemctl start nftables  

CentOS 8已内置nftables，并作为firewalld的后端，只需要手工启动firewalld：  

> #systemctl enable firewalld  
> #systemctl start firewalld  

然后覆盖安装即可：  

> #`wget https://www.janusec.com/download/janusec-latest.tar.gz`  
> #`tar zxf ./janusec-latest.tar.gz`  
> #`cd /data/janusec-0.9.11/`  
> #`./install.sh`  
> #`systemctl restart janusec`  


### 从V0.9.5~V0.9.8 升级    

V0.9.9修改了配置文件格式，请备份config.json，并复制一份新的config.json，假设您下载的安装包在`/data/janusec-latest.tar.gz`，升级步骤参考：  

> #`cd /data/`  
> #`wget https://www.janusec.com/download/janusec-latest.tar.gz`  
> #`tar zxf ./janusec-latest.tar.gz`  
> #`mv /usr/local/janusec/config.json /usr/local/janusec/config.json.old`  
> #`cp ./janusec-0.9.11/config.json.primary_bak /usr/local/janusec/config.json`  

需要编辑新的`/usr/local/janusec/config.json`，参考[配置文件](/cn/configuration/)，重新设置数据库账号口令等信息。  

> #`vi /usr/local/janusec/config.json`  

然后正常覆盖安装即可（已存在config.json的情况下不会覆盖config.json）：  

> #`cd /data/janusec-0.9.11/`  
> #`./install.sh`  
> #`systemctl restart janusec`  


### 从V0.9.4或更早版本升级  

升级过程中变更了服务类型(0.9.5)和配置文件config.json。  
建议升级前执行如下操作：  

第一步：  

> #`systemctl stop janusec`  

否则无法重启服务。

第二步：  
备份配置文件或记录其中的口令备用（原始口令或加密后的口令均可）。  
删除配置文件config.json后，重新安装最新版本（新安装不会覆盖已存在的配置文件，因此需要删除，让安装程序自动复制一份新的配置文件）。  
安装后，检查新配置文件，将原始口令或加密后的口令配置进去。  

如果:

> `systemctl restart janusec`  

无效  ，可以kill其进程。

