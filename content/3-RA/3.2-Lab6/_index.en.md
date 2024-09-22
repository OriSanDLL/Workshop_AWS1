---
title : "Lab 6 - Troubleshoot the connectivity issue between Prod EC2 instance in Prod VPC and Public IP address"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---
### Objectives
- In this lab, we will use the Amazon VPC Reachability Analyzer to troubleshoot and fix the connectivity issue between the Prod EC2 instance and the public IP address blocked by the AWS Network Firewall.

### Create and Analyze Path
1. Select the VPC service
![B4](/images/1/B4.png)
2. In VPC, select "Network Manager", then select "Reachability Analyzer" and choose "Create and analyze path".
![C1](/images/1/C1.png)
3. Enter *Name tag*: `EC2-Public-IP-via-ANF-Tin`, select *Source type*: `Instances` and choose *Source*: `Prod Instance`.
![C2](/images/1/C2.png)
4. Select *Destination type*: `IP Address`, *Destination address*: `55.3.0.1`.
![C3](/images/1/C3.png)
5. In *Additional packet header configuration at destination*, enter *Destination port*: `443`.
![C4](/images/1/C4.png)
6. Click "Create and analyze path".
![C5](/images/1/C5.png)
7. After creation, we check and see that the **Reachability status** is Not reachable.
![C6](/images/1/C6.png)
8. In the "Explanations" section, select the arrow next to "Details" to check detailed information about the component or combination of components blocking the path.
(For example: in the following explanation, because the AWS Network Firewall is configured to not allow traffic to the destination)
![C7](/images/1/C7.png)
9. Scroll down and check the **Path details** section to see a representation of the shortest path between the *Source* and *Destination*, along with an analysis of the components involved in the process.
![C8](/images/1/C8.png)

### Troubleshooting
1. Select the VPC service
![B4](/images/1/B4.png)
2. In VPC, select **Network Firewall rule groups**.
![C9](/images/1/C9.png)
3. Select *Your rule group* to view details.
![C10](/images/1/C10.png)
4. Navigate to Rules and select "Edit rules".
![C11](/images/1/C11.png)
5. Here, check the rule to be edited and select "Edit".
![C12](/images/1/C12.png)
6. In *Traffic direction*, check "Forward" and in *Action*, check "Alert" and select "Save".
![C13](/images/1/C13.png)
7. Select *Save rule group*.
![C14](/images/1/C14.png)

### Re-analyze Path
1. Go back to the **VPC Reachability Screen** and select "Analyze path".
![C15](/images/1/C15.png)
2. Select "Confirm".
![C16](/images/1/C16.png)
3. After confirmation, we check and see that the **Reachability status** is `Reachable`.
![C17](/images/1/C17.png)

**Summary**: Lab 6 completed.
- In this lab, we learned how the VPC Reachability Analyzer helps identify the ANS Firewall rules blocking network traffic.
- Key points:
  - Using VPC Reachability Analyzer: You performed an analysis to check the traffic path from a source to a destination.
  - Detecting blocking rules: The tool helped you identify which rule in the ANS Firewall was obstructing network traffic, making it easier to pinpoint the cause of the connectivity issue.
  - Troubleshooting: With detailed information from the VPC Reachability Analyzer, you could make the necessary adjustments to the firewall rules to restore valid connectivity.
{{% notice note %}}
This tool is very useful in ensuring that security rules do not hinder the operation of the network system.
{{% /notice %}}