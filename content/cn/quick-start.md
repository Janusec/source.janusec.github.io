---
title: "快速入门"
keywords: "JANUSEC应用网关,网关WAF, WAF, Web应用防火墙"
description: "快速建立一个单节点的JANUSEC应用网关。"
date: 2018-05-17T22:28:32+08:00
draft: false
weight: 100
---

# 快速入门
----

[Switch to English Edition of Quick Start](/documentation/quick-start/)   

如果您只是想快速体验一下，可参考[附录：在容器中部署JANUSEC应用网关](/cn/appendix-docker/) ，并跳过下面的安装部分。    

本文将指导您正常安装一个单节点的JANUSEC应用网关（只有一个主节点，没有副本节点）。      



## 0 安装需求  
----

| 节点      | 操作系统   | 数据库 |
|----------|--------------------------------------------------------------------------------------|-------------------------------------|
| 主节点    | Debian 9/10/11+, CentOS/RHEL 7/8+, 首选Debian 10/11, x86_64, 使用 systemd和nftables   | SQLite3 或 PostgreSQL 10/11/12/13+  |   
| 副本节点  | Debian 9/10/11+, CentOS/RHEL 7/8+, 首选Debian 10/11, x86_64, 使用 systemd和nftables   | 不需要 |  

备注：  
本入门只安装一个主节点，不安装副本节点，如需扩展，可参考[安装](/cn/installation/)一节。   
Janusec v1.0.0所支持的最低数据库版本为PostgreSQL 9.6，建议直接使用`PostgreSQL 10/11/12+`版本 。  
如果您的PostgreSQL版本低于9.6，则不支持 。  

从v1.4.0版本开始，增加支持SQLite3数据库。     
  

## 1 准备主机防火墙nftables  
----

nftables用于拦截CC攻击，减轻应用网关压力。参考下面的指令配置完成nftables之后，可以通过如下指令查看规则：  

> #nft list ruleset  

如果规则不是空的，可能会影响防火墙策略的生效。  

##### 1.1 Debian 11  

Debian 11已默认安装并启用nftables并通过ufw服务进行管理，可通过如下命令查看状态   

> #`ufw status numbered`  

如果80或443端口未允许或无法访问，您可能需要执行：  

当配置文件`/usr/local/janusec/config.json`中 primary_node - admin - `listen` 为`false`时：  

> #`ufw allow 80,443/tcp`  

当配置文件`/usr/local/janusec/config.json`中 primary_node - admin - `listen` 为`true`时：     

> #`ufw allow 80,443,9080,9443/tcp`    


##### 1.2 Debian 10   

Debian 10安装nftables：  

> apt install nftables   

##### 1.3 CentOS 8  

CentOS 8已内置nftables，不需要额外的动作。   

##### 1.4 CentOS 7  

CentOS 7默认没有安装nftables，需要手工安装并启动：  

> #yum -y install nftables  
> #systemctl enable nftables  
> #systemctl start nftables  


## 2 安装
----
##### 步骤 1: 下载  

开源版本的下载链接包括:  

