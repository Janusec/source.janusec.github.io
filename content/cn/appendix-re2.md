---
title: "附录1: RE2正则规则解读"
keywords: "RE2正则规则解读, Janusec, Application Gateway, WAF, Web Application Firewall"
description: "Janusec Application Gateway RE2正则规则解读"
date: 2020-04-09T14:16:34+08:00
draft: false
weight: 1300
---

# 附录1:RE2正则规则解读
---

JANUSEC应用网关使用[Google RE2正则规则](https://github.com/google/re2/wiki/Syntax) 。   

#### 规则样例1

检查点(CheckPoint)：URLPath  
解读：检查点URLPath表示URL地址中域名后面的路径(举例 www.yourdomain.com/blog/show.php?id=1&category=2 ，URLPath 为`/blog/show.php` )   

RE2规则：  

> (?i)/\\.(git|svn)/  

解读：(?i)表示不区分大小写，/原样匹配，\\.匹配小数点，(git&#124;svn)表示匹配git或svn ，用于阻断访问错误发布的源代码。    

#### 规则样例2

检查点(CheckPoint)：URLQuery  
解读：检查点URLQuery表示URL中的参数(举例 www.yourdomain.com/blog/show.php?id=1&category=2 ，URLQuery 为 `id=1&category=2` )  

RE2规则：  

> (?i)%\s+(and|or)\s+   

解读：%原样匹配，\s+表示一个或多个空格，(and|or)匹配and或or的各种大小写组合（如aNd、AnD、oR等），用于防止SQL注入。  

### 规则样例3

检查点(CheckPoint)：GetPostValue  
解读：GetPostValue同时作用于GET和POST方法中的参数值(举例 www.yourdomain.com/blog/show.php?id=1&category=2 ，GetPostValue 为参数值 `[1, 2]` )。  

RE2规则：  

> (?i)\s+(and|or)\s+[\w\p{L}]+=[\w\p{L}]+$  

解读：[\w\p{L}]表示任意字母、数字、下划线或Unicode字符（如汉字），=原样匹配，$表示结尾，用于防止SQL注入。   
中文或其他UNICODE字符，还可以使用`\x{FFFF}`格式，比如`[\x{007F}-\x{FFFF}]+`可匹配中文词语或句子。  