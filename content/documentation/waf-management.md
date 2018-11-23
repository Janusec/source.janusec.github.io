---
title: "WAF Management"
keywords: "WAF, Web Application Firewall"
description: "WAF Management of Janusec Application Gateway"
date: 2018-05-21T21:27:20+08:00
draft: false
weight: 530
---

# WAF Management
----

#### Add or Edit WAF Policy
![Add or Edit WAF Policy](/images/waf1.png "Add or Edit WAF Policy for Janusec Application Gateway")   

#### Typical Check Points  
> Example: `http://www.yourdomain.com/blog/show.php?id=1&category=2`   

URLPath: `/blog/show.php`  
URLQuery: `id=1&category=2`  
GetPostKey: [`id`, `category`]   
GetPostValue: [`1`, `2`]   

> GetPostKey, GetPostValue used for both GET and POST method   
> If you want to check url values only ( GET Only ), please select URLQuery .  


#### Regular Expression  
Janusec Application Gateway adopts [Google RE2 Regular Expression](https://github.com/google/re2/wiki/Syntax ) . In order to simplify configuration, Janusec Application Gateway will preprocess the values to be detected. Typically, remove `'` and `"` , replace `/**/` by white space etc. Example:   

Regex: 

> `(?i)\s+(and|or)\s+[\w\p{L}]+=[\w\p{L}]+$`   

will cover these values:   

> `1' aNd '1'='1`   
> `abc' oR "abc"="abc`   
> `1'/**/And/**/'a'='a`   

Note:   

>  `p{L}` used for unicode character.   



#### Action  
----
##### Block 
![Block Information of Janusec Application Gateway](/images/waf2.png)  

##### CAPTCHA
Usually used for CC attacks or frequently requests.   

![Captcha of Janusec Application Gateway](/images/captcha.png)   
