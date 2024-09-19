---
title : "Lab I: Bảo vệ ứng dụng web"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 7.1 </b> "
---
### Mục tiêu
- Bảo vệ WebAlb: Đảm bảo rằng toàn bộ lưu lượng truy cập đến WebAlb (Application Load Balancer) chỉ đến từ một Amazon CloudFront distribution, không phải từ các người dùng khác (ngoại trừ từ máy tính cá nhân của bạn).
- Giới hạn lưu lượng từ CloudFront cụ thể: Chỉ cho phép lưu lượng từ một CloudFront distribution cụ thể được phép truy cập WebAlb, thay vì bất kỳ CloudFront distribution nào có thể biết đến WebAlb.

### Điểm khởi đầu
1. Đảm bảo Lưu lượng Đến WebAlb chỉ từ CloudFront:
   - Xem xét các phương pháp để đảm bảo rằng tất cả lưu lượng truy cập đến WebAlb chỉ đến từ cơ sở hạ tầng CloudFront.
2. Sử dụng AWS WAF để Kiểm Tra Lưu lượng L7:
   - Sau khi đảm bảo lưu lượng chỉ đến từ CloudFront, cân nhắc sử dụng dịch vụ AWS WAF để kiểm tra lưu lượng ở tầng 7 (L7) và xác nhận rằng yêu cầu đến từ CloudFront distribution mà bạn kiểm soát.

### Cơ sở hạ tầng triển khai trước
1. Application Load Balancer (WebAlb):
   - Là một Application Load Balancer hướng Internet, đang lắng nghe trên cổng HTTP 80.
   - Phía sau WebAlb có một số instances EC2 cung cấp nội dung.
2. Amazon CloudFront distribution:
   - Đã được cấu hình để chèn một tiêu đề HTTP tùy chỉnh vào các yêu cầu nó gửi đến cơ sở hạ tầng gốc (WebAlb).
3. CloudFront distribution thứ hai:
   - Không được cấu hình để chèn tiêu đề tùy chỉnh (dùng cho mục đích kiểm soát và demo).
{{% notice note %}}
Lưu ý: Các CloudFront distributions sẽ trả về lỗi 504 lúc đầu, điều này là bình thường và sẽ được khắc phục sau khi hoàn thành nhiệm vụ đầu tiên.
{{% /notice %}}

### Dịch vụ được sử dụng
- Amazon CloudFront - [Documentation](https://docs.aws.amazon.com/cloudfront/)
- AWS Web Application Firewall (WAF) - [Documentation](https://docs.aws.amazon.com/waf/)
- Amazon VPC Security Groups

### Tiêu chí thành công
- Đảm bảo rằng Application Load Balancer (WebAlb) có thể truy cập được từ các Amazon CloudFront distributions và từ laptop của bạn
- Ngoài ra, đảm bảo rằng WebAlb chỉ có thể truy cập từ Amazon CloudFront distribution chứa custom HTTP header.

### Gợi ý
**Restricting traffic to CloudFront distributions**
- CloudFront có các node trên toàn cầu, và địa chỉ IP của chúng có thể được tìm thấy trực tuyến [here](https://docs.aws.amazon.com/vpc/latest/userguide/aws-ip-ranges.html).
- Trước đây, người dùng phải tự thêm các địa chỉ IP này vào security group.
- AWS hiện cung cấp một [managed prefix list](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/LocationsOfEdgeServers.html#managed-prefix-list), giúp tự động hóa quá trình này.

**Filtering by HTTP header on the WebAlb Application Load Balancer**
- CloudFront được cấu hình chèn tiêu đề "OriginSig" vào các yêu cầu đến ALB.
- ALB không có khả năng lọc lưu lượng theo tiêu đề nhưng có thể liên kết với AWS WAF [[here]](https://us-east-1.console.aws.amazon.com/wafv2/homev2/web-acls?region=us-west-2).
- Tạo quy tắc trong AWS WAF để chặn lưu lượng không có tiêu đề "OriginSig"[[here]](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rules.html).

**Kiến trúc phòng thích nghiệm I**
![I1](/images/structure/I1.png)

[Hướng dẫn Lab I](7.1.1-WI/_index.vi.md)