---
title : "Using Suricata to inspect traffic content"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 5.2.4 </b> "
---
### **Detailed Instructions**
- Rules: AWS Network Firewall uses two engines (stateless and stateful) to inspect packets.
- Note: Ensure there is a stateless rule to forward traffic to the stateful engine.

#### Alert based on content header User Agent
1. In the left menu, go to Firewall policies under Network firewall. You will find NFWPolicy, created as part of the initial setup. Click on it and go to Stateful rule groups, click on Actions -> Create stateful rule group.
![F19](/images/structure/F19.png)
2. Select Suricata compatible rule string, and click Next.
![F20](/images/structure/F20.png)
3. Enter the following values:
- **Name**: Suricata-Detect-User-Agent
- **Capacity**: 100
- Add under **Suricata compatible rule string**
    ```markdown
    drop http $HOME_NET any -> $EXTERNAL_NET any (msg:"ET USER_AGENTS Suspicious User-Agent (IE/1.0)"; flow:established,to_server; threshold: type limit, count 2, track by_src, seconds 300; http.user_agent; content:"IE/1.0"; sid:333;)
    ```
![F21](/images/structure/F21.png)

#### Testing
![F22](/images/structure/F22.png)
- Since we have configured the rule to *drop traffic, this request will time out.