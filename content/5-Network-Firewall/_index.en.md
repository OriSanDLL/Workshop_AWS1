---
title : "Track: AWS Network Firewall (IPv6)"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
### Lab Overview:
Your company has decided that all IPv6 traffic will be inspected by AWS Network Firewall. Additionally, all IPv4 traffic will be inspected by a third-party open-source firewall (based on Suricata), running behind a Gateway Load Balancer (deployed into a separate VPC). Labs E and F will focus on configuring AWS Network Firewall with IPv6.
- [Lab E:](5.1-LabE/_index.en.md) will configure AWS Network Firewall to inspect IPv6 traffic entering and leaving the VPC.
- [Lab F:](5.2-LabF/_index.en.md) will include tasks related to creating stateless and stateful rules in AWS Network Firewall.