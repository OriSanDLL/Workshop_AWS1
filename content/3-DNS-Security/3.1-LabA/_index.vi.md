---
title : "Lab A: Tăng cường khả năng hiển thị hoạt động của VPC"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---
### Tổng quan:
1. **Nhận thông báo từ bên thứ ba** về hoạt động đáng ngờ đến từ bên ngoài trong cơ sở hạ tầng cá nhân.
2. **Nhiệm vụ đầu tiên**: Bật **ghi nhật ký truy vấn DNS** để nắm bắt các yêu cầu truy vấn DNS.
3. Mục tiêu: **Xác định nguồn và đích** của các truy vấn để điều tra hoạt động đáng ngờ.
    
Việc này sẽ hỗ trợ trong việc theo dõi và xử lý các sự cố bảo mật liên quan đến truy vấn DNS.

### Điểm khởi đầu:
Hãy xem xét cách ta có thể nắm bắt và ghi lại các truy vấn DNS trong VPC của mình, sau đó cách bạn có thể phân tích các nhật ký đó để tìm kiếm hoạt động đáng ngờ. Một số trường hợp sẽ hữu ích để tạo các truy vấn của riêng bạn là:
   - **query_name:** tên miền đang được truy vấn (ví dụ: test.example.com.)
   - **srcids.instance:** id phiên bản EC2 đã thực hiện truy vấn
   - **rcode:** mã phản hồi được tạo ra bởi trình phân giải Route 53 để phản hồi truy vấn DNS
   - **answers.0.Rdata:** phản hồi của truy vấn (IPv4 hoặc IPv6)
    
Các truy vấn mẫu có thể được [tìm thấy ở đây](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax-examples.html).

### Cơ sở hạ tầng được triển khai trước
Có một Amazon Cloudwatch Log Group ( `"Route53Resolver"`) được cấu hình sẵn mà ta có thể sử dụng khi cấu hình ghi nhật ký truy vấn DNS. Hãy nhớ kiểm tra xem bảng điều khiển cá nhân có hiển thị ta đang làm việc ở đúng vùng cho hội thảo hay không.
    
### Dịch vụ được sử dụng
   - Ghi nhật ký truy vấn DNS Resolver của Amazon Route 53 - [Tài liệu](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-query-logs.html)
   - Amazon CloudWatch Log Insights - [Tài liệu](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)
### Tiêu chí thành công
   - Bạn đã bật thành công khả năng ghi nhật ký truy vấn DNS cho **LabVpc** VPC.
        
   - Bạn đã xác định được tên DNS của trang web không xác định mà bạn nghi ngờ là có vấn đề
    
Đảm bảo rằng tất cả các tác vụ được xác định trên [Bảng điều khiển Lab A CloudWatch](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabA) đã được hoàn thành.

### Gợi ý
**Để bật ghi nhật ký truy vấn**
   - Nhật ký truy vấn cho VPC được tìm thấy trong [Amazon Route 53 Resolver](https://console.aws.amazon.com/route53resolver/home) phần của bảng điều khiển AWS.
            + Hãy đảm bảo bạn đã chuyển sang khu vực nơi xưởng của bạn đang hoạt động.
**Để phân tích nhật ký truy vấn**
   - Sau khi bạn ghi lại nhật ký, bạn có thể bắt đầu chạy truy vấn đối với dữ liệu bằng [CloudWatch Log Insights](https://console.aws.amazon.com/cloudwatch/home#logsV2:logs-insights).
   - Một số trường sẽ hữu ích cho việc tạo truy vấn của riêng bạn là:
      + **query_name:** tên miền đang được truy vấn (ví dụ: "test.example.com.")
      + **srcids.instance:** id phiên bản EC2 đã thực hiện truy vấn
      + **rcode:** mã phản hồi được tạo ra bởi trình phân giải Route 53 để phản hồi truy vấn DNS
      + **answers.0.Rdata:** phản hồi của truy vấn (IPv4 hoặc IPv6)
   - Các truy vấn ví dụ có thể được tìm thấy [ở đây](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax-examples.html)
   - Nếu bạn đang gặp khó khăn, chúng tôi đã tạo một số truy vấn được lưu trước trong hội thảo - bạn có thể tìm thấy chúng trong trang Log Insights, ở phía bên phải, nút bên dưới nút "Discovered Fields"
{{% notice note %}}
Lưu ý rằng có thể mất vài phút từ lúc bật ghi nhật ký truy vấn đến lúc kết quả xuất hiện trong CloudWatch Logs Insights - hãy kiên nhẫn!
{{% /notice %}}   
        
**Kiến trúc Phòng thí nghiệm A**
![A](/images/structure/A1.png)

[**Hướng đẫn Lab A**](3.1.1-WA/_index.vi.md)