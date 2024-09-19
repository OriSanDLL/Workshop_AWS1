---
title : "Lab A Guide: Gaining visibility into VPC activity"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 3.1.1 </b> "
---
### Make sure you are in the correct region

1. Navigate to the [AWS Console homepage](https://console.aws.amazon.com/console/home).
2. Ensure you are working in the correct region.

### Enable DNS Query Logging

1. From the console homepage, click on the search box and type "Route 53".
2. From the Route 53 console, select the "Query logging" option from the left menu. Note - you may need to do this twice to avoid the Route 53 resolver splash page.
3. Route 53 defaults to the US-East-1 region, so ensure you are working in the correct region.
4. Click the “Configure query logging” button.
![A2](/images/structure/A2.png)
5. Enter a name for the query logging configuration, such as `netvpc`, select CloudWatch Logs Log Group as the destination, and choose the "Route53Resolver" log group from the list.
![A3](/images/structure/A3.png)
6. Scroll down and then click "Add VPC".
7. Select the "LabVpc" option and click Add.
![A4](/images/structure/A4.png)
8. Add additional tags if desired.
9. Click the "Configure query logging" button to enable query logging.
10. You can now click on the configuration and then check if logging is working by following the "destination ARN" link.
![A5](/images/structure/A5.png)
11. From the CloudWatch Logs page, you can view the log stream and check if query logs are being collected.
![A6](/images/structure/A6.png)

### Search Logs with CloudWatch Logs Insights

To search through DNS query logs, we can use CloudWatch Logs Insights.

1. From the console homepage, click on the search box and type "CloudWatch".
2. From the CloudWatch console, select the "Logs Insights" option from the left menu.
3. From the dropdown list, select the "Route53Resolver" log group to query.
![A7](/images/structure/A7.png)
4. Delete the sample query (3 lines) and replace it with:
    ```plaintext
    stats count(*) as numRequests by query_name 
    | sort numRequests desc 
    | limit 10
    ```
5. Run the query and view the results; note that if you just configured Query Logs, it may take a few minutes for results to appear.
![A8](/images/structure/A8.png)
6. Note the result with "bad.sa-demos.net" - we will investigate this domain further and see which EC2 instances queried this domain. We will also count by record type to see if these queries are for IPv4 (A) or IPv6 (AAAA) records.
7. Update the Log Insights query with the following code:
    ```plaintext
    filter query_name = 'bad.sa-demos.net.' 
    | stats count(*) as numRequests by srcids.instance, query_type, answers.0.Rdata as Response 
    | sort numRequests desc 
    | limit 10
    ```
8. Run the query and view the results.
![A9](/images/structure/A9.png)
9. You will see an EC2 instance ID in the search results; this ID along with the suspicious domain name will be used in the next lab.

**Congratulations on completing Lab A!**