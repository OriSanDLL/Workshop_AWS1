---
title : "Track: Web Application Firewall"
date :  "`r Sys.Date()`" 
weight : 7
chapter : false
pre : " <b> 7. </b> "
---
### Lab Overview
[Lab I:](7.1-LabI/_index.en.md) To protect WebAlb (Application Load Balancer) from sudden traffic spikes or DDoS attacks, we want to ensure that all traffic to WebAlb must go through an Amazon CloudFront distribution, and not directly from end-users. The secondary objective is to ensure that only traffic from a specific CloudFront distribution is allowed, rather than any CloudFront distribution.