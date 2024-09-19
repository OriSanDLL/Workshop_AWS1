---
title : "Lab D: Mirroring VPC network traffic"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---
### Objective
- Further investigate the publicInstance exposed to the Internet.
- Inspect HTTP requests on TCP port 443 to detect suspicious activity.
- Consider both IPv6 and IPv4 traffic.

### Starting Point
- VPC traffic logs only provide information about traffic at the flow level.
- AWS provides tools to capture and analyze packets from Elastic Network Interfaces.

### Pre-deployed Infrastructure
- The EC2 instance mirrorInstance has been configured to receive packets for analysis.
- Use network ID 308308 for VXLAN encapsulation.
- A mirror filter has been pre-created to exclude internal VPC traffic.
- Additional rules need to be added to the filter to mirror HTTPS (TCP:443) traffic and its responses from the internet.

### Services Used
- Amazon VPC Traffic Mirroring - [Documentation](https://docs.aws.amazon.com/vpc/latest/mirroring/what-is-traffic-mirroring.html)

### Success Criteria:
- Configure mirrorInstance as a mirror target for VPC traffic mirroring.
- Add additional rules to the pre-created mirror filter to include:
  + Traffic sent to the Internet (Destination TCP port 443) from publicInstance (including both IPv4 and IPv6 traffic).
  + Traffic returned from the Internet (Source TCP port 443) to publicInstance (including both IPv4 and IPv6 traffic).
{{% notice note %}}
Note: IPv4 and IPv6 require separate rules in the mirror filter.
Create a mirror session to start capturing packets and sending them to mirrorInstance.
{{% /notice %}}

- Instructions for connecting and using tshark:
  1. Connect to mirrorInstance:
    - Use [Session Manager to connect to mirrorInstance](https://ap-southeast-2.signin.aws.amazon.com/oauth?client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fec2-tb&code_challenge=ArCf5Hse0GaVbDJyAKIyALiVL7PO-8C_tQnfaaZYPvM&code_challenge_method=SHA-256&response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fec2%2Fhome%3FhashArgs%3D%2523Instances%253A%26isauthcode%3Dtrue%26oauthStart%3D1726678112171%26state%3DhashArgsFromTB_ap-southeast-2_c3e90e272fa4fd26).
  2. Run the tshark command:
    - Open a terminal and run the following command to view the mirrored traffic:
    ```plaintext
    sudo tshark -i vxlan0 -Y http -T fields -e ipv6.src -e ipv6.dst -e http.request.method -e http.request.uri -e http.file_data
    ```

### Tips
**Configuring a Mirror Target**
- A mirror target can be EC2 instances, Network Load Balancers, or Gateway Load Balancers. In this workshop, we will use an EC2 instance as the target.
- The Traffic Mirroring interface requires you to specify an Elastic Network Interface (ENI) rather than directly specifying the EC2 instance.
- You can find the ENI ID of mirrorInstance in the [Lab D CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DLabD%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=wQc7X0stB0KmBa7-wT73ACi4nL4ZpsXY_MmpWkVJXFc).

**Configuring a Mirror Filter**
- A mirror filter helps determine what type of traffic will be mirrored to the target. You can include and/or exclude specific types of traffic (based on source/destination port, source/destination IP, protocol) through rules.
- The filter will process rules in the specified order, stopping when a match is found.
- In this lab, we want to mirror traffic leaving LabVpc to port 443 on the internet. Two rules are needed:
  + One rule for outbound requests to port 443.
  + One rule for responses from port 443 back to the original source port.
- Each rule must be specific to either IPv4 or IPv6, so a total of 4 rules are needed.

**Starting a Mirror Session**
- Each mirror session represents a single mirror source, sending traffic matching a mirror filter to a single mirror target.
- Mirror filters can be reused in multiple sessions, and a mirror target can be used by multiple mirror sessions. However, a source can only be associated with a single mirror session.
- The mirror source is identified by the Elastic Network Interface (ENI), rather than the instance ID. You can find the ENI ID of publicInstance in the [Lab D CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DLabD%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=wQc7X0stB0KmBa7-wT73ACi4nL4ZpsXY_MmpWkVJXFc).

**Lab D Architecture**
![D1](/images/structure/D1.png)

[Lab D Guide](4.2.1-WD/_index.en.md)