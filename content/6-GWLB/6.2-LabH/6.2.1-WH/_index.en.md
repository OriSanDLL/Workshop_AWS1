---
title : "Lab H Guide: Configuring Routing for Gateway Load Balancer"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 6.2.1 </b> "
---
### Configuring
1. Create GWLB Endpoint:
   - Create a GWLB endpoint in each Firewall (FW) subnet in LabVpc to redirect traffic.
2. Update route tables:
   - Modify the route tables to ensure all traffic to or from the IGW is inspected by the Suricata firewall.

{{% notice note %}}
The Suricata firewall has been pre-configured with two rules: The first rule blocks traffic to https://www.facebook.com. The second rule blocks ICMP ping to 9.9.9.9.
{{% /notice %}}

### Create Endpoint in LabVpc
1. Go to VPC -> Endpoints and click create Endpoint
- **Name**: GWLB-endpoint-1
- **Service Category**: Other Endpoint services
- **Service Settings**: paste the GWLB's service name created in the previous lab.

If you did not note down your endpoint name, click the link [here](https://ap-southeast-2.signin.aws.amazon.com/oauth?client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fvpcconsole&code_challenge=qOqEpqCBj9MZuHfJD2RtruSSCuvThI37dR3pdmDDfIk&code_challenge_method=SHA-256&response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fvpcconsole%2Fhome%3FhashArgs%3D%2523EndpointServices%26isauthcode%3Dtrue%26oauthStart%3D1726717506767%26state%3DhashArgsFromTB_ap-southeast-2_fc379777bbcacefc) and check the service name in the Details tab.

![H2](/images/structure/H2.png)

- Click "Verify Service". It will display a confirmation message for the service name.
- Check Subnet
![H3](/images/structure/H3.png)
![H4](/images/structure/H4.png)
- After confirmation, you will have the option to select the VPC.
- VPC: Select LabVpc.
- Subnets: Select LabVpc/FWSubnet1.
- Click *Create*
![H5](/images/structure/H5.png)
{{% notice note %}}
Although you can select multiple subnets at this step, each GWLB endpoint can only be associated with a single subnet. Therefore, you need to create two GWLB endpoints, one for each AZ.
{{% /notice %}}

**Repeat the above steps for the LabVpc/FWSubnet2 subnet**
![H6](/images/structure/H6.png)
![H7](/images/structure/H7.png)

2. Go to VPC -> Endpoint Services.
3. Click on GWLB-Endpoint.
4. Select the "Endpoint connections" tab.
5. Select the endpoint you want to accept the connection request for.
6. Click "Actions" and select "Accept endpoint connection request".
![H8](/images/structure/H8.png)

### Add default route via IGW to the FW route tables
This is how the route tables will handle the configuration afterward:
![H9](/images/structure/H9.png)
1. In the AWS Management Console, start typing "VPC" into the quick search box at the top and press Enter.
2. Go to VPC → Route Tables and filter by LabVpc.
3. Select LabVpc/FwSubnet1RouteTable and click on the Routes tab. Click Edit Routes.
![H10](/images/structure/H10.png)
4. Add a route and configure it so that all traffic should go to the internet gateway.
- **Destination**: 0.0.0.0/0
- **Target**: igw-xxxx..
![H13](/images/structure/H13.png)
5. Repeat the above steps for **LabVpc/FwSubnet2RouteTable**
![H14](/images/structure/H14.png)
![H15](/images/structure/H15.png)

**Congratulations on completing Lab H!**