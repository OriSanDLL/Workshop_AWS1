---
title : "Lab 1 - Set up Amazon VPC Network Access Analyzer and check Internet accessibility"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---
### Objectives
- In this lab, we will use the Amazon VPC Network Access Analyzer to identify resources in your environment that can be accessed from internet gateways and verify that they are limited to resources that have a legitimate need to be accessible from the internet.

### Enable Amazon VPC Network Access Analyzer
1. Select the `VPC` service
![A1](/images/1/A1.png)
2. In "VPC", scroll down and select "Network Manager"
![A2](/images/1/A2.png)
3. In "Network Manager", select "Network Access Analyzer"
![A3](/images/1/A3.png)
4. In "Network Access Analyzer", select "Get started"
![A4](/images/1/A4.png)
5. Here you will see 4 pre-created scopes
![A5](/images/1/A5.png)
6. To analyze all ingress paths into your VPC, click on the network access scope ID **AWS-VPC-Ingress**. Click **Analyze**.
![A6](/images/1/A6.png)
7. Scroll down to explore the findings after the analysis is complete.
- Each finding displays the network path from the Internet through the internet gateway to resources in AWS (e.g., EC2 instances). This is where you will review each finding and flag/take action on those findings/paths that are not intended for internet access.
![A7](/images/1/A7.png)
8. Select the first finding by clicking the radio button, and in the right pane, you can click on the resource to view additional information about the specific resource.
![A7](/images/1/A7.png)

**Summary**: Lab 1 completed.
- In this lab, we performed the following steps:
  + Enabled Amazon VPC Network Access Analyzer: Activated this feature to analyze network traffic paths.
  + Analyzed ingress traffic: Checked paths from the internet gateway using the default Amazon network access scope.
{{% notice note %}}
This process helps us identify network configurations that could lead to unintended access to resources in the individual's VPC.
{{% /notice %}}