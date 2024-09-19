---
title : "Lab C: Analysing VPC Network Configuration"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---
### Objective
- After detecting suspicious DNS traffic in LabVpc, we need to check the security configuration to identify which VPC resources can receive connections from the internet. The goal is to ensure these resources meet compliance and security requirements. If unwanted access is detected, we will need to remove it.

### Starting Point
- AWS provides tools to help us identify and visualize allowed access to our VPC based on the current configuration, instead of manually checking each security group and Network ACL configuration in large environments.

### Services Used
- Amazon VPC Network Access Analyzer - [Documentation](https://docs.aws.amazon.com/vpc/latest/network-access-analyzer/what-is-network-access-analyzer.html)

### Success Criteria
- Identify VPC access controls that allow unwanted connections (e.g., SSH port open to the Internet) into the VPC and remove this access.

### Tips
**To find insights about network access configuration**
- Network Access Analyzer can be found [in the Network Manager console](https://us-west-2.signin.aws.amazon.com/oauth?client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fnetworkmanager&code_challenge=gJlDcvz1ZMOURphAAzCNh3LUQ2lp84w54jZpfTNc1FI&code_challenge_method=SHA-256&response_type=code&redirect_uri=https%3A%2F%2Fus-west-2.console.aws.amazon.com%2Fnetworkmanager%2Fhome%3FhashArgs%3D%2523%252F%26isauthcode%3Dtrue%26oauthStart%3D1726669137896%26state%3DhashArgsFromTB_us-west-2_12ab578f6b8aae3b).
  + While you can use AWS's default scope, you may get many results unrelated to the lab. Consider how you can narrow the scope to LabVpc.
  + If you want a long-term goal, also consider how you can exclude "known good" traffic from the analysis.
{{% notice note %}}
Note that (as of November 2023), Network Access Analyzer only supports IPv4 analysis.
{{% /notice %}}
**Lab C Architecture**
![C1](/images/structure/C1.png)

[Lab C Guide](4.1.1-WC/_index.en.md)