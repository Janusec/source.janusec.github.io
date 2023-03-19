---
title: "Appendix 5: Data Discovery"
keywords: "Data Discovery"
description: "Implementing data discovery on the janusec application gateway"
date: 2023-03-19T20:11:32+08:00
draft: false
weight: 1700
---

# Data Discovery    
----

## Data Discovery Introduction    

In global settings, when the Data Discovery function is enabled, the application gateway will check the field values in the request and response.   
When a Data Discovery rule is hit, it will report it to JANUCAT (Compliance, Accountability and Transparency) for data privacy governance.   

## Data Discovery Report   

When a Data Discovery rule is hit, it will be reported to JANUCAT through the API.   
In order to verify the identity of the sender, an API Key is required.

The format of API is like this:   

> `http://127.0.0.1:8088`/api/v1/data-discoveries  

Modifications need to be made based on the actual deployment and release of JANUCAT.  
Data Discovery reports use POST requests (API interface usage is shown when the link is opened using GET mode).
The API Key is used to verify the identity of the sender and can be provided by the JANUCAT administrator (log in to the JANUCAT background to see and copy it).

## Data Discovery Rules  

Click the button of "Manage Data Discovery Rules", and then edit them.   

Google RE2 Regular Expression is used, for JSON value in the request and response of this content type:     

> Content-Type: application/json  

For example, detect a 11-bit phone number begin with number 1, it may be:    

> ^1\d{10}$  


## Data Discovery Results  

The results reported by Data Discovery need to be viewed by logging in to JANUCAT.  

