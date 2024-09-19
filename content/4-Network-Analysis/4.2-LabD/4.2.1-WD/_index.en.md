---
title : "Lab D Guide: Mirroring VPC network traffic"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 4.2.1 </b> "
---
### Make sure you are in the correct region
1. Navigate to the [AWS Console homepage](https://console.aws.amazon.com/console/home).
2. Ensure you are working in the correct region.

### Configure the mirror target
1. Record the ENI ID of mirrorInstance from the [Lab D CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DLabD%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=wQc7X0stB0KmBa7-wT73ACi4nL4ZpsXY_MmpWkVJXFc) to use as the mirror target.
![D2](/images/structure/D2.png)
2. From the console homepage, click on the search box and type `VPC`.
3. From the VPC console, select the "Mirror targets" option in the menu, then select "Create traffic mirror target”.
![D3](/images/structure/D3.png)
4. Add a name and description for the target, select the target type as "Network Interface" and choose the pre-identified ENI, then click "Create".
![D4](/images/structure/D4.png)

### Configure the mirror filters
1. Record the private IP addresses (IPv4 and IPv6) of the EC2 mirrorInstance and use this information when creating the mirror filter rules, with information obtained from the [Lab D CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DLabD%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=wQc7X0stB0KmBa7-wT73ACi4nL4ZpsXY_MmpWkVJXFc).
![D5](/images/structure/D5.png)
2. From the VPC console, select the "Mirror filters" option in the menu, then click the filter ID link of the pre-created filter (named "LabDMirrorFilter") to edit it.
![D6](/images/structure/D6.png)
3. Four rules have already been configured; two inbound rules deny (do not mirror) HTTPS response traffic from the VPC CIDR block (both IPv4 and IPv6) to publicInstance; and two outbound rules deny HTTPS request traffic from publicInstance to the VPC CIDR block (both IPv4 and IPv6). This is to eliminate some noise for the purpose of the demo.
4. We will add four more rules; two outbound rules to accept (mirror) HTTPS requests from publicInstance to the Internet (IPv4 and IPv6), and two inbound rules to accept HTTPS responses from the Internet (IPv4 and IPv6) to publicInstance.
5. Access the "Outbound rules" tab and click "Add outbound rule" to add the outbound rules.
6. Keep the rule number as 210 (we want this rule to execute after the existing rules), keep the rule action as "accept". Set the protocol to TCP and set the source port range to 1024-65535, and the destination port range to 443. Enter the private IPv4 address (with /32 at the end) of publicInstance in the source CIDR block field, then add 0.0.0.0/0 to the destination CIDR block field, then click "Add rule".
![D7](/images/structure/D7.png)
7. Add a second outbound rule (this time for IPv6). Keep the rule number as 310, keep the rule action as "accept". Set the protocol to TCP and set the source port range to 1024-65535, and the destination port range to 443. Enter the private IPv6 address (with /128 at the end) of publicInstance in the source CIDR block field, then add ::0/0 to the destination CIDR block field, then click "Add rule".
![D8](/images/structure/D8.png)
8. Click on the "Inbound rules" tab and select "Add inbound rule" to create the inbound rules.
9. To create the inbound rule, keep the rule number as 210 (we want this rule to run after the existing rules), keep the rule action as "accept". Set the protocol to TCP, and set the source port range to 443, and the destination port range to 1024-65535. Enter 0.0.0.0/0 in the source CIDR block field, and then add the private IPv4 address (with /32 at the end) of publicInstance in the destination CIDR block field, then click "Add rule".
![D9](/images/structure/D9.png)
10. Keep the rule number as 310 (we want this rule to run after the existing rules), keep the rule action as "accept". Set the protocol to TCP, and set the source port range to 443, and the destination port range to 1024-65535. Enter ::0/0 in the source CIDR block field, and then add the private IPv6 address (with /128 at the end) of publicInstance in the destination CIDR block field, then click "Add rule".
![D10](/images/structure/D10.png)

### Configure the mirror session
1. Record the ENI ID of publicInstance and the Virtual Network ID (308308) from the [Lab D CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DLabD%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=wQc7X0stB0KmBa7-wT73ACi4nL4ZpsXY_MmpWkVJXFc) to use when registering as the mirror source.
![D11](/images/structure/D11.png)
2. From the VPC console, select the "Mirror session" option in the menu, then click the "Create traffic mirror session" button.
3. Set the name and description for the session, select the mirror source as the ENI of publicInstance and the mirror target as the target you created earlier.
![D12](/images/structure/D12.png)
4. Set the Session number to 1, VNI to "308308", leave the packet length blank, select the updated filter, and click "Create".
![D13](/images/structure/D13.png)

### View the packet captures
1. Connect to mirrorInstance via Session Manager using the link in the Useful Links section of the [Lab D CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DLabD%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=wQc7X0stB0KmBa7-wT73ACi4nL4ZpsXY_MmpWkVJXFc).
![D14](/images/structure/D14.png)
2. Connect to mirrorInstance, switch to root privileges using the command sudo bash, and check the network interfaces using the command ifconfig -a to confirm that the vxlan0 interface has been configured.
![D15](/images/structure/D15.png)
3. You can run the command:
    ```
    tshark -i vxlan0 -Y http -T fields -e ipv6.src -e ipv6.dst -e http.request.method -e http.request.uri -e http.file_data
    ```
To view the mirrored HTTP packet data over IPv6 to mirrorInstance. Let the command run for about a minute to see the traffic, and press Ctrl-C to stop when enough data has been captured.
![D17](/images/structure/D17.png)
4. You will see some packets transmitted in clear text instead of being encrypted.
![D18](/images/structure/D18.png)
"publicInstance" is using port 443 for unencrypted HTTP connections, and in Labs E and F, we will explore how to configure AWS Network Firewall to automatically detect this traffic.

**Congratulations on completing Lab D!**