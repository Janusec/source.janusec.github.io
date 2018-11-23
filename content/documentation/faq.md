---
title: "FAQ"
keywords: "FAQ, Janusec Application Gateway"
description: "FAQ of Janusec Application Gateway"
date: 2018-05-12T07:45:49+08:00
draft: false
weight: 1000
---

# FAQ
----

##### Q: What is the difference between Janusec Application Gateway and WAF ?
> A: **Janusec Application Gateway** includes a WAF (Web Application Firewall), and Janusec eliminates the defects of traditional WAF.   

##### Q: Does it requires agent installation on business servers ?
> A: No agent required.  

##### Q: Does it support **https** ?
> A: Yes, **https** is naturally supported.  

##### Q: Is my private key secure ?
> A: The private key of certificate is encrypted and stored in the database, and only occurs in the memory of your gateway nodes.  

#### Q: How to get user's IP address from my business ?
> A: Janusec Application Gateway will add or append user's IP address to request header `X-Forwarded-For` .      

