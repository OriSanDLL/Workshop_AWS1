---
title : "Lab 4 - Trusted network access"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---
## Objectives
- Using the Network Access Analyzer, we can verify that our resources only have network access from trusted IP address ranges, through specific ports and protocols.
- In this lab, we will create a custom network access scope to validate the following compliance requirement:
  + Outbound traffic from the Pro instance to the Internet is only allowed to download updates from the public IP range 55.3.0.0/16 through port 443.

1. Go back to "Network Access Analyzer", select "Create Network Access Scope"
![A8](/images/1/A8.png)
2. Select "Empty Template" and click "Next"
![A9](/images/1/A9.png)
3. Enter Name and Description
![A27](/images/1/A27.png)
4. Select "Add match condition"
- Follow the instructions in the image:
![A28](/images/1/A28.png)
5. In "Exclusion", select "Add exclusion condition" and expand "Traffic type" under "Destination". Enter the following information:
+ Enter `55.3.0.0/16` for the Destination address.
+ Enter `443` for the Destination port.
+ Select `TCP` for the Traffic type.
![A29](/images/1/A29.png)
6. Select "Next" and "Create Network Access Scope"
7. Select the newly created "Network Access Scope" and click "Analyze"
8. After completion:
- We can see that there are findings indicating that network paths exist between the EC2 instance and the internet gateway beyond the IP and port restrictions we specified in the network scope.
- This is because we can see that the security group allows traffic to 0.0.0.0/0 on all ports.
- Select the first result and view the details
![A30](/images/1/A30.png)
9. Select "sg-..." to navigate to the "security group" page and follow the instructions in the image below:
![A31](/images/1/A31.png)
10. After making the changes, go back to "Network Access Analyzer" and select "Analyze"
- After completion, we can see that there are no findings, confirming that we are compliant. (i.e., our EC2 instance can only download updates from 55.3.0.0/16 through TCP port 443).
![A32](/images/1/A32.png)
![A33](/images/1/A33.png)

**Summary**: Lab 4 completed.
- In this lab, we performed the following steps:
  - Created a custom network access scope: Created a custom network access scope to analyze outbound traffic paths from an EC2 instance to the internet.
  - Modified the security group: Adjusted the security group to restrict outbound access to a specific IP/port.
  - Re-analyzed the traffic paths: After making changes, we re-analyzed the traffic paths to confirm that compliance requirements were met.
{{% notice note %}}
This process helps ensure that outbound traffic from the EC2 instance is properly controlled, reducing security risks.
{{% /notice %}}