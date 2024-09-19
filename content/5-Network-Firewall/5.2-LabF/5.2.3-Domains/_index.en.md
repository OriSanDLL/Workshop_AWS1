---
title : "Drop specific Domains"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 5.2.3 </b> "
---
### **Detailed Instructions**
- AWS Network Firewall: Uses two rule engines to inspect packets.
- Task: Create a Stateless rule to forward all traffic to the Stateful engine.
![F13](/images/structure/F13.png)
1. Go to the VPC Console and in the left menu, select Firewall policies under Network firewall. You will find an NFWPolicy, created in the initial setup. Click on it and go to Stateless rule groups, click on Actions -> Create stateless rule group.
2. In step 1 - click Next. In step 2, enter the following values:
- **Name**: FwdToStateful
- **Capacity**: 100
- Click Next
- Under Add rule
  + **Priority**: 3
  + **Protocol**: Leave All selected
  + **Source**: Any IPv6 address
  + **Destination**: Any IPv6 address scroll down
  + **Rule action**: Forward to stateful rule groups
- Ensure DropPingRule has priority 1, DropDNS has priority 2, and FwDtoStateful has priority 3.
![F14](/images/structure/F14.png)
3. Scroll down to the Stateful rule groups section and click on Actions -> Create stateful rule group.
![F15](/images/structure/F15.png)
4. Select Domain list as the rule group format
![F16](/images/structure/F16.png)
5. In step 2, enter the name and Capacity = 100.
6. In step 3, complete as follows:
    - Domain: [www.amazon.com](http://www.amazon.com/)
    - CIDR Range: Default
    - Protocol: HTTP and HTTPS
    - Action: Deny
    - Click **Next**
![F17](/images/structure/F17.png)
- You will see 3 Stateless rules and 1 Stateful rule.
![F18](/images/structure/F18.png)
- This will not return any responses.