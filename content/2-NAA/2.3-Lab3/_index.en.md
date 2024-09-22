---
title : "Lab 3 - Security controls (e.g., firewall/NAT-GW) in path"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---
### Objectives
- Using the Network Access Analyzer, we can ensure that we have proper network controls such as Network Firewall and NAT gateways on all network paths between our resources.
- In this lab, we will create a custom network access scope to check if instances in the VPC route traffic through an inspection VPC containing a Network Firewall.

1. Go back to the main page of "Network Access Analyzer" and click "Create Network Access Scope".
![A8](/images/1/A8.png)
2. Select "Empty Template" and click "Next".
![A9](/images/1/A9.png)
3. Enter Name and Description.
![A19](/images/1/A19.png)
4. Select "Add match condition".
- Follow the instructions in the image:
![A20](/images/1/A20.png)
5. Select "Next" and "Create Network Access Scope".
6. Select the newly created "Network Access Scope" and click "Analyze".
7. After completion, we can see that there are findings.
- We are interested in reviewing 'TCP' traffic to filter the findings as shown in the diagram.
![A21](/images/1/A21.png)

- We will see that the Prod and Dev VPCs have a route through the Network Firewall (Inspection VPC).

{{% notice note %}}
**Note**: We can filter the findings using resource type, protocol, etc. Select one of the instances. We may notice that the destination address is 0, indicating a route to the internet.
{{% /notice %}}
- Scroll down the right pane, and we will see the full path ending at the Central Egress VPC IGW. Notice the Network Firewall in the path as shown below.
![A22](/images/1/A22.png)

8. Now we can validate:
- Whether compliance is met.
- That is, find any paths that do not have a Network Firewall.
- We will duplicate and modify this network scope and analyze it.
9. Click "Actions", select "Duplicate and modify".
![A23](/images/1/A23.png)
10. Enter Name and Description.
![A24](/images/1/A24.png)
11. In "Exclusion Conditions", select "Add exclusion condition" and add "AWS Network Firewalls", then select "Duplicate and analyze Network Access Scope".
![A25](/images/1/A25.png)
12. As expected, we will not see any findings.
- Because our Prod - Dev VPC traffic routes through the Network Firewall and complies with the policy, no findings are detected.
![A26](/images/1/A26.png)

**Summary**: Lab 3 completed.
- In this lab, you performed the following steps:
  - Created a custom network access scope: Created a custom network access scope to check if the Prod and Dev VPCs route traffic to the internet through the Network Firewall.
  - Duplicated the network access scope: Duplicated the scope and added an exclusion to identify any paths that do not go through the Network Firewall.
  - Detected unwanted paths: If anyone tries to bypass the Network Firewall, we will be able to detect these cases through analysis and take appropriate actions to address them.
{{% notice note %}}
This process helps ensure that your network traffic is properly protected and monitored, preventing non-compliant behavior.
{{% /notice %}}