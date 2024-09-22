---
title : "Lab 8 - Use Route Analyzer to validate the routing on TGW"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---
### Objectives
- In this lab, we will use the AWS Network Manager's Route Analyzer feature to troubleshoot the connectivity issue between the Dev and Prod VPCs.

{{% notice note %}}
Disassociate Network Firewall Rule Group from Firewall Policy:
Before proceeding with this module, we need to disassociate the rule group from the firewall policy. Please refer to the following steps.
{{% /notice %}}

{{% notice note %}}
In the underlying infrastructure, an AWS Network Firewall with a TCP rule set to deny all traffic from any source to any destination on port 443 was deployed to complete Lab 6 of the VPC Reachability Analyzer. We need to disable this rule for this lab as we will need to access the EC2 Instance to check the connectivity between the Prod and Dev instances.
{{% /notice %}}

### Disassociate the firewall rule group from the Firewall Policy.
1. Select the VPC service
![F1](/images/1/F1.png)
2. Here, select **Firewall policies**, check the `Inspection-Firewall-Policy` page
![F2](/images/1/F2.png)
3. Here, find **stateful rule groups** and check `TCP-Alert-Rule-Group`, select "Actions" and choose "Disassociate from policy".
![F3](/images/1/F3.png)
![F4](/images/1/F4.png)

### Route Analysis
1. Select the VPC service
![F1](/images/1/F1.png)
2. Go to **Network Manager**, select **Global Networks** and select the path ID here.
![F5](/images/1/F5.png)
3. Select the Global network that we created in Lab 7 of this module and click on Transit Gateway Network
![F6](/images/1/F6.png)
7. Select **Route Analyzer**, we are troubleshooting the connectivity issue between the Dev and Prod VPCs.
![F7](/images/1/F7.png)
8. Find the private IPs of the Dev - Prod at the EC2 instance.
![F8](/images/1/F8.png)
![F9](/images/1/F9.png)
8. Then enter the private IPs, check the **Middlebox Appliance** box and select **Run Route Analysis**
![F10](/images/1/F10.png)
9. The connection failure is detected, and we see the cause. There is a blackhole route added for the Prod VPC CIDR in the Transit Gateway Route Table.
![F11](/images/1/F11.png)
10. Go to the Transit Gateway Route Table of the Dev VPC and delete the blackhole route to fix the issue.
![F12](/images/1/F12.png)
![F13](/images/1/F13.png)
11. Go back and run **Route Analysis** again.
![F14](/images/1/F14.png)
{{% notice note %}}
This time, the blackhole route warning will disappear, but the status will still be Not Connected. Since the traffic between the Dev and Prod VPCs is inspected by the AWS Network Firewall in the Inspection VPC, we need to enable the middlebox appliance.
{{% /notice %}}
![F15](/images/1/F15.png)
12. After enabling the middlebox appliance:
- Run "Route analysis" again, this time it will show the entire path as connected in both directions. You can log in to the Dev or Prod Instance via Session Manager and check the connection using the "Ping" command.
![F16](/images/1/F16.png)
{{% notice note %}}
To log in to the Instance using Session Manager, you need to stop and start the instance.
{{% /notice %}}

**Summary**: Lab 8 completed.
- In this lab, we used the Route Analyzer to troubleshoot the connectivity issue between the Prod VPC and Dev VPC through the TGW and Inspection VPC.