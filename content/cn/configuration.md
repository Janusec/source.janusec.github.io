---
title: "配置文件"
keywords: "配置文件, Janusec, 应用网关, WAF, Web应用防火墙"
description: "JANUSEC应用网关配置文件说明"
date: 2020-03-22T10:11:09+08:00
draft: false
weight: 350
---

# 配置文件
----

### 配置文件路径
----
生产环境配置文件为 `/usr/local/janusec/config.json`  
开发环境配置文件为 `./config.json`

### 配置项说明
----

以下为Janusec Application Gateway V1.0.0+ 配置文件参考。  
由于JSON格式支持的注释格式看起来不方便，下面采用`//`作为注释说明，实际使用时需要删除注释。

```
{
    "node_role": "primary",            // 单节点或主节点配置为"primary"，副本节点配置为"replica"
    "primary_node": {                  // 主节点使用此配置，如果是副本节点，这部分配置留空
        "admin": {                    // 后台管理
            "listen": true,           // 后台管理界面开启独立的监听端口，通常用于只允许内网登录，不允许外网登录
            "listen_http": ":9080",   // 格式为:port或IP:Port，listen为true时，允许后台管理通过 http://IP:9080/janusec-admin/ 访问
            "listen_https": ":9443",  // 格式为:port或IP:Port，listen为true时，允许后台管理通过 https://any_application_domain:9443/janusec-admin/ 访问
            "portal": "https://gate.janusec.com:9443/janusec-admin/",   // 管理入口地址，用于OAuth回调，如果listen为false，请去掉冒号和端口号
        },
        "database": {                 // PostgreSQL 10/11/12+  
            "host": "127.0.0.1",      // PostgreSQL IP地址
            "port": "5432",           // PostgreSQL 监听端口，默认5432
            "user": "postgres",       // 数据库用户名
            "password": "123456",     // 数据库口令，不超过32位，直接配置明文，Janusec会自动加密
            "dbname": "janusec"       // 数据库名
        }
    },
    "replica_node": {      // 副本节点配置
        // node_key在主节点的后台管理-节点管理中复制
        "node_key": "",  
        // 如果后台管理界面开启独立的监听端口(上面的listen为true)，则在IP或域名后面带上端口
        // 如果使用https和域名，需要单独为主节点申请一个域名，并配置一个独立的应用(Application)使用该域名，Destination可填写127.0.0.1:9999(实际不用)
        "sync_addr": "http://gateway.primary_node.com:9080/janusec-admin/api"
    }
}
```

### 升级说明
---

从版本1.0.0开始，OAuth身份认证配置迁移到Web管理界面，config.json文件中不再需要oauth字段。  
如果从0.9.X版本升级而来，config.json文件中的该字段不会被自动删除，虽不影响网关运行，但建议手工删除（连同它前面的逗号）。   
以下是0.9.X版本中的oauth配置参考：   


```
"oauth": {                    // OAuth2统一身份认证
            "enabled": false,         // 设置为true时，启用OAuth2统一身份认证
            "provider": "wxwork",     // 目前支持wxwork(企业微信)、dingtalk(钉钉)、feishu(飞书)、ldap(LDAP)、cas2(CAS Server)  
            "wxwork": {               // 企业微信配置
                // 登录界面显示
                "display_name": "Login with WeChat Work",     

                // callback中只修改http/https及域名，后面的路径不变，也不带端口号
                "callback": "https://your_domain.com/oauth/wxwork",  

                // 在https://work.weixin.qq.com/wework_admin/frame#profile 下方可看到企业ID信息
                "corpid": "wwd03be1f8",  

                // 在https://work.weixin.qq.com/wework_admin/frame#apps 创建名为JANUSEC的自建应用即可看到
                "agentid": "1000002",  

                // corpsecret即上述自建应用的Secret，请据实修改                             
                "corpsecret": "BgZtz_hssdZV5em-AyGhOgLlm18rU_NdZI"  

                // 备注：需要同时在该应用"开发者接口"一栏配置"授权回调域"，配置一个常用的应用域名作为网关的访问域名
            },
            "dingtalk": {             // 钉钉配置
                "display_name": "Login with DingTalk",  

                // callback中只修改http/https及域名，后面的路径不变，也不带端口号
                "callback": "https://your_domain.com/oauth/dingtalk", 

                // 需要在钉钉开放平台注册应用
                "appid": "dingoa8xvc",
                "appsecret": "crrALdXUIj4T0zBekYh4u9sU_T1GZT"
            },
            "feishu": {
                "display_name": "Login with Feishu",

                // callback中只修改http/https及域名，后面的路径不变，也不带端口号
                "callback": "https://your_domain.com/oauth/feishu",

                // 需要在飞书开放平台注册应用并经企业管理员审核通过(名称JANUSEC)
                // 需要在飞书开放平台后台配置"安全设置"-"重定向URL"，配置为"https://your_domain.com/oauth/feishu" 
                "appid": "cli_9ef21d00e",
                "appsecret": "ihUBspRAG1PtNdDLUZ"
                
            },
            "ldap": {
                // 显示在登录界面
                "display_name": "Login with LDAP",

                // 修改entrance,使用您的网关域名替换
                "entrance": "https://gate.janusec.com/ldap/login",

                // LDAP服务器地址，格式为 域名:端口  
                "address": "ldap.janusec.com:389",

                // {uid} 保持不变，其他根据实际修改
                "dn":"uid={uid},ou=People,dc=janusec,dc=com",

                // 是否启用加密传输
                "using_tls":false,

                // 是否启用Authenticator认证码双因子认证
                // 需要安装手机APP（Google Authenticator或Microsoft Authenticator）
                "authenticator_enabled": false
            },
            "cas2": {
                // 显示在登录界面
                "display_name": "Login with CAS 2.0",

                // CAS服务器入口，使用/cas结尾   
                "entrance": "https://cas_server/cas",

                // 回调地址，使用网关域名，以/oauth/cas2结尾，不带端口号
                "callback": "http://gate.janusec.com/oauth/cas2"
            }
        }
```