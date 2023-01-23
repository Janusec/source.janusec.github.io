---
title: "Application Management"
keywords: "Janusec Application Gateway, WAF, Web Application Firewall"
description: "Application Management of Janusec Application Gateway"
date: 2018-05-20T21:58:35+08:00
draft: false
weight: 510
---

# Application Management
----

#### 1 Add or Edit Application  

Open web administration portal and navigate to `Application Management`.  
![Add or Edit an Application](/images/application1.png "Application Management of Janusec Application Gateway")   

#### 2 Scheme (HTTP/HTTPS)   

Backend scheme (From **Application Gateway** to **Backend Server**) is usually in intranet, select `http` by default, digital certiface is not required.    

Note：   

* Scheme From **User's Web Browser** to **Application Gateway**  is usually `HTTPS` (digital certificate is required), by ticking the following "Redirect HTTP to HTTPS". If you have a certificate already, you can import it at [Certificate Management](/documentation/certificate-management) . If you want JANUSEC to apply for a free digital certificate on its own, you can choose to use the 'ACME Automated Certificate' (free certificate) in the `Domain Configuration` below, and ensure that the real DNS of the domain name has pointed to the JANUSEC Application Gateway, which is used by the certification authority to verify the ownership of the domain name. ACME certificates are managed by file system,  and not need to be imported under `Certificate Management` (note: automatic certificates do not support the use of wildcard domain names; automatic certificates do not support multi-node deployment; automatic certificates do not support local hosts domain names).   


#### 3 Route  

Four route modes are supported:     
* Reverse_Proxy: Default mode, backend service(s) is/are required (Format `IP:Port`, the port number cannot be omitted), the gateway will forward the request to back-end service(s)；   
* Local_FastCGI: for PHP or Python, backend server is not required；    
* Static_WebSite: Application Gateway works as a Web Server directly, for static content, backend server is not required, default file name such as `index.html` and the path to static content should be provided；  
* K8S_Ingress: K8S Ingress Controller, K8S Pods API (such as http://127.0.0.1:8080/api/v1/namespaces/default/pods ) and Pod Port (such as 80) are required, the gateway obtains the Pods address list through this API and forwards the HTTP request to backend Pods.    

Note:  If the backend servers or K8S Pods are not running HTTP services, you can use the ** Port Forwarding **  to forward through Layer-4 TCP/UDP (layer-4 forwarding does not have security functions such as WAF/CC).       


#### 4 IP Address  

Application Gateway uses `REMOTE_ADDR` to acquire user's IP address by default.     

Note：

* `REMOTE_ADDR` means to obtain the IP address directly from the IP message. The actual IP address obtained is the IP address of the last hop.      
* `X-Forwarded-For` indicates that the IP address is obtained from the 'X-Forwarded-For' field in the HTTP request header. The actual IP address obtained is the user IP address passed from the previous hop (possibly forged);
* JANUSEC Application Gateway will attach IP address obtained by `REMOTE_ADDR` to the Header `X-Forwarded-For` of the HTTP request;
* When users directly access the gateway, the gateway should use `REMOTE_ ADDR` gets users' IP address. If the back-end business needs to process the user IP address, it can extract the last IP address in the HTTP header `X-Forwarded-For`;
* When the user accesses the gateway after passing through the CDN, the gateway should select the appropriate method to obtain the user's real IP address according to the CDN manufacturer's instructions (usually select `X-Forwarded-For`).   
