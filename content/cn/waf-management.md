---
title: "WAF管理"
keywords: "WAF, Web Application Firewall, Web应用防火墙"
description: "WAF管理 of Janusec应用网关"
date: 2018-05-21T21:27:20+08:00
draft: false
weight: 530
---

# WAF管理
----

#### 添加或编辑WAF策略
![添加或编辑WAF策略](/images/waf1.png "Janusec应用网关，添加或编辑WAF策略")   

#### 典型的检查点  
> 举例: `http://www.yourdomain.com/blog/show.php?id=1&category=2`   

URLPath: `/blog/show.php`  
URLQuery: `id=1&category=2`  
GetPostKey: [`id`, `category`]   
GetPostValue: [`1`, `2`]   

> GetPostKey, GetPostValue ： 同时作用于GET和POST方法     
> 如果仅检查GET方法，请使用 URLQuery  .  


#### 正则表达式  
Janusec应用网关采用 [Google RE2 正则表达式](https://github.com/google/re2/wiki/Syntax ) .  
为简化正则表达式配置，Janusec应用网关对待检测的字符串值进行了预处理：  

* 删除  `'` 及  `"`    
* 替换 `/**/` 为空格   


正则举例: 

> `(?i)\s+(and|or)\s+[\w\p{L}]+=[\w\p{L}]+$`   

可覆盖如下值:   

> `1' aNd '1'='1`   
> `abc' oR "abc"="abc`   
> `1'/**/And/**/'a'='a`   

备注:   

>  `p{L}` 用于UNICODE字符.    



#### 动作  
----
##### Block（阻断） 
![Janusec应用网关阻断恶意请求](/images/waf2.png)  

##### CAPTCHA（验证码）
用于CC攻击或高频访问   
![Janusec应用网关验证码](/images/captcha.png)   
