---
title : "Lab H: Thiết lập định tuyến cho Gateway Load Balancer"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 6.2 </b> "
---
### Mục tiêu
Bạn cần cấu hình định tuyến trong Lab VPC để đảm bảo toàn bộ lưu lượng IPv4 được chuyển đến tường lửa Suricata qua các điểm cuối Gateway Load Balancer (GWLB). Điều này sẽ cho phép Suricata kiểm tra toàn bộ lưu lượng IPv4 vào và ra qua IGW. Tường lửa này đã được cấu hình sẵn để chặn tất cả lưu lượng truy cập đến https://www.facebook.com qua IPv4.

### Điểm khởi đầu
-  Trong bài lab trước, bạn đã tạo một dịch vụ GWLB (Gateway Load Balancer) Endpoint trong VPC GWLB. Bây giờ, bạn cần tạo một endpoint trong FW Subnet của Lab VPC. Để kiểm tra tất cả lưu lượng đến và đi, bạn cần cấu hình bảng định tuyến để đảm bảo rằng lưu lượng sẽ đi qua các AWS Gateway Load Balancer endpoints trong FwSubnets trước khi rời khỏi và khi vào VPC.

### Cơ sở hạ tầng triễn khai sẵn
- AWS GWLB service endpoint (từ bài lab trước).
- LabVpc/FwSubnet1 và LabVpc/FwSubnet2 (một trong mỗi AZ).
{{% notice note %}}
Lưu ý: Nếu bạn đã hoàn thành bài lab E (cấu hình định tuyến firewall NFW), thì một trong các nhiệm vụ dưới đây (gán bảng định tuyến edge với IGW của LabVpc) có thể đã được thực hiện. Bạn vẫn cần thêm các tuyến vào bảng định tuyến này.
{{% /notice %}}

### Dịch vụ được sử dụng
- Amazon VPC- Route Tables [Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
- AWS Gateway Load Balancer - [Documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/gateway/getting-started.html)

### Tiêu chí thành công
- Bạn đã tạo/sử dụng lại bảng định tuyến cạnh và liên kết nó với LabVpc IGW.
- Bạn đã thêm một tuyến đường cho mỗi khối IPv4 PubSubnet CIDR vào bảng tuyến đường biên thông qua các điểm cuối GWLB.
- Bạn đã thêm một tuyến đường mặc định từ mỗi FWSubnet tới IGW.
- Bạn đã cập nhật các tuyến IPv4 mặc định từ mỗi PubSubnet đến các điểm cuối GWLB.
- Bạn đã kiểm tra rằng bạn vẫn có thể truy cập Internet từ `publicInstance` *Remote Access thông qua Systems Manager Session Manager*. Để kiểm tra điều này, hãy kết nối với và ping bất kỳ trang web công cộng nào.
- Bạn đã kiểm tra và không thể truy cập [www.facebook.com](http://www.facebook.com/) `publicInstance` *Remote Access qua Systems Manager Session Manager* [Lab H CloudWatch Dashboard](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabH) từ. Để kiểm tra điều này, hãy kết nối với và ping bất kỳ trang web công cộng nào. Đảm bảo rằng tất cả các tác vụ được xác định trên đã được hoàn thành.

### Gợi ý
**Ingess Route table**
- Configure the Ingress Route table [Ingress Routing](https://aws.amazon.com/vi/blogs/aws/new-vpc-ingress-routing-simplifying-integration-of-third-party-appliances/)

**How to associate a route table to a subnet**
- Associate Route table to a Subnet [Associate](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html#subnet-route-tables) to the relevant subnets.

**Multiple GWLB endpoints needed**
- Dù bạn có thể chọn nhiều subnet khi tạo một GWLB endpoint, mỗi GWLB endpoint chỉ có thể được liên kết với một subnet duy nhất. Do đó, bạn cần tạo hai GWLB endpoints, mỗi endpoint dành cho một AZ khác nhau.

**Kiến trúc phòng thí nghiệm H**
![H1](/images/structure/H1.png)

**How to test?**
- Kết nối với publicInstance qua AWS Session Manager:
  + Sử dụng AWS Systems Manager Session Manager để kết nối với publicInstance.
- Chạy lệnh kiểm tra:
  + Nhập lệnh sau vào terminal của publicInstance:
    ```markdown
    curl -v -4 https://www.facebook.com
    ```
- Kết quả mong đợi:
  + Lệnh trên nên bị timeout, có nghĩa là yêu cầu không thành công vì lưu lượng truy cập tới www.facebook.com đang bị chặn bởi firewall.

[Hướng dẫn Lab H](6.2.1-WH/_index.vi.md)