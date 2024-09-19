---
title : "Drop DNS Traffic"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 5.2.2 </b> "
---
### **Detailed Instructions**
1. Go to the VPC console and in the left menu, go to Firewall policies under Network firewall. You will find an NFWPolicy, created as part of the initial setup. Click on it and go to the Stateless rule groups, then click on Actions -> Create stateless rule group.
![F8](/images/structure/F8.png)
2. In step 1 - click Next. In step 2, enter the following values:
- **Name**: DropDNS
- **Capacity**: 100
- Click Next
![F9](/images/structure/F9.png)
3. Under Stateless rule
- Protocol: 6 (TCP), 17 (UDP)
- Source: Any IPv6 address
- Source port range: Any port
- Destination port range: Custom, then enter 53
- Destination: Any IPv6 address
- Rule Action: Drop
{{% notice note %}}
Note: Before clicking next, make sure you can see the newly created rule.
{{% /notice %}}
Click Next for steps 4, 5 and Create rule group for step 5.

#### Testing
![F12](/images/structure/F12.png)