---
title: "Appendix 4: Docker Deployment"
keywords: "JANUSEC Application Gateway, Docker"
description: "Deploy JANUSEC Application Gateway in Docker Container"
date: 2023-01-24T11:58:32+08:00
draft: false
weight: 1600
---

# Deploy JANUSEC Application Gateway in Docker Container  
----

The JANUSEC application gateway is **preferred to be deployed directly on the Linux server** and listens on port 80/443. It can be used as the K8S Ingress Controller to forward external user requests to K8S Pods (each Pod listens on the same port). If you adopt the direct deployment mode, you can ignore the following content and see [Quick Start](/cn/quick-start/).  

If you want to deploy JANUSEC application gateway inside docker container, please continue to read the following content, for experiential purposes only, not recommended for production environments.   


## Prerequisite  

Please check whether the Docker service is enabled (the expected result is enabled).  

> systemctl status docker  

If the return result is disabled, please enable it： 

> systemctl enable docker  
 

## Step 1：Download image 

> docker pull janusec/janusec:1.3.1   


## Step 2：Run    

> docker run -d --privileged=true --restart=always -p 80:80 -p 443:443 -p 9080:9080 -p 9443:9443 janusec/janusec:1.3.1 /sbin/init   

## Step 3：Access the Management Entrance of the Gateway  

http://IP_Address:9080/janusec-admin/   

username： `admin`   
Password： `J@nusec123` （Prompt for modification after login）  

Please ensure that the network from this container to the back-end service is reachable.   


## Update from earlier version（optional）   

If you have installed the previous version, you can refer to the following steps to upgrade it.    
The following assumes that the container ID is XXXXXX (please modify according to the actual ID).  

Copy update package to container:  

> docker cp ./janusec-1.x.x-amd64.tar.gz XXXXXX:/tmp/   

Log in to the console in the container：  

> docker exec -it XXXXXX /bin/bash   

Update it within the container：  

> cd /tmp/  
> tar -zxf janusec-1.x.x-amd64.tar.gz  
> cd janusec-1.x.x  
> ./install.sh   （Input `1` to continue）   
> systemctl restart janusec  

Check the service status：

> systemctl status janusec  

If OK then exit the container.

You can commit the container as a new image (optional)：  

> docker commit -m "1.3.1" XXXXXX janusec/janusec:1.3.1   

