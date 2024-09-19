---
title : "Lab B: Triển khai tường lửa DNS"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---
### Tổng Quan:
1. **Xác định trang web đáng ngờ**: ""bad.sa-demos.net"" là mục tiêu của nhiều lưu lượng truy cập.
2. **Nhiệm vụ tiếp theo**: Chặn trang web này để ngăn truy cập trong khi tiếp tục điều tra.
3. Giải pháp: **Triển khai và cấu hình cơ chế** để ngăn các phiên bản trong VPC có thể phân giải tên miền đó.
    
Mục tiêu là tạm thời chặn truy cập vào tên miền đáng ngờ từ VPC nhằm đảm bảo an toàn trong quá trình điều tra.

### Điểm khởi đầu:
1. Có nhiều cách tiếp cận để **từ chối quyền truy cập** vào miền đáng ngờ, như **nhóm bảo mật VPC** hoặc **ACL mạng**, nhưng chúng chỉ chặn lưu lượng đến các địa chỉ IP cụ thể.
2. Nếu trang web độc hại chuyển sang **địa chỉ IP khác**, các lệnh chặn này sẽ mất hiệu lực.
3. Thay vào đó, trong lab này, sẽ sử dụng **tường lửa DNS** để ngăn các tài nguyên trong VPC phân giải địa chỉ IP của miền đáng ngờ.
    
Mục tiêu là dùng tường lửa DNS để chặn tên miền một cách hiệu quả, bất kể thay đổi IP.

### Dịch vụ được sử dụng
- Tường lửa DNS của Amazon Route 53 Resolver - [Tài liệu](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-dns-firewall.html)
- Amazon CloudWatch Log Insights - [Tài liệu](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)

### Tiêu chí thành công
- Các phiên bản trong LabVpc không còn có thể giải quyết thành công địa chỉ IP của tên miền đáng ngờ mà bạn đã xác định trong Lab A ( `bad.sa-demos.net` [Bảng điều khiển CloudWatch của Lab B](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabB))sẽ hiển thị tiến trình của bạn.
- Lưu ý rằng các nỗ lực truy vấn tên miền đáng ngờ sẽ trả về phản hồi **NXDOMAIN**.

### Gợi ý
**Để bật và cấu hình tường lửa DNS**
Tường lửa DNS được tìm thấy trong [Amazon Route 53 Resolver](https://console.aws.amazon.com/vpc/home#DNSFirewallRuleGroups) phần của bảng điều khiển AWS. Quá trình này là:

1. Tạo [danh sách tên miền](https://console.aws.amazon.com/vpc/home#DNSFirewallDomainLists:)  có tên miền đáng ngờ bad.sa-demos.net. trong đó
2. Tạo [nhóm Quy tắc](https://console.aws.amazon.com/vpc/home#DNSFirewallRuleGroups) CHẶN (sử dụng phản hồi NXDOMAIN) các yêu cầu đến danh sách miền bạn đã tạo ở trên
3. Liên kết nhóm quy tắc với LabVpc.

**Để phân tích nhật ký truy vấn**
Bạn có thể kiểm tra xem các yêu cầu DNS có bị chặn hay không theo một số cách:
1. Thông qua [CloudWatch Log Insights](https://console.aws.amazon.com/cloudwatch/home#logsV2:logs-insights).
2. Kết nối với `publicInstance`[Bảng điều khiển CloudWatch của Lab B](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabB) `nslookup bad.sa-demos.net.`

**Kiến trúc Phòng thí nghiệm B**
![B1](/images/structure/B1.png)

[**Hướng đẫn Lab B**](3.2.1-WB/_index.vi.md)