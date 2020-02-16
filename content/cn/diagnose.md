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

* CentOS 7  
* RHEL 7  
* Debian 9  

### 服务管理工具

服务管理工具为systemd，检查方法，在命令行运行： 

> `command -v systemctl`   

（预期结果为：/usr/bin/systemctl） 


### PostgreSQL

使用`psql`命令来检查配置是否正常：  

> psql -h `127.0.0.1` -U `janusec` -W `janusec`  

参数h后面跟IP地址，参数U后面跟数据库用户名，参数W表示接下来需要输入口令，最后是数据库名。  
如果登录验证不成功，可参考 [运维管理](/cn/operation-management) 中的PostgreSQL安装部分。

在PSQL Shell中执行版本检查：  

> select version();  

版本要求为`9.3`以上（不支持PostgreSQL 9.2或更早的版本）。  

### 端口

> `netstat -anp | grep LISTEN | grep ':\(80\|443\)\s'`  

Janusec网关需要使用80/443端口，如果在与Web服务器同一台主机上安装Janusec且有其它程序占用了这些端口，需要其它程序修改端口。  

### DNS

如果Janusec网关和后面的Web服务器位于同一台主机，则DNS不需要任何修改。  
如果是单独部署的Janusec网关，需要将现有的Web服务接入迁移到Janusec，请先使用hosts方式调试通过，然后再修改DNS指向。 

### 主从节点同步

为了保证主从节点正确同步，需要满足：  

* 各节点时间正确（误差不超过一分钟）  
* 从节点`node_key`跟Web管理控制台中[节点管理](/cn/node-management)中显示的`node_key`一致。  
  
  
  

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

开发者所需要的Golang版本最低要求为Go 1.12+ 。  

### 代码获取

> `go get -u github.com/Janusec/janusec`  

不推荐使用git clone方式。