* Mirror 1 Github (USA):   [Github Releases](https://github.com/Janusec/janusec/releases)     
* Mirror 2 Gitee (China):  [Gitee Releases](https://gitee.com/Janusec/janusec/releases)   

增强特性体验版：  

* Mirror 3 JANUSEC: [Janusec Application Gateway Professional Plus](https://www.janusec.com/download/janusec-1.4.2-amd64.tar.gz) 

增强特性说明：增强体验版是在开源版本基础上进一步增强，增强特性部分不开源，仅用于测试或体验（JANUSEC保留在未来版本中对增强特性进行变更的权利，包括继续增强、删减等）。     

增强特性包括：  

* Cookie合规管理(提供Cookie Banner与用户同意管理) ， v1.4.2版本开始提供     
* GSLB (全局负载均衡，自带权威DNS服务器) ， v1.4.2版本开始提供   


请下载到当前用户目录`/home/xxx/`或其他非安装目录，然后解压：    

> $tar zxf ./janusec-1.4.2-amd64.tar.gz  

##### 步骤 2: 安装  

请切换到`root`用户并运行 install.sh , JANUSEC应用网关将安装在目录： `/usr/local/janusec/ ` 

> $su   
> #cd janusec-1.4.x-amd64  （根据实际版本号和CPU架构类型修改）   
> #./install.sh   

选择 `1. Primary Node`   

然后安装程序会自动将所需文件复制到安装目录 `/usr/local/janusec/`，将服务配置文件复制到系统服务目录，以及将服务设置为自动启动，但首次安装时不会启动，需要在配置完成后手工启动一次。   

##### 步骤 3: 配置   

从v1.4.0开始，支持两种数据库，SQLite和PostgreSQL，可根据情况选用。   

如果选用SQLite，可忽略下面配置文件中"database"下面的各字段。  

如果选用PostgreSQL，由于PostgreSQL没有包含在发布包中，需要自行准备好PostgreSQL数据库、用户名 、口令，PostgreSQL 的安装步骤可参考 [附录2 PostgreSQL常见操作](/cn/appendix-psql/) 。如果您已经安装好了PostgreSQL，且数据库已创建，用户名和口令已准备好，可继续下面的动作。  

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
            "portal": "https://your_gate_domain.com:9443/janusec-admin/"   // 不使用OAuth时先忽略，用于管理入口的OAuth回调，如果listen为false，请去掉冒号和端口号
        },
        "database_type": "sqlite",    // sqlite or postgres
        "database": {                 // PostgreSQL 10/11/12+
            "host": "127.0.0.1",      // PostgreSQL IP地址
            "port": "5432",           // PostgreSQL 监听端口，默认5432
            "user": "postgres",       // 数据库用户名
            "password": "123456",     // 数据库口令，不超过32位，直接配置明文，Janusec会自动加密
            "dbname": "janusec"       // 数据库名
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

这是JANUSEC应用网关的第一个管理地址（后面可启用安全的管理地址）。  
默认用户名：`admin`   
默认口令：`J@nusec123`   


## 3 配置数字证书 (可选)
----
如果仅使用HTTP，不使用HTTPS，可跳过此步骤；但强烈建议配置证书并启用HTTPS。

使用浏览器打开 http://`您的网关IP地址`/janusec-admin/ 并[添加一张数字证书](/cn/certificate-management/)。
如果您还没有数字证书，可以从`Let's Encrypt`申请免费的数字证书，或者让Janusec生成一张自签名的数字证书（自签名证书仅用于测试用途），或者跳过证书配置，在应用管理中配置使用`ACME自动证书`。  

## 4 配置Web应用 (必选)
----
使用浏览器打开  http://`您的网关IP地址`/janusec-admin/ 并[添加一个应用](/cn/application-management/).  
填写应用名称、实际服务器的 `IP:端口` 等信息。

## 5 修改DNS或Hosts (必选)
----

生产环境，需要修改DNS将您的域名指向网关地址。  
测试环境，可直接修改您本地电脑的hosts文件：  `C:\Windows\System32\drivers\etc\hosts` 。   
如需使用ACME自动化证书，域名必须为互联网可访问的真实域名，否则证书颁发机构回调验证时不通过。  

## 6 验证
----
配置完成后，验证网关是否正常工作。  
打开浏览器，访问：
http://`your_domain_name`/   
或   
https://`your_domain_name`/ .  
如果可以正常访问，表明网关已正常工作。  

## 7 WAF验证
----
检验WAF（Web应用防火墙）是否工作正常。
可使用如下测试用例:  

> `http://your_domain_name/.svn/entries`   
> `http://your_domain_name/test?id=1 and 1=1`  

阻断效果:   
![WAF](/images/waf2.png "WAF of Janusec Application Gateway")  

## 8 防火墙验证  
----

可通过：  

> #nft list table inet janusec  

或者：  

> #nft list table inet janusec -a  

查看应用网关相关的规则，大概是这样的：  
```
[root@CentOS8]# nft list table inet janusec -a
table inet janusec { # handle 20
	set blocklist { # handle 2
		type ipv4_addr
		flags timeout
	}

	chain input { # handle 1
		type filter hook input priority 0; policy accept;
		@nh,96,32 @blocklist drop # handle 3
	}
}

```

如需测试CC防御效果，请先检查WAF管理中的CC防护规则。  