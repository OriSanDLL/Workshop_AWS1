---
title : "Track: DNS Security"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 3. </b> "
---
### Lab Overview:

- [**Lab A**](3.1-LabA/_index.en.md): You have received a report from a third party about suspicious activity coming from a part of your infrastructure. Your first task is to enable DNS query logging to capture these requests - this will help identify both the source and destination of the queries.

- [**Lab B**](3.2-LabB/_index.en.md): After identifying requests from your VPC to a suspicious website, the next task is to block it. You will look at how Amazon Route 53 DNS Firewall can prevent resources in the VPC from resolving the IP address of this suspicious domain.
{{% notice note %}}
Note: Occasionally, you may see the AWS console revert to the US East (N Virginia) region, especially when working with Route 53; make sure to switch back to the US West (Oregon) region (or the region you are instructed to work in).
{{% /notice %}}