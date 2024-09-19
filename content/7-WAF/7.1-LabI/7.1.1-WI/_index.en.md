---
title : "Lab I Guide: Protecting Web Applications"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 7.1.1 </b> "
---
### Ensure You Are in the Correct Region
1. Navigate to the [AWS Console Home](https://console.aws.amazon.com/console/home).
2. Ensure you are working in the correct region.

### Adding a CloudFront Global Prefix List to the WebAlb
1. Go to VPC.
2. Select "Managed prefix lists".
![I2](/images/structure/I2.png)
3. Find the "Managed prefix lists" with the following line:
`com.amazonaws.global.cloudfront.origin-facing`
![I3](/images/structure/I3.png)
4. Note down the last few characters of the prefix list ID (you will need this later) - in this example, it is `4526`.
5. From VPC, go to Security Groups.
6. Select the box next to "LabVpc/webAlbSG" in the security group.
7. Select the "Inbound rules" tab in the lower panel, then click "Edit inbound rules".
![I4](/images/structure/I4.png)
8. Click "Add rule".
- Type: HTTP
- Source: Custom
- then click the search box
- Find the "Prefix lists" with characters like in this example `4526`.
9. Select it, then add an optional description - in this example, "Inbound TCP:80 from CloudFront".
10. Click "Save rules".
![I5](/images/structure/I5.png)
11. Confirm that your rule is in the Inbound rules list.

### Configuring AWS WAF
1. From the main console page, click the search box and type "WAF".
2. From the WAF console:
- select the "Web ACLs" option from the left menu.
- Ensure you are in the correct region.
- Click "Create web ACL".
3. Step 1:
- Web ACL Name: "netwks-webacl"
- Description: As desired
- CloudWatch metric name: "netwks-webacl"
- Resource type: Regional resources
- Region: Ensure it is the region you are working in.
- Click "Next" to continue.
![I9](/images/structure/I9.png)
4. Step 2: Add a new rule.
- From the Rules section, click "Add rules".
- Select "Add my own rules and rule groups".
- This will take you to the "Add my own rules and rule groups" screen.
- Ensure the "Rule builder" option is selected.
- Add a name for the rule (e.g., "netwks-rule01").
- Ensure the rule is a "regular rule".
![I10](/images/structure/I10.png)
![I11](/images/structure/I11.png)
5. Continue building the rule:
- Configure the rule to trigger when the request "matches the statement".
- Choose to inspect a header, and enter "originsig" as the header field name.
- Select the match type as "Exactly matches string", and provide "vpcsecurity" as the string to match.
- Ensure the text transformation is set to "Lowercase".
![I12](/images/structure/I12.png)
6. Final part of the rule:
- Select "Allow" as the action, and leave the other parts at their default values.
- Click "Add rule" to return to the Step 2 page.
- Here, configure the default action of the Web ACL:
  + Block any requests that do not match the rule.
  + Optional: Configure a custom response code and message. For example, use HTTP response code 403 and choose to create a custom response body.
![I13](/images/structure/I13.png)
7. Custom response body:
- You can return JSON, HTML, or plain text.
- For example, create a custom response body called "netwks-denied", returning an HTML response with the message: `<strong>Invalid CloudFront distribution used to request this resource</strong>`.
- Click "Save" and complete Step 2 by clicking "Next".
![I14](/images/structure/I14.png)
8. Next steps:
- Step 3: With only one rule, no need to change the priority. Click "Next".
- Step 4 (Configure metrics): Leave the default options unchanged and click "Next".
- Step 5: Review the configured options and click "Create web ACL". AWS WAF may take a few minutes to complete the necessary tasks - do not leave the page while this process is ongoing.
9. Next step: To associate the web ACL with a resource, from the Web ACL list, click the name "netwks" to view the Web ACL details page. In the "Associated AWS resources" tab, click "Add AWS resources".
![I15](/images/structure/I15.png)
10. Next step: Select "Application Load Balancer", then select "WebAlb" and click "Add". This may take a minute or two to complete. Once done, you can test the web ACL by accessing the two different CloudFront distributions created.
![I16](/images/structure/I16.png)
![I17](/images/structure/I17.png)

**Congratulations on completing Lab I!**