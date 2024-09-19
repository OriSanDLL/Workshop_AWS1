---
title : "Lab G: Configuring Gateway Load Balancer Service"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 6.1 </b> "
---
### Objective
- Your security team has created a Gateway Load Balancer VPC (GwlbVpc) and installed Suricata, an open-source firewall on an ECS cluster. Your task now is to configure the Gateway Load Balancer service, ready for use in LabVpc in Lab H.

### Pre-deployed Infrastructure
- You will find an auto-scaling group (netwks-Suricata...) connected to an Elastic Container Service (ECS) cluster running the Suricata firewall. These components have been deployed into a separate VPC (GwlbVpc).

### Services Used
- AWS Gateway Load Balancer and Amazon VPC

### Success Criteria
- You have created a target group using the GENEVE protocol (the protocol used by AWS Gateway Load Balancer - GWLB).
- You have successfully configured the AWS Gateway Load Balancer in the GWLB VPC.
- You have successfully associated the auto-scaling group (ASG) with the GWLB.
- You have successfully created a GWLB endpoint service.

### Tips
**Create Gateway Load Balancer**
- Configure the Gateway Load Balancer [GWLB](https://docs.aws.amazon.com/elasticloadbalancing/latest/gateway/create-load-balancer.html)

**Create GWLB Endpoint Service**
- Create an endpoint service using your Gateway Load Balancer [Endpoint](https://docs.aws.amazon.com/elasticloadbalancing/latest/gateway/getting-started.html#create-endpoint-service) to the relevant subnets.

**Lab G Architecture**
![G1](/images/structure/G1.png)

[Lab G Guide](6.1.1-WG/_index.en.md)