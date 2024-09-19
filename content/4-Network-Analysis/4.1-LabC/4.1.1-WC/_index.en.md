---
title : "Lab C Guide: Analysing VPC Network Configuration"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 4.1.1 </b> "
---
### Make sure you are in the correct region

1. Navigate to the [AWS Console homepage](https://console.aws.amazon.com/console/home).
2. Ensure you are working in the correct region.

### Run Network Access Analyzer

We can use Network Access Analyzer to automatically check any allowed access patterns according to the VPC network configuration. In this case, we will identify all traffic that can access EC2 from the Internet, except for traffic going through the WebAlb load balancer, as that is valid traffic.

1. From the console homepage, click on the search box and type "Network Manager". This is a feature of the VPC console.
2. From the Network Manager console, select the "Network access analyzer" option from the left menu and click "Get started".
3. After a few minutes, a list of pre-configured network access scopes will appear. While we could use one of those, we will create our own network access scope.
4. Click the "Create Network Access Scope" button, then select "Next".
5. Choose to start with a blank template, then select "Next".
![C2](/images/structure/C2.png)
6. Name the scope (in the example, `netvpc`) and select "Add Match condition".
7. "Match findings - condition 1"
- Source:
  + Resource selection: **"Resource IDs"**
  + Resource types: **"InternetGateway"**
  + Resource ID: **"LabVpc/IGW"**
![C3](/images/structure/C3.png)
8. "Match findings - condition 1"
- Destination:
  + Resource selection: **"Resource Types"**
  + Resource types: **"AWS::EC2::Instance"**
![C4](/images/structure/C4.png)
9. "Add exclusion condition"
- Through:
    + Resource selection: **"Resource IDs"**
    + Resource types: **"Elastic Load Balancers V2"**
![C5](/images/structure/C5.png)
10. Click "Next".
- Click "Create Network Access Scope".
- From the Network Access Scopes screen:
  + Select the row containing the scope you just created.
  + Select "Analyze".
![C6](/images/structure/C6.png)
11. The analysis will take a few minutes to complete. Once finished, you will be able to click to view the results.
- The left panel shows a summary of the findings.
- The diagram on the right side of the screen provides a detailed analysis of the network traffic path to the EC2 instance.
![C7](/images/structure/C7.png)
- The left panel can be scrolled and column sizes adjusted to view details of allowed TCP traffic.
- Click on the hop points in the diagram on the right to view detailed information about the source.
![C8](/images/structure/C8.png)
- The analysis shows the EC2 instance receiving SSH traffic from the internet from two addresses: 203.0.113.0/24 (IPv4) and 2001:db8::/32 (IPv6). The final part of the lab is to remove the SSH rules from the security group.

{{% notice note %}}
Note: Don't worry - your instance has not been exposed to the Internet during this workshop! These two network ranges are actually non-routable on the Internet; see [RFC 5735](https://www.rfc-editor.org/rfc/rfc5735) for IPv4 and [RIPE's IPv6 reference card](https://www.ripe.net/media/documents/ipv6_reference_card.pdf) for IPv6.
{{% /notice %}}

### Remove SSH Security Group Rules
Now that we have identified the incoming SSH access, we should remove it from the security group.

1. Navigate to the VPC console, then find the `LabVpc/publicInstanceSG` security group.
2. Click on the "Inbound rules" tab and select "Edit inbound rules".
![C9](/images/structure/C9.png)
3. {{% notice note %}}
Note that although Network Access Analyzer currently only works with IPv4, you may find that an SSH rule has also been configured for IPv6.
{{% /notice %}}
- Select "Delete" for both the IPv4 and IPv6 rules, then select "Save rules".
![C10](/images/structure/C10.png)
4. Now:
- Return to Network Access Analyzer.
- Re-run the analysis for the created scope.
- Result: No findings (as there is no other way for Internet traffic to reach the instances, except through WebAlb).
![C11](/images/structure/C11.png)

**Congratulations on completing Lab C!**