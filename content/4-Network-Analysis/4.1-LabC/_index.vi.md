---
title : "Lab C: Phân tích cấu hình mạng VPC"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---
### Mục tiêu
- Sau khi phát hiện lưu lượng DNS đáng ngờ trong LabVpc, ta cần kiểm tra cấu hình bảo mật để xác định tài nguyên VPC nào có thể nhận kết nối từ internet. Mục tiêu là đảm bảo các tài nguyên này đáp ứng yêu cầu tuân thủ và bảo mật. Nếu phát hiện truy cập không mong muốn, ta sẽ cần xóa nó.

### Khởi điểm
- AWS cung cấp các công cụ để giúp ta xác định và hiển thị quyền truy cập được phép vào VPC của mình dựa trên cấu hình hiện tại, thay vì phải kiểm tra thủ công từng nhóm bảo mật và cấu hình Network ACL trong các môi trường lớn.

### Dịch vụ sử dụng
- Trình phân tích truy cập mạng Amazon VPC - [Tư liệu](https://docs.aws.amazon.com/vpc/latest/network-access-analyzer/what-is-network-access-analyzer.html)

### Tiêu chí thành công
- Xác định các biện pháp kiểm soát truy cập VPC cho phép kết nối không mong muốn (ví dụ: cổng SSH mở Internet) vào VPC và xóa quyền truy cập này.

### Gợi ý
**Để tìm thông tin chuyên sâu về cấu hình truy cập mạng**
- Phân tích truy cập mạng có thể được tìm thấy [trong bảng điều khiển Trình quản lý mạng](https://us-west-2.signin.aws.amazon.com/oauth?client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fnetworkmanager&code_challenge=gJlDcvz1ZMOURphAAzCNh3LUQ2lp84w54jZpfTNc1FI&code_challenge_method=SHA-256&response_type=code&redirect_uri=https%3A%2F%2Fus-west-2.console.aws.amazon.com%2Fnetworkmanager%2Fhome%3FhashArgs%3D%2523%252F%26isauthcode%3Dtrue%26oauthStart%3D1726669137896%26state%3DhashArgsFromTB_us-west-2_12ab578f6b8aae3b).
  + Mặc dù bạn có thể sử dụng phạm vi mặc định của AWS, nhưng bạn có thể nhận được nhiều kết quả không liên quan đến phòng thực hành. Hãy suy nghĩ về cách bạn có thể thu hẹp phạm vi đến LabVpc.
  + Nếu bạn muốn có một mục tiêu kéo dài, cũng hãy suy nghĩ về cách bạn có thể loại trừ lưu lượng truy cập "tốt đã biết" khỏi phân tích.
{{% notice note %}}
Lưu ý rằng (kể từ tháng 11/2023), Network Access Analyzer chỉ hỗ trợ phân tích IPv4.
{{% /notice %}}
**Kiến trúc phòng thí nghiệm C**
![C1](/images/structure/C1.png)

[Hướng dẫn Lab C](4.1.1-WC/_index.vi.md)