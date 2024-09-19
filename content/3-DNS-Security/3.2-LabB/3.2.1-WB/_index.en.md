---
title : "Lab B Guide: Implementing a DNS firewall"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 3.2.1 </b> "
---
### Make sure you are in the correct region

1. Navigate to the [AWS Console homepage](https://console.aws.amazon.com/console/home).
2. Ensure you are working in the correct region.

### Create Route 53 DNS Firewall Domain List
1. From the console homepage, click on the search box and type `VPC`.
2. From the VPC console, select the "Domain lists" option from the left menu.
3. Ensure you are working in the correct region.
4. Click the "Add domain lists" button.
5. In the domain list creator, name the list (in our example, `netvpc`).
6. Add `bad.sa-demos.net.` (note the period), then click "Add domain lists".
![B2](/images/structure/B2.png)
{{% notice note %}}
From here, you can use this domain list in the DNS Firewall rule group.
{{% /notice %}}

### Create Route 53 DNS Firewall Rule Group
1. From the Amazon VPC console, select the "Rule groups" option from the left menu (under the DNS Firewall section).
2. Click the "Add rule group" button.
3. For step 1, enter a name for the rule group (in my example, `netvpc` and a description if desired), then click Next.
4. For step 2, click the "Add rules" button.
5. Name the rule (in the example: `block-bad-domain`), choose to add your own domain list, and select the domain list you created in the previous steps.
6. For the action, select "Block", then choose "NXDOMAIN" as the preferred response.
7. Click "Add rule", then click Next.
![B3](/images/structure/B3.png)
8. For step 3, keep the rule priority as is and click "Next".
9. For step 4, add tags if desired and click "Next".
10. In step 5, review the details and click "Create rule group".
11. From the rule group list, click on your newly created rule group and select "View details".
12. From the details page, click the "Associate VPCs" tab.
![B4](/images/structure/B4.png)
13. Click "Associate VPC" and in the popup window, select the "LabVpc" option, then click Associate.
![B5](/images/structure/B5.png)
14. Wait a few minutes for the association process to complete.

### Verify DNS Rule Group is Working
    
You can verify if DNS resolution is failing by connecting to the `publicInstance` session manager - a quick link to do this can be found on the [Lab B CloudWatch Dashboard](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabB) - and once connected, try the command: `nslookup bad.sa-demos.net`.
    
Another approach is to check the CloudWatch Logs group; to do this, we can use a similar CloudWatch Logs Insights query as we used in Lab 2:
    
1. Navigate to the CloudWatch console and select the "Logs Insights" option from the left menu.
2. From the dropdown list, select the `"Route53Resolver"` log group to query.
3. Delete the sample query (3 lines) and replace it with:
    ```plaintext
    filter query_name = 'bad.sa-demos.net.' and rcode = 'NXDOMAIN' 
    | stats count(*) as numRequests by srcids.instance 
    | sort numRequests desc 
    | limit 10
    ```
4. Run the query and view the results.
![B6](/images/structure/B6.png)
5. You will see the EC2 instance ID in the search results; this indicates that we have successfully blocked queries to that domain from that instance.

**Congratulations on completing Lab B!**