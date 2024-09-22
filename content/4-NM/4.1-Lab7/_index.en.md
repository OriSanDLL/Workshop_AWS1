---
title : "Lab 7 - Create and visualize the Global Network"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---
### Objectives
- Visualize VPC connectivity: See how VPCs are attached to the Transit Gateway and how they interact with each other.
- Expand visualization: Explore the visualization capabilities of other TGW attachments such as AWS Direct Connect, Connect attachments, and Peering attachments.

This tool provides an overview of the network architecture and the connections between resources in the AWS environment.
### Enable AWS Network Manager
1. Select the VPC service
![D1](/images/1/D1.png)
2. In VPC, select "Network Manager", select "Global Network" under **Connectivity** and choose "Create Global Network".
![D2](/images/1/D2.png)
3. Here, enter *Name*: `Global-Network-Connectivity-Tin` and *Description*: `Global Network Connectivity Tin`, select "Next".
![D3](/images/1/D3.png)
4. In **Include core network**, uncheck the box and select "Next".
![D4](/images/1/D4.png)
5. In **Review**, select "Create Global Network".
![D5](/images/1/D5.png)
6. Successfully created! and access the ID of the Global Network.
![D6](/images/1/D6.png)
7. Now, select **Transit Gateway** and start by selecting **Register transit gateway**
![D7](/images/1/D7.png)
8. Check the ID of the **Transit Gateway** and select **Register transit gateway**
![D8](/images/1/D8.png)
9. After the Transit Gateway is ready, follow these steps:
- Click on "Transit Gateways" from the left-hand panel.
- Open the previously registered Transit Gateway.
- Visualize the topology view of your network. You will see a diagram illustrating the connection structure between resources.
- Check all VPC attachments to ensure that the connections between the Transit Gateway and the VPCs are functioning correctly.
- This way, you can clearly identify the connections and the status of each VPC attachment in your network.
![D9](/images/1/D9.png)
10. Next:
- Select Transit Gateway Network to view the network structure visually.
- In the next lab, you will use the Route Analyzer to understand how routing works on the Transit Gateway (TGW).
![D10](/images/1/D10.png)

**Summary**: Lab 7 completed.
- In this lab, we performed the following steps:
  + Created a global network: Created a global network in AWS.
  + Registered the Transit Gateway: Registered the Transit Gateway into the global network.
  + Visualized the network structure: Used Network Manager to visualize the network topology and check connectivity between resources.
{{% notice note %}}
This process provides an overview of the network architecture and connections, helping to manage and optimize your network more effectively.
{{% /notice %}}