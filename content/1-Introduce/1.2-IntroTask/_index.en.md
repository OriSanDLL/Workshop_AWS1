---
title : "Introductory Task: Managing VPC Access Control at Scale"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 1.2 </b> "
---
### Objective:

- Before starting the labs, you need to configure security controls for your VPC so that the WebAlb load balancer can receive HTTP/HTTPS requests from your laptop's internet connection. This workshop uses managed prefix lists in VPC to help you achieve this. IPv6 has been enabled on some deployed resources, and although your computer may not be configured to connect via IPv6, we have included an option to allow IPv6 connections if available.

- Note that in many cases, traffic may not come from your computer's actual IP address but from an HTTP or NAT gateway of the connecting network. We call this the "Gateway IP Address," and this address will differ for IPv4 and IPv6.

### Starting Point:

- Your goal in this lab is to update your VPC security controls so that the WebAlb load balancer can receive HTTP traffic (TCP port 80) from the Gateway IP address. We have added rules to some of WebAlb's security groups referencing [**VPC prefix lists**](https://docs.aws.amazon.com/vpc/latest/userguide/managed-prefix-lists.html) to address this. After adding your Gateway IP address to the correct managed prefix list, you will be able to browse from your computer to the WebAlb load balancer.

### Pre-deployed Infrastructure

The following resources have been pre-deployed to assist you:

- **WebAlb Load Balancer ("webAlb")**: Includes a few EC2 instances behind it, providing some useful information.
- **Security Group for WebAlb ("LabVpc/webAlbSG")**: Contains inbound rules for "workshopPrefixListIPv4" and "workshopPrefixListIPv6".
- **Managed Prefix List for VPC (IPv4) ("workshopPrefixListIPv4")**: This list is currently empty. For IPv4, use CIDR format like `x.x.x.x/32`.
- **Managed Prefix List for VPC (IPv6) ("workshopPrefixListIPv6")**: This list is also currently empty. For IPv6, use the format `xxxx:xxxx:xxxx:xxxx::/128`.

You can find links to WebAlb and CloudFront endpoints on the [introductory CloudWatch dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DIntro%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=3E143Zf0PtHRf11obIDAEqRxb7AxZ3S246oVgrOe1vQ).

### Services Used:

1. **Amazon VPC Managed Prefix Lists**:
    - **Description**: Managed prefix lists in Amazon VPC allow you to create and manage lists of CIDR blocks that you can reference in security group rules and route tables.
    - **Documentation**: [Amazon VPC Managed Prefix Lists Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/managed-prefix-lists.html)
2. **Amazon VPC Security Groups**:
    - **Description**: Security groups in Amazon VPC act as virtual firewalls to control inbound and outbound traffic to Amazon EC2 resources in the VPC.
    - **Documentation**: [Amazon VPC Security Groups Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)
3. **AWS CheckIp**:
    - **Description**: A simple AWS service that allows you to check your public IP address, helping to identify the Gateway IP address you need to add to the prefix list.
    - **Website**: [AWS CheckIp](https://checkip.amazonaws.com/)

### **Tips:**

**What are Prefix lists?**

   - Prefix lists are collections of CIDR blocks that simplify the configuration and management of security groups and route tables. Instead of referencing individual IP addresses, you can use prefix lists to group commonly used IP addresses into a set and apply them in security or routing rules. This helps consolidate rules with the same port and protocol but different CIDRs into a single rule.
   - [Reference Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/managed-prefix-lists.html)

**Finding your Gateway IP address**

   - To restrict access to your laptop, you need to find the "gateway" IP address that your network uses to connect to the internet, which can be done via the [AWS CheckIp website](https://checkip.amazonaws.com/). This address is a single IP, and to turn it into a valid CIDR range, you need to add **/32** at the end. If you cannot find it, you can use **0.0.0.0/0** (IPv4) or **::/0** (IPv6) to allow access from anywhere, but this is a last resort.

**Managing your own prefix lists**

   - Amazon VPC allows you to create and manage your own prefix lists, which can be referenced in other VPC constructs like security groups and route tables. You can use the existing prefix lists "workshopPrefixListIPv4", add your gateway IP address to it, and use it to control access through the WebAlb security group ("LabVpc/webAlbSG").

**Introductory task completed architecture**

![IntroArchitecture](/images/structure/architecture.png)