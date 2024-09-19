---
title : "Lab G Guide: Configuring Gateway Load Balancer Service"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 6.1.1 </b> "
---
We want to set up an AWS Gateway Load Balancer (GWLB) to inspect all ingress and egress traffic before it reaches public and private subnets. To do this, we need to create a GWLB in the GWLB VPC, and this GWLB will point to the AutoScaling Group (ASG) running Suricata. After this configuration, the route tables will be updated to ensure all traffic passes through the GWLB.
![G1](/images/structure/G1.png)

### Create Target Group
1. In the AWS Management Console, type "EC2" into the quick search box and press Enter:
2. Go to Load Balancing → Target Group and click *Create Target Group*.
3. Under Basic configuration:
- **Choose a target type**: Instances
- **Target group name**: netvpc-TG-GWLB
- **Protocol: Port**: GENEVE
- **VPC**: GwlbVpc
4. In Health check protocol - choose HTTP and set / under the path.
- Click Next.
![G2](/images/structure/G2.png)
5. In the Available instances section, do not select anything, then click Create target group.
- Click Create target group.

### Create Gateway Load Balancer
1. In the AWS Management Console, start typing "EC2" into the quick search box and press Enter.
2. Go to Load Balancing → Load Balancer and click Create load balancer.
![G3](/images/structure/G3.png)
3. Select Gateway Load Balancer and click Create.
![G4](/images/structure/G4.png)
4. Set up the following details:
- **Load balancer name**: GWLB
- **IP address type**: IPV4
- **VPC**: GwlbVpc
- **Subnets**: GwlbVpc/PriSubnet1 & GwlbVpc/PriSubnet2
- **IP listener routing**: netvpc-TG-GWLB (the one created in the previous step)
- Click **Create load balancer**
![G5](/images/structure/G5.png)

### Associate the ASG with the Gateway Load Balancer
1. In the AWS Management Console, start typing EC2 into the quick search box and press Enter:
2. Click on Autoscaling Groups in the left menu.
3. Click on netwks-SuricataAutoScalingGroup.
4. In the Details tab, scroll down to the Load balancing section and click Edit.
5. Scroll down to the Load balancers section:
   - Click on Application, Network or Gateway Load Balancer target groups.
   - Select the GWLB created in the previous step.
![G6](/images/structure/G6.png)
6. Click **Update**

### Create GWLB Endpoint Service
1. Go to VPC → Endpoint Services and click Create endpoint service in the top right corner.
- **Name**: GWLB-Endpoint
- **Load balancer type**: Gateway
- **Available load balancers**: GWLB (load balancer created in the previous step)
- **Acceptance required**: should be un-ticked
- **Supported IP address types**: IPv4
![G7](/images/structure/G7.png)
2. Click Create. After creation, copy the service name into a note-taking application; you will need it for the next lab.
![G8](/images/structure/G8.png)

**Congratulations on completing Lab G!**