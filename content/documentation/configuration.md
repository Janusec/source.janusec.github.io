---
title: "Configuration File"
keywords: "Configuration, Janusec, WAF"
description: "Configuration of Janusec Application Gateway"
date: 2020-03-22T10:11:09+08:00
draft: false
weight: 350
---

# Configuration File
----

#### Path of Configuration File
----
Production environment:  `/usr/local/janusec/config.json`  
Development environment: `./config.json`

#### Configuration Items
----

The following is based on Janusec Application Gateway V0.9.7, and use `//` as comment, please delete `// comment` before using it.

```
{
    "node_role": "master",            // "master" for master node, "slave" for slave nodes
    "master_node": {                  // keep empty for slave nodes
        "admin": {                    // Administrator portal
            "listen": true,           // Listen on new ports for admin portal
            "listen_http": ":9080",   // Format :port or IP:Port，when listen is true, http://IP:9080/janusec-admin/ is available
            "listen_https": ":9443",  // Format :port or IP:Port，when listen is true, https://any_application_domain:9443/janusec-admin/ is available
            "portal": "https://gate.janusec.com:9443/janusec-admin/",   // admin portal, used for OAuth callback, if listen is false, remove colon and port number
            "webssh_enabled": false   // Web SSH Operation permitted when it is true
        },
        "database": {                 // PostgreSQL 9.3+
            "host": "127.0.0.1",      // PostgreSQL IP Address
            "port": "5432",           // PostgreSQL Port, 5432
            "user": "postgres",       // PostgreSQL user
            "password": "123456",     // PostgreSQL password, less than 32bit
            "dbname": "janusec"       // PostgreSQL database name
        },
        "oauth": {                    // OAuth2
            "enabled": false,         // true: Enable OAuth2 
            "provider": "wxwork",     // wxwork(WeChat Work)、dingtalk(DingTalk)、feishu(Feishu)
            "wxwork": {               // WeChat Work
                "display_name": "Login with WeChat Work",     
                // Only http/https and domain changable, don't use port number
                "callback": "https://your_domain.com/oauth/wxwork",  
                // Get form https://work.weixin.qq.com/wework_admin/frame#profile
                "corpid": "wwd03be1f8",  
                // Create Application "JANUSEC" at https://work.weixin.qq.com/wework_admin/frame#apps 
                "agentid": "1000002",  
                // Secret                             
                "corpsecret": "BgZtz_hssdZV5em-AyGhOgLlm18rU_NdZI"  
                // Note：Authorized Callback domian should be configured. 
            },
            "dingtalk": {             // DingTalk
                "display_name": "Login with DingTalk", 
                "callback": "https://your_domain.com/oauth/dingtalk", 
                "appid": "dingoa8xvc",
                "appsecret": "crrALdXUIj4T0zBekYh4u9sU_T1GZT"
            },
            "feishu": {
                "display_name": "Login with Feishu",
                "callback": "https://your_domain.com/oauth/feishu",
                "appid": "cli_9ef21d00e",
                "appsecret": "ihUBspRAG1PtNdDLUZ"
                // Create application JANUSEC is required
                // "Secure Domain"-"Redirect URL" is required, example: "https://your_domain.com/oauth/feishu" 
            }
        }
    },
    "slave_node": {      // for slave nodes
        // copy from the node management
        "node_key": "",  
        // If listen is true, IP:Port is required.
        // If https is required, it need a seperate domain for master node, and an empty applicaiton should be configured, destination may be 127.0.0.1:9999 which not used.
        "sync_addr": "http://gateway.master_node.com:9080/janusec-admin/api"
    }
}
```
