---
title : "Lab H: Configuring Routing for Gateway Load Balancer"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 6.2 </b> "
---
### Objective
You need to configure routing in the Lab VPC to ensure all IPv4 traffic is directed to the Suricata firewall via the Gateway Load Balancer (GWLB) endpoints. This will allow Suricata to inspect all IPv4 traffic entering and leaving through the IGW. The firewall has been pre-configured to block all traffic to https://www.facebook.com over IPv4.

### Starting Point
- In the previous lab, you created a GWLB (Gateway Load Balancer) Endpoint service in the GWLB VPC. Now, you need to create an endpoint in the FW Subnet of the Lab VPC. To inspect all ingress and egress traffic, you need to configure the route tables to ensure traffic passes through the AWS Gateway Load Balancer endpoints in the FwSubnets before leaving and entering the VPC.

### Pre-deployed Infrastructure
- AWS GWLB service endpoint (from the previous lab).
- LabVpc/FwSubnet1 and LabVpc/FwSubnet2 (one in each AZ).
{{% notice note %}}
Note: If you have completed Lab E (NFW firewall routing configuration), one of the tasks below (associating the edge route table with the LabVpc IGW) may have already been done. You still need to add routes to this route table.
{{% /notice %}}

### Services Used
- Amazon VPC- Route Tables [Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
- AWS Gateway Load Balancer - [Documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/gateway/getting-started.html)

### Success Criteria
- You have created/reused the edge route table and associated it with the LabVpc IGW.
- You have added a route for each IPv4 PubSubnet CIDR block to the edge route table via the GWLB endpoints.
- You have added a default route from each FWSubnet to the IGW.
- You have updated the default IPv4 routes from each PubSubnet to the GWLB endpoints.
- You have verified that you can still access the Internet from `publicInstance` *Remote Access via Systems Manager Session Manager*. To test this, connect and ping any public website.
- You have verified that you cannot access [www.facebook.com](http://www.facebook.com/) from `publicInstance` *Remote Access via Systems Manager Session Manager* [Lab H CloudWatch Dashboard](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabH). To test this, connect and ping any public website. Ensure all the tasks identified above have been completed.

### Tips
**Ingress Route table**
- Configure the Ingress Route table [Ingress Routing](https://aws.amazon.com/blogs/aws/new-vpc-ingress-routing-simplifying-integration-of-third-party-appliances/)

**How to associate a route table to a subnet**
- Associate Route table to a Subnet [Associate](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html#subnet-route-tables) to the relevant subnets.

**Multiple GWLB endpoints needed**
- While you can select multiple subnets when creating a GWLB endpoint, each GWLB endpoint can only be associated with a single subnet. Therefore, you need to create two GWLB endpoints, one for each AZ.

**Lab H Architecture**
![H1](/images/structure/H1.png)

**How to test?**
- Connect to publicInstance via AWS Session Manager:
  + Use AWS Systems Manager Session Manager to connect to publicInstance.
- Run the test command:
  + Enter the following command in the publicInstance terminal:
    ```markdown
    curl -v -4 https://www.facebook.com
    ```
- Expected result:
  + The above command should time out, meaning the request failed because traffic to www.facebook.com is being blocked by the firewall.

[Lab H Guide](6.2.1-WH/_index.en.md)