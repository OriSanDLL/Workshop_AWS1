---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
### What is VPC Network Access Analyzer?
- Network Access Analyzer is a feature that helps identify unintended network access to your resources on AWS:
1. Understand, verify, and improve your network security posture:
   + Network Access Analyzer helps you identify unintended network access against your security and compliance requirements, allowing you to take steps to improve network security.
2. Demonstrate compliance:
   + You can use Network Access Analyzer to demonstrate that your network on AWS meets certain compliance requirements.
3. Verify your network security posture:
   + Network Access Analyzer allows you to specify network security requirements and identify potential network paths that do not meet those requirements.

### Why do you need Network Access Analyzer?
1. Problem:
- As organizations expand their cloud environments and distributed application teams begin to operate network infrastructure, it becomes difficult to determine whether there are adequate network controls in place to achieve the organization's security and compliance goals.
2. Validate network controls:
- Customers struggle to validate network controls because they have to manually test and audit network designs and configurations, which is not scalable and time-consuming.
- Operations and network teams need to demonstrate the presence of network controls for security requirements, especially during external audits, making the process tedious and inefficient.
3. Network controls quickly become outdated:
- Due to the ever-changing customer environment with evolving security requirements, changes in network design, and the continuous launch of new AWS network services.
4. Operations teams try to ensure a compliant environment by restricting developers from using certain AWS services or applying manual approval processes for configuration changes. Both methods put pressure on the operations team and make development teams feel they cannot move as quickly as desired.

### What is VPC Reachability Analyzer?
- Reachability Analyzer is a configuration analysis tool that allows you to perform connectivity tests between a source resource and a destination resource within your virtual private clouds (VPCs).
- Key functions:
  + Connectivity testing: When the destination resource is reachable, Reachability Analyzer provides step-by-step details of the virtual network path between the source and destination.
  + Identify issues: If the destination resource is not reachable, Reachability Analyzer identifies the component blocking it. These issues can be due to misconfigurations in security groups, network ACLs, route tables, or load balancers.

### Architecture Diagram
This architecture is provided through pre-configured AWS CloudFormation and will be used in the lab.
![image](/images/1/NetworkAccessAnalyzer.png)