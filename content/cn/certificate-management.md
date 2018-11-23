---
title: "证书管理"
keywords: "证书管理, Janusec应用网关"
description: "Janusec应用网关的证书管理"
date: 2018-05-20T21:05:08+08:00
draft: false
weight: 500
---

# 证书管理
----

#### 证书列表  
在Web管理界面，导航到 `Certificate Management` （证书管理） .  
![证书列表](/images/certificate1.png "Janusec应用网关的证书管理")

#### 添加或编辑证书   
这里可使用单域名证书（`subdomain.yourdomain.com`）或通配符证书（`*.yourdomain.com`），推荐使用通配符证书。   

![编辑数字证书](/images/certificate2.png "编辑数字证书")

生产环境，请使用权威第三方CA颁发的证书。   
测试环境，可使用Janusec自签发证书(在`Common Name or Subject Alternative Name`字段输入通配域名之后，可点击`Self-Sign Certificate`按钮)，不过自签证书不受信任，需要手工从浏览器导出，并导入到可信根CA颁发机构中。  


#### 证书保护
> 证书私钥，加密存储在数据库中.    
> 不同的部署实例，加密密钥是不同的.    
