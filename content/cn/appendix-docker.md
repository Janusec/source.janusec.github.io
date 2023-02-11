---
title: "附录4: Docker部署"
keywords: "JANUSEC应用网关, Docker"
description: "在容器中部署JANUSEC应用网关"
date: 2023-01-24T11:58:32+08:00
draft: false
weight: 1600
---

# 在容器中部署JANUSEC应用网关  
----

JANUSEC应用网关**首选直接部署在Linux服务器上**，监听80/443端口，可以作为K8S Ingress Controller将外部用户请求转发到K8S Pods（各Pod监听同一个端口）。如您采用直接部署的模式，可忽略以下内容，查看[快速入门](/cn/quick-start/) 。   

如您希望在容器内部署JANUSEC应用网关，请继续阅读下面的内容。  

## 前提条件

请先检查docker服务是否已启用（预期结果为 enabled）：

> systemctl status docker  

如果返回结果为 disabled ，请先启用：

> systemctl enable docker  
 

## 第一步：下载镜像  

> docker pull registry.cn-shenzhen.aliyuncs.com/janusec/janusec:1.3.1   


## 第二步：运行  

> docker run -d --privileged=true --restart=always -p 80:80 -p 443:443 -p 9080:9080 -p 9443:9443 registry.cn-shenzhen.aliyuncs.com/janusec/janusec:1.3.1 /sbin/init   

## 第三步：使用浏览器访问登录并修改口令  

http://IP_Address:9080/janusec-admin/   

登录用户名： `admin`   
口令： `J@nusec123` （登录后会提示修改）  

请确保此容器到后端服务的网络可达。  


## 早期版本容器升级（可选）   

如果您已经安装了之前的版本，可参考如下步骤升级，以下假设容器ID为 XXXXXX（请根据实际ID修改） ：  

复制更新包到容器：  

> docker cp ./janusec-1.x.x-amd64.tar.gz XXXXXX:/tmp/   

调出容器控制台：

> docker exec -it XXXXXX /bin/bash   

接下来在容器内升级：

> cd /tmp/  
> tar -zxf janusec-1.x.x-amd64.tar.gz  
> cd janusec-1.x.x  
> ./install.sh   （输入1继续）   
> systemctl restart janusec  

检查服务状态：

> systemctl status janusec  

无误后，退出镜像。

可将容器提交为新镜像（该步骤可选）：  

> docker commit -m "1.3.1" XXXXXX janusec/janusec:1.3.1   

