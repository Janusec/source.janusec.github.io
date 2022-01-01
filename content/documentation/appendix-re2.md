---
title: "Appendix 1: RE2 Regex"
keywords: "RE2 regex, Janusec, Application Gateway, WAF, Web Application Firewall"
description: "Janusec Application Gateway RE2 regular expression"
date: 2020-04-09T14:16:34+08:00
draft: false
weight: 1300
---

# Appendix 1: RE2 Regex
---

JANUSEC Application Gateway uses [Google RE2 Regular Expression](https://github.com/google/re2/wiki/Syntax) 。   

#### Regex Rule Example 1

Checkpoint: URLPath  
Description: checkpoint `URLPath` represents the path after the domain name in the URL address, for example www.yourdomain.com`/blog/show.php`?id=1&category=2 ，URLPath is `/blog/show.php`    

RE2 Rule:   

> (?i)/\\.(git|svn)/  

Description: `(?i)` represents case insensitive，`/` represents itself, `\\.` matches the decimal point，(git&#124;svn) matches git or svn, used to block access to the wrongly released source code.      

#### Regex Rule Example 2

Checkpoint: URLQuery  
Description: checkpoint `URLQuery` represents the parameters in URL (example: www.yourdomain.com/blog/show.php?`id=1&category=2` , URLQuery is `id=1&category=2` )  

RE2 Rule:   

> (?i)%\s+(and|or)\s+   

Description: `%` matches itself, `\s+` matches one or more spaces, `(and|or)` matches various case combinations such as `aNd`, `AnD`, `oR` etc., used to prevent SQL Injection.    

### Regex Rule Example 3

Checkpoint: GetPostValue  
Description:  `GetPostValue` represents parameter values in GET and POST methods (example: www.yourdomain.com/blog/show.php?id=1&category=2 ,GetPostValue is `[1, 2]` ).    

RE2 Rule:   

> (?i)\s+(and|or)\s+[\w\p{L}]+=[\w\p{L}]+$  

Description: [\w\p{L}] matches any letter, number, underscore or Unicode character (such as Chinese characters), `=` matches itself, `$` matches the end, used to prevent SQL Injection.      
`\x{FFFF}` matches UNICODE, example `[\x{007F}-\x{FFFF}]+` matches unicode words.   
