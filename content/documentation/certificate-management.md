---
title: "Certificate Management"
keywords: "Certificate Management, Janusec Application Gateway, Encryption"
description: "Certificate Management of Janusec Application Gateway"
date: 2018-05-20T21:05:08+08:00
draft: false
weight: 500
---

# Certificate Management
----

Scenario: You can add and maintain the digital certificate you already have here (for example, the certificate of a corporate website).    

If you don't have a digital certificate and want JANUSEC to automatically apply for and manage a digital certificate, you can skip this section and do not need to configure it here (usually applicable to personal websites).  

#### Certificate List  
Open web administration portal and navigate to `Certificate Management`.  
![Certificate List](/images/certificate1.png "Certificate Management of Janusec Application Gateway")

#### Add or Edit Certificate  
Single domain `subdomain.yourdomain.com` or wildcard  `*.yourdomain.com` certificate are all acceptable. A wildcard certificate is preferred.  

![Certificate Edit](/images/certificate2.png "Edit Certificate of Janusec Application Gateway")

For production environment, you should have a legal digital certificate issued by a trusted CA ( such as `Let's Encrypt` ) .

For test purpose, when filled in the first field (`Common Name or Subject Alternative Name`), click `Self-Sign Certificate` button, it will produce a self-sign certificate for you, and you need export it with browser and add it to the trusted root CA.  

#### Certificate Protection
> Private key is encrypted and stored in the database.   
> Different encryption keys used for each instance.   


#### ACME Automatic Certificate Description

If you want JANUSEC to automatically apply for and manage digital certificates, you don't need to add them in the `Certificate Management` module, but choose to use `ACME Automatic Certificate` when configuring the domain name under the menu `Application Management`.  

The above domain name needs to be pointed to the JANUSEC application gateway, which is used by the certificate authority to verify the ownership of the domain name.  