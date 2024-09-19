---
title : "Cleanups"
date :  "`r Sys.Date()`" 
weight : 8
chapter : false
pre : " <b> 8. </b> "
---
### Step 1 - reverse lab changes
**Lab I:**
- Delete resource association between the WebACL and the Application - Load Balancer
- Delete the WebACL that you created

**Lab H:**
- Remove the IPv4 GWLB endpoint routes from the Public Subnets
- Disassociate the edge route table and delete it
- Delete routes in the FW route tables
- Remove the GWLB endpoints from the LabVpc

**Lab G:**
- Delete the GWLBe service
- Remove the GWLB integration in the Suricata ASG
- Delete the GWLB

**Lab F**
- Disassociate the stateful rules from the NFW policy
- Disassociate the stateless rules from the NFW policy
- Delete the stateful and stateless rule groups created during the lab
- Delete the NFWPolicy

**Lab E:**
- Remove ::/0 routes to NFW from the Public Subnets
- Remove ::/0 routes to IGW from the FW Subnets
- Disassociate the edge route table and delete it

**Lab D:**
- Delete mirror session
- Delete mirror filter
- Delete mirror target

**Lab C:**
- Delete all of the analyses from the network access scope you created
- Delete the network access scope you created as part of this lab.

**Lab B:**
- Disassociate DNS firewall rule group from LabVpc
- Delete the rule in the rule group
- Delete the rule group
- Delete the Domain list you created

**Lab A:**
- Stop query logging for the LabVpc
- Delete query logging

**Intro**
- Remove prefix list entries from IPv4 list
- Remove prefix list entires from IPv6 list

### Step 2 - pre-CloudFormation stack removal
Before you delete the CloudFormation stack, you should:
- Force Delete the Suricata ECS service

### Step 3 - Delete the CloudFormation stack
- Delete CloudFormation stack

### Step 4 - post-CloudFormation stack removal
- Empty and remove Suricata S3 bucket if needed