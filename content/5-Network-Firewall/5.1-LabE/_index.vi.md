---
title : "Lab E: Thiết lập định tuyến cho Network Firewall"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---
### Mục tiêu
- Bạn cần cấu hình một tường lửa để kiểm tra toàn bộ lưu lượng IPv6 đáng ngờ vào và ra khỏi mạng LabVpc do có báo cáo về hoạt động bất thường.

### Điểm khởi đầu
- Bạn cần cấu hình bảng định tuyến để đảm bảo lưu lượng vào và ra VPC đi qua các điểm cuối AWS Network Firewall trong FwSubnets để kiểm tra.

### Cơ sở hạ tầng
- AWS Network Firewall (với các điểm cuối trong mỗi AZ).
- LabVpc/FwSubnet1 & LabVpc/FwSubnet2 (một cái trong mỗi AZ).

### Dịch vụ được sử dụng
Amazon VPC - Route Tables [Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
AWS Network Firewall [Documentation](https://docs.aws.amazon.com/network-firewall/latest/developerguide/what-is-aws-network-firewall.html)

### Tiêu chí thành công
- Cấu hình bảng định tuyến IPv6 cho VPC để đảm bảo rằng tất cả lưu lượng truy cập vào/ra đều được kiểm tra bởi AWS Network Firewall.
- Đảm bảo rằng các điểm cuối của firewall được liên kết đúng cách với VPC và các subnet liên quan.
- Kiểm tra khả năng truy cập internet từ publicInstance sau khi cập nhật bảng định tuyến.

### Gợi ý
**Working with IPv6**

Làm việc với IPv6 Nếu bạn chưa từng làm việc với IPv6 trước đây, có một vài điều cần nhớ:
- Đường định tuyến mặc định cho IPv6 là ::/0 (tương đương với 0.0.0.0/0 cho IPv4).
- Khi phân bổ một khối CIDR IPv6 cho VPC, VPC sẽ nhận được một khối địa chỉ IPv6 công cộng /56; mỗi subnet sẽ nhận được một phân bổ /64 từ khối này.

**Working with VPC routing**
- Để cấu hình định tuyến trong VPC, hãy nghĩ về từng bước nhảy của gói tin từ điểm xuất phát đến đích.
- Bước nhảy tiếp theo được xác định bởi các tuyến trong bảng định tuyến của subnet.
- Hãy hỏi nhân viên AWS nếu gặp khó khăn.

**Ingess Route table**
Configure the Ingress Route table [Ingress Routing](https://aws.amazon.com/vi/blogs/aws/new-vpc-ingress-routing-simplifying-integration-of-third-party-appliances/)

**Kiến trúc phòng thí nghiệm E**
![E1](/images/structure/E1.png)

[Hướng dẫn Lab E](5.1.1-WE/_index.vi.md)