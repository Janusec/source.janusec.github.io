---
title: "附录3: 高可用配置"
keywords: "高可用, 高可用配置, Janusec, Application Gateway, WAF, Web Application Firewall"
description: "Janusec Application Gateway 高可用配置"
date: 2020-04-09T14:16:34+08:00
draft: false
weight: 1500
---

# 附录3: 高可用配置
---

## 1 架构说明

JANUSEC应用网关本身支持安装一个主节点和多个副本节点，用于负载均衡。  
如果需要保障单个节点的高可用性，可配合keepalived来构建高可用节点。  
建议的高可用部署架构为：  

1. 部署一个主节点，专门用于后台管理，不对用户开放  
2. 部署两个或多个副本节点（Replica），在每个副本节点上安装keepalived   

以下采用两个副本节点，假设IP地址分别为`192.168.56.101`和`192.168.56.102`，使用keepalived来构建一个虚拟节点，让域名指向虚拟节点的IP地址`192.168.56.103` 。  

需要注意的是，在同一时刻，一组keepalived节点中只有一台网关服务器在工作，如果这一台出现故障，则自动迁移到另一台服务器。   
如果您需要多个虚拟节点同时工作，请创建一组新的keepalived网关节点。  


## 2 安装keepalived

以Debian为例，安装keepalived可使用如下指令：  

> apt install keepalived  

## 3 配置keepalived  

从v1.2.8版本开始，在JANUSEC安装目录`/usr/local/janusec/`下面提供了keepalived的配置文件`keepalived.conf`，可修改这个文件。  

* `vrrp_instance VI_01`部分的`interface eth0`，将`eth0`修改为实际使用的内部网卡名称，通常为`eth0`、`eth1`、`enp0s8`等，IP地址对应上面的`192.168.56.101`或`192.168.56.102`  

* `virtual_ipaddress`部分的虚拟IP地址为`192.168.56.103` （根据实际需要修改）  

修改完毕后，将`keepalived.conf`复制到`/etc/keepalived/`目录下，然后启动keepalived服务:   

> systemctl restart keepalived   

备注： 两台服务器均需要安装keepalived。  

## 4 验证

查看keepalived服务是否正常运行：  

> systemctl status keepalived   

分别检查两台服务器的IP地址：   

> ip addr show `eth0`    

eth0跟上述配置文件中的interface一致。  
如果配置成功，在其中一台服务器上可以看到虚拟IP已成功添加。  

1: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000  
    link/ether 08:00:27:6d:69:45 brd ff:ff:ff:ff:ff:ff  
    inet 192.168.56.101/24 brd 192.168.56.255 scope global eth0  
       valid_lft forever preferred_lft forever  
    `inet 192.168.56.103/32 scope global eth0`  
       valid_lft forever preferred_lft forever  
    inet6 fe80::a00:27ff:fe6d:6945/64 scope link   
       valid_lft forever preferred_lft forever  


然后可以停掉这一台服务器上面的网关服务来模拟网关故障：   

> systemctl stop janusec   

然后重新查看两台服务器的IP地址，如果工作正常，则`192.168.56.103`会迁移到另一台服务器。  

测试完毕后，启动手工停掉的网关服务：  

> systemctl start janusec  
