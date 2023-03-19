---
title: "附录5: 数据发现"
keywords: "JANUSEC应用网关, 数据发现"
description: "通过JANUSEC应用网关实现数据发现"
date: 2023-03-19T20:11:32+08:00
draft: false
weight: 1700
---

# 通过JANUSEC应用网关实现数据发现    
----

## 数据发现功能简介  

在全局设置中，启用数据发现功能时，应用网关将检查请求和响应中的字段值，当命中数据发现规则时，就将其报告给JANUCAT（Compliance, Accountability and Transparency），用于数据隐私治理。   

## 数据发现报告配置   

命中数据发现规则时，通过API报告给JANUCAT，为了验证发送方的身份，需要API Key。  

API格式通常类似这样：  

> `http://127.0.0.1:8088`/api/v1/data-discoveries  

需根据JANUCAT实际部署发布的情况进行修改。  
数据发现报告使用POST请求（当使用GET方式打开该链接时，会展示API接口用法）。    

API Key (API密钥)用于验证发送方的身份，可请JANUCAT管理员提供（需要登录到JANUCAT后台才能看到并复制）。  

## 数据发现规则  

点击“管理数据发现规则”按钮，可对数据发现的规则进行编辑处理。  

数据发现规则使用Google RE2正则表达式，对如下内容类型的请求及响应中的JSON值进行检测：   

> Content-Type: application/json  

检测手机号（假设是以1开头的11位数字）的正则表达式可写为：  

> ^1\d{10}$  


## 数据发现结果  

数据发现上报的结果，需要登录JANUCAT查看。  
