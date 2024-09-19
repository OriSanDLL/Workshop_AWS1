---
title : "Lab I: Protecting Web Applications"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 7.1 </b> "
---
### Objective
- Protect WebAlb: Ensure that all traffic to WebAlb (Application Load Balancer) only comes from an Amazon CloudFront distribution, not from other users (except from your personal computer).
- Limit traffic from specific CloudFront: Only allow traffic from a specific CloudFront distribution to access WebAlb, rather than any CloudFront distribution that might know about WebAlb.

### Starting Point
1. Ensure Traffic to WebAlb Only from CloudFront:
   - Consider methods to ensure that all traffic to WebAlb only comes from CloudFront infrastructure.
2. Use AWS WAF to Inspect L7 Traffic:
   - After ensuring traffic only comes from CloudFront, consider using AWS WAF to inspect Layer 7 (L7) traffic and verify that requests come from the CloudFront distribution you control.

### Pre-deployed Infrastructure
1. Application Load Balancer (WebAlb):
   - An Internet-facing Application Load Balancer listening on HTTP port 80.
   - Behind WebAlb are several EC2 instances serving content.
2. Amazon CloudFront distribution:
   - Configured to insert a custom HTTP header into requests it sends to the origin infrastructure (WebAlb).
3. Second CloudFront distribution:
   - Not configured to insert a custom header (used for control and demo purposes).
{{% notice note %}}
Note: The CloudFront distributions will initially return a 504 error, which is normal and will be resolved after completing the first task.
{{% /notice %}}

### Services Used
- Amazon CloudFront - [Documentation](https://docs.aws.amazon.com/cloudfront/)
- AWS Web Application Firewall (WAF) - [Documentation](https://docs.aws.amazon.com/waf/)
- Amazon VPC Security Groups

### Success Criteria
- Ensure that the Application Load Balancer (WebAlb) is accessible from the Amazon CloudFront distributions and from your laptop.
- Additionally, ensure that WebAlb can only be accessed from the Amazon CloudFront distribution containing the custom HTTP header.

### Tips
**Restricting traffic to CloudFront distributions**
- CloudFront has nodes globally, and their IP addresses can be found online [here](https://docs.aws.amazon.com/vpc/latest/userguide/aws-ip-ranges.html).
- Previously, users had to manually add these IP addresses to the security group.
- AWS now provides a [managed prefix list](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/LocationsOfEdgeServers.html#managed-prefix-list), which automates this process.

**Filtering by HTTP header on the WebAlb Application Load Balancer**
- CloudFront is configured to insert the "OriginSig" header into requests to the ALB.
- ALB cannot filter traffic by header but can be associated with AWS WAF [[here]](https://us-east-1.console.aws.amazon.com/wafv2/homev2/web-acls?region=us-west-2).
- Create a rule in AWS WAF to block traffic without the "OriginSig" header [[here]](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rules.html).

**Lab I Architecture**
![I1](/images/structure/I1.png)

[Lab I Guide](7.1.1-WI/_index.en.md)