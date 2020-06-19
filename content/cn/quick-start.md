---
title: "快速入门"
keywords: "Janusec应用网关,网关WAF, WAF, Web应用防火墙"
description: "快速建立一个单节点的Janusec应用网关。"
date: 2018-05-17T22:28:32+08:00
draft: false
weight: 100
---

# 快速入门
----

[Switch to English Edition of Quick Start](/documentation/quick-start/)   


本入门指导将安装一个单节点（主节点）的Janusec[应用网关](https://www.janusec.com/)（网关WAF）.   


## 安装需求  
----

| 节点                | 操作系统   | 数据库 |
|---------------------|--------------------------------------------------|----------|
| 主节点 | CentOS/RHEL 7+, 或 Debian 9+, x86_64, 使用 systemd | PostgreSQL 9.3 / 9.4 / 9.5 / 9.6 / 10  |   
| 副本节点  | CentOS/RHEL 7+, 或 Debian 9+, x86_64, 使用 systemd | 不需要 |  

本入门只安装一个主节点，不安装副本节点，如需扩展，可参考[安装](/cn/installation/)一节。
  
## 安装
----
##### 步骤 1: 下载
> $cd ~  
> $wget `https://www.janusec.com/download/janusec-latest.tar.gz`  
> $tar zxf ./janusec-latest.tar.gz  

##### 步骤 2: 安装
请切换到root用户并运行 install.sh , janusec应用网关将安装在目录： `/usr/local/janusec/ ` 

> $su   
> #cd janusec-0.9.9   
> #./install.sh   

选择 `1. Primary Node`   

然后安装程序会自动将所需文件复制到安装目录 `/usr/local/janusec/`，将服务配置文件复制到系统服务目录，以及将服务设置为自动启动，但首次安装时不会启动，需要在配置完成后手工启动一次。   

##### 步骤 3: 配置 
PostgreSQL没有包含在发布包中，需要自行准备PostgreSQL数据库、用户名 、口令，可参考[运维管理](/cn/operation-management/)中的PostgreSQL安装。   
现在我们假设您已经安装好了PostgreSQL，且数据库已创建，用户名和口令已准备好。  
然后编辑 `/usr/local/janusec/config.json` ，快速入门只修改数据库配置即可。  
(由于JSON格式支持的注释格式看起来不方便，下面采用`//`作为注释说明，实际使用时需要删除注释):  

```
{
    "node_role": "primary",            // 单节点或主节点配置为"primary"
    "primary_node": {                  
        "admin": {                    // 后台管理
            "listen": true,           // 后台管理界面开启独立的监听端口，通常用于只允许内网登录，不允许外网登录
            "listen_http": ":9080",   // 格式为 :port 或 内网IP:Port，listen为true时，允许后台管理通过 http://IP:9080/janusec-admin/ 访问
            "listen_https": ":9443",  // 格式为 :port 或 内网IP:Port，listen为true时，允许后台管理通过 https://any_application_domain:9443/janusec-admin/ 访问
            "portal": "https://your_gate_domain.com:9443/janusec-admin/",   // 不使用OAuth时先忽略，用于管理入口的OAuth回调，如果listen为false，请去掉冒号和端口号
            "webssh_enabled": false   // 为true时，允许使用Web SSH运维功能
        },
        "database": {                 // PostgreSQL 数据库配置，需要至少使用9.3以上版本
            "host": "127.0.0.1",      // PostgreSQL IP地址
            "port": "5432",           // PostgreSQL 监听端口，默认5432
            "user": "postgres",       // 数据库用户名
            "password": "123456",     // 数据库口令，不超过32位，直接配置明文，Janusec会自动加密
            "dbname": "janusec"       // 数据库名
        },
        "oauth": { 
            ... 
        }
    },
    "replica_node": {
        ...
    }
}
```

更多具体配置，可参见[配置文件](/cn/configuration/)说明。


##### 步骤 4: 启动网关并测试
> #systemctl start janusec  

打开浏览器（比如Chrome）,当(config.json中listen=false时) ，使用如下地址：

> http://`您的网关IP地址`/janusec-admin/    

当(config.json中listen=true时)，使用如下地址：  

> http://`您的网关IP地址:9080`/janusec-admin/    

这是Janusec应用网关的第一个管理地址（后面可启用安全的管理地址）。  
默认用户名：`admin`   
默认口令：`J@nusec123`   


## 配置数字证书 (如使用HTTPS，必选)
----
如果仅使用HTTP，不使用HTTPS，可跳过此步骤；但强烈建议配置证书并启用HTTPS。

使用浏览器打开 http://`您的网关IP地址`/janusec-admin/ 并[添加一张数字证书](/cn/certificate-management/)。
如果您还没有数字证书，可以从`Let's Encrypt`申请免费的数字证书，或者让Janusec生成一张自签名的数字证书（自签名证书仅用于测试用途）。

## 配置Web应用 (必选)
----
使用浏览器打开  http://`您的网关IP地址`/janusec-admin/ 并[添加一个应用](/cn/application-management/).  
填写应用名称、实际服务器的 `IP:端口` 等信息。

## 修改DNS或Hosts (必选)
----
生产环境，需要将修改DNS将您的域名指向网关地址。  
测试环境，可直接修改您本地电脑的hosts文件： 
 `C:\Windows\System32\drivers\etc\hosts`.    

## 验证
----
配置完成后，验证网关是否正常工作。  
打开浏览器，访问：
http://`your_domain_name`/   
或   
https://`your_domain_name`/ .  
如果可以正常访问，表明网关已正常工作。  

## WAF验证
----
检验WAF（Web应用防火墙）是否工作正常。
可使用如下测试用例:  

> `http://your_domain_name/.svn/entries`   
> `http://your_domain_name/test?id=1 and 1=1`  

阻断效果:   
![WAF](/images/waf2.png "WAF of Janusec Application Gateway")  

