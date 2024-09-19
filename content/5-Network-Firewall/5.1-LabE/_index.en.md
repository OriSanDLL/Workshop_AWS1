---
title : "Lab E: Setting the routing for AWS Network Firewall"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---
### Objective
- You need to configure a firewall to inspect all suspicious IPv6 traffic entering and leaving the LabVpc network due to reports of unusual activity.

### Starting Point
- You need to configure the route tables to ensure that traffic entering and leaving the VPC passes through the AWS Network Firewall endpoints in the FwSubnets for inspection.

### Infrastructure
- AWS Network Firewall (with endpoints in each AZ).
- LabVpc/FwSubnet1 & LabVpc/FwSubnet2 (one in each AZ).

### Services Used
Amazon VPC - Route Tables [Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
AWS Network Firewall [Documentation](https://docs.aws.amazon.com/network-firewall/latest/developerguide/what-is-aws-network-firewall.html)

### Success Criteria
- Configure the IPv6 route tables for the VPC to ensure that all ingress/egress traffic is inspected by AWS Network Firewall.
- Ensure that the firewall endpoints are correctly associated with the VPC and relevant subnets.
- Verify internet access from publicInstance after updating the route tables.

### Tips
**Working with IPv6**

If you haven't worked with IPv6 before, here are a few things to remember:
- The default route for IPv6 is ::/0 (equivalent to 0.0.0.0/0 for IPv4).
- When allocating an IPv6 CIDR block to a VPC, the VPC receives a /56 public IPv6 address block; each subnet receives a /64 allocation from this block.

**Working with VPC routing**
- To configure routing in the VPC, think about each hop of the packet from source to destination.
- The next hop is determined by the routes in the subnet's route table.
- Ask AWS staff if you encounter difficulties.

**Ingress Route table**
Configure the Ingress Route table [Ingress Routing](https://aws.amazon.com/blogs/aws/new-vpc-ingress-routing-simplifying-integration-of-third-party-appliances/)

**Lab E Architecture**
![E1](/images/structure/E1.png)

[Lab E Guide](5.1.1-WE/_index.en.md)