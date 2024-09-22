---
title : "Lab 5 - Troubleshoot the connectivity issue between App server and Database Server"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---
### Objectives
- In this lab, we will use the VPC Reachability Analyzer to troubleshoot the connectivity issue between the application and the database.

### Prerequisites
- Find the **Network Interface** ID for the **database-server**

1. Use the EC2 service.
![B3](/images/1/B3.png)
2. Scroll down, select "Network Interfaces" under "Network & Security" in EC2.
![B1](/images/1/B1.png)
3. Search for type **RDS** and select **RDSNetworkInterface.**
4. Note the **Network Interface ID**
![B2](/images/1/B2.png)

### Create and Analyze Path
1. Select the VPC service
![B4](/images/1/B4.png)
2. In VPC, select "Network Manager".
3. Here, select "Reachability Analyzer" and choose "Create and analyze path"
![B5](/images/1/B5.png)
4. Enter the following information
![B6](/images/1/B6.png)
![B7](/images/1/B7.png)
5. Select *Destination type*: **Network Interfaces** and *Destination*: enter the **Network Interface ID** as **RDSNetworkInterface** noted earlier.
![B8](/images/1/B8.png)
6. Enter *Destination port*: `5432` and select "Create and analyze path"
![B9](/images/1/B9.png)
7. After creation, we check and see that the **Reachability status** is Not reachable.
![B10](/images/1/B10.png)
8. In the "Explanations" section, select the arrow next to "Details" to check detailed information about the component or combination of components blocking the path.
(For example: in the following explanation, because the security group does not have the appropriate inbound rule, traffic from external sources cannot reach the destination resource, resulting in a blocked network connection.)
![B11](/images/1/B11.png)
9. Scroll down and check the **Path details** section to see a representation of the shortest path between the *Source* and *Destination*.
![B12](/images/1/B12.png)

### Troubleshooting
1. Click on the address provided under **Explanations**.
![B13](/images/1/B13.png)
2. After selecting the address, it will navigate to the **Security Group** page of EC2. Select **Outbound Rules** and choose **Edit Outbound rules**.
![B14](/images/1/B14.png)
3. Click "Add rule" and select Type as `Custom TCP`, Port range as `5432`, Destination IP address range as `10.1.0.0/16` and click **Save rules**.
![B15](/images/1/B15.png)
4. After saving, go back to the **Reachability Analyzer** page and select **Analyze path**, then select **Confirm**.
![B16](/images/1/B16.png)
![B17](/images/1/B17.png)
5. We see that the **Reachability status** is `Reachable`.
![B18](/images/1/B18.png)

**Summary**: Lab 5 completed.
- In this lab, we learned how to create an analysis in the VPC Reachability Analyzer and how this tool helps in identifying connectivity issues between two AWS resources.
- Key points:
  - Create analysis: Use VPC Reachability Analyzer to check connectivity between two resources, such as EC2 instances or VPCs.
  - Identify connectivity issues: The tool helps detect issues such as:
    - Security group rules blocking traffic.
    - Incorrect route table configurations.
    - Other network components that may impede connectivity.
  - Provide detailed information: VPC Reachability Analyzer provides clear information about traffic paths and related components, making it easier to diagnose and troubleshoot issues.
{{% notice note %}}
This tool is very useful in maintaining the availability and performance of AWS services.
{{% /notice %}}