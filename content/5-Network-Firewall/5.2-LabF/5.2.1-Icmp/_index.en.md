---
title : "Block outbound ICMP"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.2.1 </b> "
---
### **Detailed Instructions**
AWS Network Firewall uses two rules engines to inspect packets:
- **Stateless Engine:**
  + Inspects packets based on stateless rules.
  + Actions: Can drop the packet, allow it to reach its destination, or forward it to the Stateful Engine.
- **Stateful Engine:**
  + Inspects packets in the context of the traffic flow, based on stateful rules.
  + Actions: Can drop the packet or allow it to reach its destination.
  + Logging: Sends traffic and alert logs to firewall logs if logging is configured. Can send alerts for dropped packets and optionally for allowed packets.
![F1](/images/structure/F1.png)

#### Configure Stateless Rule to Drop All ICMP Traffic
1. Go to VPC:
- Navigate to Firewall Policies:
  + In the AWS Management Console, open the VPC service.
- Select Firewall Policies:
  + In the left menu, select Firewall policies under Network firewall.
- Select NFWPolicy:
  + Find and select the NFWPolicy created in the initial setup.
- Create Stateless Rule Group:
  + In the Stateless rule groups tab, click on Actions.
  + Select Create stateless rule group.
![F2](/images/structure/F2.png)
2. Enter the following values:
- **Name**: DropICMPv6
- **Capacity**: 100
![F3](/images/structure/F3.png)
Under Add rule
- **Protocol**: Choose IPv6-ICMP and delete All
- **Source**: Any IPv6 address
- **Destination**: Any IPv6 address scroll down
- **Actions**: Drop
Click Add rule
![F4](/images/structure/F4.png)
{{% notice note %}}
Before clicking Next, make sure you see the newly created rule in the Rules section.
{{% /notice %}}
Click Next for steps 4, 5 and Create rule group for step 5.

### Testing
**Remote Access via Session Manager**
![F5](/images/structure/F5.png)
![F6](/images/structure/F6.png)


    ```markdown
    ping6 tools.keycdn.com
    ```

![F7](/images/structure/F7.png)
**It will not return any responses.**