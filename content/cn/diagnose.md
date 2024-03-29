---
title: "问题诊断"
keywords: "问题诊断, Janusec, Application Gateway, WAF, Web Application Firewall"
description: "安装Janusec Application Gateway时遇到问题怎么排除"
date: 2018-08-05T10:16:34+08:00
draft: false
weight: 1200
---

# 问题诊断
---

## 部署
---

当安装Jausec时遇到问题，可先对照此检查表进行检查。

### 操作系统

操作系统需要为x86_64架构的如下操作系统之一：  

* Debian 9/10/11+  (首选Debian 10/11+)   
* CentOS 7/8+  
* RHEL 7/8+  


### 时间

服务器时间请先校准，时区不限，但需要保证时间正确，误差在一分钟之内。  

### 服务管理工具

服务管理工具为systemd，检查方法，在命令行运行： 

> `command -v systemctl`   

（预期结果为：/usr/bin/systemctl） 

### 配置文件

参见[配置文件](/cn/configuration/)  

### PostgreSQL

使用`psql`命令来检查配置是否正常：  

> psql -h `127.0.0.1` -U `janusec` -W `janusec`  

参数h后面跟IP地址，参数U后面跟数据库用户名，参数W表示接下来需要输入口令，最后是数据库名。  
如果登录验证不成功，可参考 [运维管理](/cn/operation-management) 中的PostgreSQL安装部分。

在PSQL Shell中执行版本检查：  

> select version();  

版本要求为`10`以上（不支持早于PostgreSQL 9.6的版本）。  

> show SERVER_ENCODING;  

要求数据库编码为UTF8 。  

### 端口

> `netstat -anp | grep LISTEN | grep ':\(80\|443\)\s'`  

Janusec网关需要使用80/443端口，如果在与Web服务器同一台主机上安装Janusec且有其它程序占用了这些端口，需要其它程序修改端口。  

当config.json中listen=true时，网关还需要使用TCP 9080/9443端口，一般用于从内网发起管理。  

> `netstat -anp | grep LISTEN | grep ':\(9080\|9443\)\s'`  

当启用GSLB和DNS服务器时，网关还需要使用TCP/UDP 53端口，请确认端口是否被占用，防火墙策略是否开通：    

> `netstat -anp | grep ':53\s'`  

### DNS

DNS需要指向网关的IP地址。应用迁移时，可以先使用hosts方式将域名指向网关，调试通过后再修改DNS指向。   

如需使用GSLB，可考虑切换到增强体验版本。   

### 证书  

如果使用ACME自动证书，则要求：   

* 对应的域名是互联网用户可访问的（不能是hosts中的临时测试域名），用于证书颁发机构回调验证    
* 只能使用单节点   


### 主副本节点同步

为了保证主副本节点正确同步，需要满足：  

* 各节点时间正确（误差不超过一分钟）  
* 副本节点`node_key`跟Web管理控制台中[节点管理](/cn/node-management)中显示的`node_key`一致（不同时间看到的结果不同，不影响同步）。  

### 日志

日志文件路径为 `/usr/local/janusec/log/` ，可查看日志中是否存在错误输出，特别是`error.log`。   

### nftables防火墙

请确保nftables防火墙正常工作，且没有多余的规则影响JANUSEC，参考[安装](/cn/installation/)一节。  
JANUSEC启动后，正常情况下规则类似如下： 
 
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

如果测试过程中被误封IP，可清除防火墙规则（后续仍会正常触发）:   

> nft flush ruleset   

或者在防火墙的CC防护配置界面，降低锁定时间。  
  
### 更多错误信息  

如果上述检查都没有问题，可停止Janusec服务，改由控制台运行，以便观察更多的输出信息：  

> #`systemctl stop janusec`  
> #`cd /usr/local/janusec`  
> #`./janusec`  

如果发现有错误输出，可通过QQ群（776900157）反馈。  
  

## 开发

---

备注：以下内容仅供开发者阅读，直接下载二进制安装包的用户不需要阅读。

### 操作系统

开发者操作系统不限，但建议采用Linux系统。  
如果使用不在发布范围内的操作系统（Windows等），则只能在控制台调试，不能保障安装使用。
发布脚本（release.sh）暂仅支持Linux。  

### PostgreSQL

开发所需要的PostgreSQL跟上述部署要求相同，配置文件使用当前目录的`./config.json` (部署使用的配置文件为`/usr/local/janusec/config.json`) 。

### Golang版本

开发者所需要的Golang版本最低要求为Go 1.16+ 。  

### 代码获取

> git clone https://github.com/Janusec/janusec.git  




