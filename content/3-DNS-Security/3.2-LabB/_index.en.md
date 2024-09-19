---
title : "Lab B: Implementing a DNS firewall"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---
### Overview:
1. **Identify the suspicious website**: "bad.sa-demos.net" is the target of much traffic.
2. **Next task**: Block this website to prevent access while continuing the investigation.
3. Solution: **Deploy and configure mechanisms** to prevent instances in the VPC from resolving that domain name.
    
The goal is to temporarily block access to the suspicious domain from the VPC to ensure safety during the investigation.

### Starting Point:
1. There are several approaches to **deny access** to the suspicious domain, such as **VPC security groups** or **network ACLs**, but they only block traffic to specific IP addresses.
2. If the malicious website switches to **another IP address**, these blocks will become ineffective.
3. Instead, in this lab, we will use a **DNS firewall** to prevent resources in the VPC from resolving the IP address of the suspicious domain.
    
The goal is to use the DNS firewall to effectively block the domain name, regardless of IP changes.

### Services Used
- Amazon Route 53 Resolver DNS Firewall - [Documentation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-dns-firewall.html)
- Amazon CloudWatch Log Insights - [Documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)

### Success Criteria
- Instances in LabVpc can no longer successfully resolve the IP address of the suspicious domain you identified in Lab A (`bad.sa-demos.net`). The [Lab B CloudWatch Dashboard](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabB) will show your progress.
- Note that attempts to query the suspicious domain will return an **NXDOMAIN** response.

### Tips
**To enable and configure the DNS firewall**
The DNS firewall can be found in the [Amazon Route 53 Resolver](https://console.aws.amazon.com/vpc/home#DNSFirewallRuleGroups) section of the AWS console. The process is:

1. Create a [domain list](https://console.aws.amazon.com/vpc/home#DNSFirewallDomainLists:) containing the suspicious domain bad.sa-demos.net.
2. Create a [rule group](https://console.aws.amazon.com/vpc/home#DNSFirewallRuleGroups) to BLOCK (using NXDOMAIN response) requests to the domain list you created above.
3. Associate the rule group with LabVpc.

**To analyze query logs**
You can check if DNS requests are being blocked in several ways:
1. Through [CloudWatch Log Insights](https://console.aws.amazon.com/cloudwatch/home#logsV2:logs-insights).
2. Connect to `publicInstance` and use `nslookup bad.sa-demos.net.` [Lab B CloudWatch Dashboard](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabB).

**Lab B Architecture**
![B1](/images/structure/B1.png)

[**Lab B Guide**](3.2.1-WB/_index.en.md)