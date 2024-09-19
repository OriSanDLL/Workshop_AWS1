---
title : "Lab E Guide: Setting the routing for AWS Network Firewall"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.1.1 </b> "
---
### Make sure you are in the correct region
1. Navigate to the [AWS Console homepage](https://console.aws.amazon.com/console/home).
2. Ensure you are working in the correct region.

### NFW routing for IPv6
- Objective: Ensure that all ingress and egress traffic is inspected by AWS Network Firewall before reaching public and private subnets.
- Work done: Configured a firewall subnet with AWS Network Firewall deployed.
- Next steps: Modify the IPv6 route tables to ensure all ingress and egress traffic passes through the Network Firewall.

**This is how the route tables will handle the configuration afterwards**
![E2](/images/structure/E2.png)
{{% notice note %}}
Note: When you add Network Firewall endpoints to a VPC, they appear as Gateway Load Balancer endpoints (technically, AWS Network Firewall uses Gateway Load Balancer). The CloudWatch dashboard will help you identify the correct endpoint IDs.
{{% /notice %}}

### Add default route via IGW to the FW route tables
1. In the AWS Management Console, start typing **VPC** into the quick search box and press **Enter**:
2. Go to VPC → Route Tables and filter by *LabVpc* in the left box.
3. Select **LabVpc/FwSubnet1RouteTable** and click on the Routes tab. Click on *Edit Routes*
![E3](/images/structure/E3.png)
4. Add a route and configure it so that all traffic must go through the internet gateway.
- **Destination:** ::/0
- **Target:** igw-xxxx..
![E4](/images/structure/E4.png)
5. Now, repeat steps 1 to 4 for the "LabVpc/FwSubnet2RouteTable".

### Create and Configure Edge Route Table
1. Go to VPC → Route Tables and click on Create route table in the top right corner.
- Name: LabVpc/Edge
- VPC: vpc-xxx (LabVpc)
- Click on Create route table.
![E5](/images/structure/E5.png)
2. Click on Edit on routes
![E6](/images/structure/E6.png)
3. Add 2 new routes
    - Destination: *Check CloudWatch Dashboard for Public Subnet CIDR (IPv6) in AZ1*
    - Target: *Check CloudWatch Dashboard for NFW Endpoint ID in AZ1*
    - Destination: *Check CloudWatch Dashboard for Public Subnet CIDR (IPv6) in AZ2*
    - Target: *Check CloudWatch Dashboard for NFW Endpoint ID in AZ2*
    
    As a best practice, routing from a subnet should go through the GWLB endpoint in the same Availability Zone (AZ). Go to the [Lab E CloudWatch Dashboard](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabE) to check the correct GWLB endpoint for each subnet.
    
    You will see two new routes with the VPC Network Firewall GWLB endpoint as the target.
![E7](/images/structure/E7.png)
4. On the Edge associations tab, click on **Edit edge association**
![E8](/images/structure/E8.png)
Then select Internet Gateway and **Save Changes**

### Modify Public subnets route tables
1. Go to VPC → Route Tables and filter by LabVpc.
2. Select LabVpc/PubSubnet1RouteTable and click on the Routes tab.
Click on Edit Routes.
![E9](/images/structure/E9.png)
3. We need to modify the ::/0 rule. Remove the IGW as the target and click on Gateway Load Balancer Endpoint.
- As a best practice, the selected endpoint needs to be in the same AZ as the subnet the route table is associated with. Go to the [Lab E CloudWatch Dashboard](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabE) to check the correct GWLB endpoint for each subnet.
![E10](/images/structure/E10.png)
4. Add a route and now configure it so that all traffic must go to the Network Firewall before leaving the VPC
- **Destination**: ::/0
- **Target**: vpce-xxxx
5. This is how your route table will look.
{{% notice note %}}
Please note that we are only configuring routes for IPv6, so IPv4 still points to the IGW, while IPv6 points to the NFW endpoint.
{{% /notice %}}
![E11](/images/structure/E11.png)
6. Now, repeat steps 1-4 for LabVpc/PubSubnet2RouteTable.

**Congratulations on completing Lab E!**