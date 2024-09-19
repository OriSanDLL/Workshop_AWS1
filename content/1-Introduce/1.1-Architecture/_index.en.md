---
title : "Architecture"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 1.1 </b> "
---
### Architecture Diagram
An overview of the initial architecture is shown below:
![Architecture](/images/structure/intro-architecture.png)

- The workshop focuses on "LabVpc", simulating infrastructure with two Availability Zones (AZ) and deploying four network layers:
  + Public Subnets (PubSubnets): These have a default route to the Internet Gateway and contain a NAT Gateway, an Application Load Balancer (webAlb), and EC2 instances.
  + Private Subnets (PriSubnets): These have a default route to the NAT gateways in the Public Subnets and contain an Auto Scaling Group for EC2 web servers and an EC2 instance to handle mirrored traffic (mirrorInstance).
  + Isolated Subnets (EndSubnets): These have no default routes and contain PrivateLink endpoints for AWS services such as CloudFormation, CloudWatch, and Systems Manager.
  + Firewall Subnets (FwSubnets): These do not have a default route yet but will use AWS Network Firewall later in the workshop.

Some additional components have been pre-deployed, such as AWS Network Firewall, Amazon CloudFront (using webAlb as the origin), and CloudWatch Log Groups. Links to these resources can be found on the [CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DIntro%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=mIJhXAMe_gxLDCJtBz3QaX-B7dmpdF-_ybkJ4TR6JBA) after logging into the AWS workshop account or deploying the CloudFormation stack into a personal account.

### Additional Infrastructure
- The workshop uses an additional VPC (ExtVpc) to simulate Internet-facing targets, containing public subnets and an Internet-facing ALB. This ALB provides a simple Lambda response for information retrieval.

- Additionally, Lambda functions run every minute, triggered by an Amazon EventBridge rule, to monitor the workshop infrastructure status and check task completion.

- No changes are required for any of these additional infrastructure components throughout the workshop.