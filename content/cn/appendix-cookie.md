---
title: "附录6: Cookie合规"
keywords: "JANUSEC应用网关, Cookie合规, Cookie Banner"
description: "通过JANUSEC应用网关实现Cookie合规管理"
date: 2023-07-16T14:18:32+08:00
draft: false
weight: 1800
---

# Cookie合规      
----

## 1 Cookie合规简介    

<br>

Cookie是实现统计分析、个性化推荐、个性化营销/广告的典型技术，同时适用个人数据保护（如GDPR）和隐私保护相关法律（如ePrivacy Directive/Regulation），监管日趋严格。  

在相关法律适用地区，在没有获得用户的明示同意之前，直接将非必要Cookie植入用户的终端设备涉嫌侵犯用户隐私。     

Janusec Application Gateway 的增强体验版提供了Cookie合规管理功能，而开源版本不含此功能。   
   
## 2 Cookie Banner 启用  

<br>

在`应用管理`中，打开需启用Cookie管理的应用，勾选`启用Cookie合规管理`并保存，此时配置界面会新增两个Tab配置页面，分别为`应用Cookie设置`和`应用Cookie管理` 。  

其中，`应用Cookie设置` 可以定制Cookie Banner的展示内容； `应用Cookie管理` 对具体Cookie字段进行分类，用于决定是否允许设置到用户的终端设备。    

## 3 Cookie扫描  

管理员使用自己的用户侧计算机，访问目标网站，在Cookie Banner弹窗中启用`未分类Cookie`进行扫描，步骤如下：  

1. 用户侧：请确保域名指向网关的主节点  
2. 用户侧：用浏览器打开目标网站，如果没有出现Cookie偏好设置窗口，请点击窗口左下角的Cookie图标  
3. 用户侧：在Cookie偏好设置窗口，启用未分类Cookie并确认，然后浏览可能设置Cookie的页面  
4. 网关侧：点击上方刷新按钮，查看识别出的Cookie清单  
5. 网关侧：检查或修改每一Cookie类型，直至未分类Cookie数量清零  
6. 用户侧：如果未分类Cookie已清零，在Cookie偏好设置窗口禁用未分类Cookie  
7. 用户侧：浏览目标网站，检查Cookie设置是否生效  

## 4 Cookie清理    

在 `应用Cookie管理` 界面，随着时间的增加，由于业务的变更、黑客的扫描行为，待分类Cookie会增加，其中大部分可能来自于黑客的扫描行为，这部分可以直接删除。  
如果是来自业务的真实Cookie，则将其正确分类。  

## 5 管理Cookie引用  

`Cookie引用`的作用是对Cookie进行自动分类。  

您可以将常见的Cookie纳入管理，比如各种统计分析插件。  



