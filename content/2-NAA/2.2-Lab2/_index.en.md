---
title : "Lab 2 - Network segmentation via AWS Transit Gateway"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---
### Objectives
- Corporate networks can span multiple VPCs through VPC Peering or Transit Gateway.
- In this lab, we will create a custom network access scope to test network segmentation via AWS Transit Gateway and verify that the Prod and Dev VPCs are isolated from each other. Next, we will verify if the Prod VPC can communicate with the inspection VPC.

1. In "Network Access Analyzer", select "Create Network Access Scope"
![A8](/images/1/A8.png)
2. In "Create Network Access Scope", select **Empty Template**, click **Next**
![A9](/images/1/A9.png)
3. Provide Name and Description
![A10](/images/1/A10.png)
4. Select **Add match condition.**
- Follow the steps in the image:
![A11](/images/1/A11.png)
5. Select **Next**, then select **Create Network Access Scope**
6. Select the newly created **Network Access Scope** and select **Analyze**
![A12](/images/1/A12.png)
7. After completion, we can see that there are no findings, confirming that the newly created VPCs - Prod and Dev are segmented and traffic cannot be routed between them.
![A13](/images/1/A13.png)

**Next, let's verify if the Dev VPC can communicate through the Network Firewall by modifying the network access scope and re-analyzing it.**

1. Select **Actions** and choose **Duplicate and modify**
![A14](/images/1/A14.png)
2. Provide Name and Description
![A15](/images/1/A15.png)
3. In **Destination**, change **Prod VPC TGW Attachment** to **Inspection VPC TGW Attachment**.
![A16](/images/1/A16.png)
4. Select "Duplicate and analyze Network Access Scope"
![A17](/images/1/A17.png)
5. We will see that findings are detected, as expected. This is because resources in your Dev VPC route traffic through the inspection VPC containing the network firewall.
![A18](/images/1/A18.png)

**Summary**: Lab 2 completed.
- In this lab, we performed the following steps:
  - Created a custom network access scope: Created a custom network access scope to analyze VPC segmentation.
  - Verified VPC isolation: Confirmed that the Prod and Dev VPCs are isolated and cannot communicate with each other.
  - Duplicated and modified the network access scope: Duplicated and adjusted the network access scope to re-analyze traffic paths from the Prod VPC to the Inspection VPC.
  - Verified valid connectivity: The results showed that the Prod VPC can communicate with the Inspection VPC, which is expected in this case.
{{% notice note %}}
If we discover unintended paths between two VPCs that should not communicate with each other, this will be flagged as a finding (i.e., non-compliance), and appropriate actions will need to be taken to address this issue.
{{% /notice %}}