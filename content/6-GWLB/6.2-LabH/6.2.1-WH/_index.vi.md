---
title : "Hướng dẫn Lab H: Thiết lập định tuyến cho Gateway Load Balancer"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 6.2.1 </b> "
---
### Configuring
1. Tạo GWLB Endpoint:
   - Tạo một GWLB endpoint trong mỗi subnet Firewall (FW) ở LabVpc để chuyển hướng lưu lượng.
2. Cập nhật bảng định tuyến:
   - Thay đổi các bảng định tuyến để đảm bảo rằng tất cả lưu lượng đi đến hoặc từ IGW được kiểm tra bởi firewall Suricata.

{{% notice note %}}
Firewall Suricata đã được cấu hình sẵn với hai quy tắc: Quy tắc đầu tiên chặn lưu lượng đến https://www.facebook.com. Quy tắc thứ hai chặn ICMP ping đến 9.9.9.9.
{{% /notice %}}

### Create Endpoint in LabVpc
1. Đến VPC -> Endpoints và click tạo Endpoint
- **Name**: GWLB-endpoint-1
- **Service Category**: Other Endpoint services
- **Service Settings**: paste the GWLB's 'service name created in the previous lab.

Nếu bạn không ghi chú lại tên endpoint của mình, hãy nhấp vào liên kết [here](https://ap-southeast-2.signin.aws.amazon.com/oauth?client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fvpcconsole&code_challenge=qOqEpqCBj9MZuHfJD2RtruSSCuvThI37dR3pdmDDfIk&code_challenge_method=SHA-256&response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fvpcconsole%2Fhome%3FhashArgs%3D%2523EndpointServices%26isauthcode%3Dtrue%26oauthStart%3D1726717506767%26state%3DhashArgsFromTB_ap-southeast-2_fc379777bbcacefc) và kiểm tra tên dịch vụ trong tab Chi tiết.

![H2](/images/structure/H2.png)

- Nhấp vào "Verify Service". Nó sẽ hiển thị thông báo xác nhận tên dịch vụ.
- Kiểm tra Subnet
![H3](/images/structure/H3.png)
![H4](/images/structure/H4.png)
- Sau khi xác nhận, bạn sẽ có tùy chọn chọn VPC.
- VPC: Chọn LabVpc.
- Subnets: Chọn LabVpc/FWSubnet1.
- Click *Create*
![H5](/images/structure/H5.png)
{{% notice note %}}
Mặc dù có thể chọn nhiều subnet tại bước này, mỗi endpoint của GWLB chỉ có thể được liên kết với một subnet duy nhất. Do đó, bạn cần tạo hai endpoint GWLB, mỗi endpoint cho một AZ khác nhau.
{{% /notice %}}

**Lặp lại các bước trên cho subnet LabVpc/FWSubnet2**
![H6](/images/structure/H6.png)
![H7](/images/structure/H7.png)

2. Đi đến VPC -> Endpoint Services.
3. Nhấp vào GWLB-Endpoint.
4. Chọn tab "Endpoint connections".
5. Chọn điểm cuối mà bạn muốn chấp nhận yêu cầu kết nối.
6. Nhấp vào "Actions" và chọn "Accept endpoint connection request".
![H8](/images/structure/H8.png)

### Add default route via IGW to the FW route tables
Đây là cách các bảng định tuyến sẽ xử lý cấu hình sau:
![H9](/images/structure/H9.png)
1. Trong AWS Management Console, bắt đầu gõ "VPC" vào ô tìm kiếm nhanh ở trên cùng và nhấn Enter.
2. Đi đến VPC → Bảng định tuyến (Route Tables) và lọc theo LabVpc.
3. Chọn LabVpc/FwSubnet1RouteTable và nhấp vào tab Routes. Nhấp vào Edit Routes.
![H10](/images/structure/H10.png)
4. Thêm một tuyến đường và cấu hình để tất cả lưu lượng truy cập nên đi đến internet gateway.
- **Destination**: 0.0.0.0/0
- **Target**: igw-xxxx..
![H13](/images/structure/H13.png)
5. Làm lại các bước trên cho **LabVpc/FwSubnet2RouteTable**
![H14](/images/structure/H14.png)
![H15](/images/structure/H15.png)

**Chúc mừng bạn đã hoàn thành Lab H!**