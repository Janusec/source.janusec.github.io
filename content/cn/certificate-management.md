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

适用场景：已经拥有的数字证书，可在这里添加并维护（比如企业网站的证书）。  

如果您没有数字证书，希望JANUSEC自动申请并管理数字证书，可跳过这一节，不用在这里配置（通常适用于个人网站）。    

#### 证书列表  
在Web管理界面，导航到 `Certificate Management` （证书管理） .  
![证书列表](/images/certificate1.png "Janusec应用网关的证书管理")

#### 添加或编辑证书   
这里可使用单域名证书（`subdomain.yourdomain.com`）或通配符证书（`*.yourdomain.com`）。   

![编辑数字证书](/images/certificate2.png "编辑数字证书")

生产环境，请使用权威第三方CA颁发的证书。   
测试环境，可使用Janusec自签发证书(在`Common Name or Subject Alternative Name`字段输入通配域名之后，可点击`Self-Sign Certificate`按钮)，不过自签证书不受信任，需要手工从浏览器导出，并导入到可信根CA颁发机构中。  


#### 证书保护

通过证书管理录入的证书私钥，会加密存储在数据库中，并且对于不同的部署实例，加密密钥是不同的。    

#### ACME自动化证书说明  

如果您希望JANUSEC自动申请并管理数字证书，不用在`证书管理`模块添加，而是在`应用管理`的域名配置时，选择使用`ACME自动证书`。 

上述域名需要已经指向JANUSEC应用网关，用于证书颁发机构验证域名的所有权。  