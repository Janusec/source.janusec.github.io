---
title: "Appendix 6: Cookie Compliance"
keywords: "Cookie Compliance, Cookie Banner"
description: "Cookie Compliance Management through Janusec Application Gateway"
date: 2023-07-16T14:18:32+08:00
draft: false
weight: 1800
---

# Cookie Compliance        
----

## 1 Introduction to Cookie Compliance    

<br>

Cookies are a typical technology for achieving statistical analysis, personalized recommendations, and personalized marketing/advertising.   

They are also subject to personal data protection (such as GDPR) and privacy protection laws (such as ePrivacy Directive/Regulation), and supervisions are becoming increasingly strict.  

In areas where relevant laws apply, directly implanting unnecessary cookies into a user's terminal device without obtaining their explicit consent is suspected of infringing on user privacy.   

The professional plus edition of Janussec Application Gateway provides cookie compliance management functionality, which is not included in the open source version.
   
## 2 Enable Cookie Banner  

<br>

In `Application Management`, open the application that needs to enable cookie management, check `Enable Cookie Compliance Management` and save it.   

At this time, two new tab configuration pages will be added to the configuration interface, namely `Application Cookie Settings` and `Application Cookie Management` .  

Among them, `Application Cookie Settings` can customize the display content of Cookie Banner, `Application Cookie Management` categorizes specific cookie fields to determine whether they are allowed to be set to the user's terminal devices.   

## 3 Cookie Scanning  

The administrator uses their own user side computer to access the target website and enable `Unclassified Cookies` in the Cookie Banner pop-up window for scanning. The steps are as follows:   

1. User Side: Please ensure that the domain name points to the primary node of the gateway.  
2. User Side: Open the target website using a browser. If the Cookie Preferences window does not appear, please click on the Cookie icon in the bottom left corner of the window.  
3. User Side: Enable Unclassified Cookies and Confirm the Choice in the Cookie Preference window, and navigate pages which may set cookies.  
4. Gateway Side: Click the refresh button above to view the list of recognized cookies.  
5. Gateway Side: Check or modify each cookie type until the number of unclassified cookies is reset to zero  
6. User Side: Disable Unclassified Cookies if the number of unclassified cookies becomes zero.  
7. User Side: Navigate the target website and check if the cookie settings are in effect.  

## 4 Cookie Cleaning   

In the `Application Cookie Management` interface, over time, due to business changes and hackers' scanning behavior, the number of cookies to be classified will increase.   
Most of them may come from hacker scanning behavior, which can be directly deleted.  
If it is a real cookie from the business, classify it correctly.  


## 5 Manage Cookie References  

`Cookie References` used for classification automatically.    

You can include common cookies, such as various statistical analysis plugins.   




