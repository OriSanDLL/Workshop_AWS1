---
title : "Kiến Trúc"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 1.1 </b> "
---
###  Sơ đồ kiến trúc
Tổng quan về kiến trúc ban đầu được hiển thị bên dưới:
![Architecture](/images/structure/intro-architecture.png)

- Hội thảo tập trung vào **"LabVpc"**, mô phỏng hạ tầng với hai Vùng sẵn sàng (AZ) và triển khai 4 tầng mạng:
  + Mạng con công cộng (PubSubnets): Có tuyến đường mặc định đến Cổng Internet, chứa NAT Gateway, Application Load Balancer (webAlb), và EC2 instance.
  + Mạng con riêng (PriSubnets): Có tuyến mặc định đến các cổng NAT trong PubSubnets, chứa Auto Scaling Group cho máy chủ web EC2 và một EC2 để xử lý lưu lượng phản chiếu (mirrorInstance).
  + Mạng con bị cô lập (EndSubnets): Không có tuyến mặc định, chứa các điểm cuối PrivateLink cho AWS services như CloudFormation, CloudWatch, và Systems Manager.
  + Mạng con tường lửa (FwSubnets): Chưa có tuyến mặc định nhưng sẽ dùng AWS Network Firewall sau này.

Một số thành phần khác đã được triển khai sẵn, như AWS Network Firewall, Amazon CloudFront (có webAlb làm gốc), và CloudWatch Log Groups. Liên kết đến các tài nguyên này có thể tìm thấy trên [CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DIntro%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=mIJhXAMe_gxLDCJtBz3QaX-B7dmpdF-_ybkJ4TR6JBA) sau khi đăng nhập vào tài khoản AWS hoặc triển khai CloudFormation stack vào tài khoản cá nhân.

### Cơ sở hạ tầng bổ sung
- Hội thảo sử dụng một VPC bổ sung (ExtVpc) để mô phỏng các mục tiêu Internet, chứa mạng con công khai và một ALB hướng tới Internet. ALB này cung cấp phản hồi Lambda đơn giản để cung cấp thông tin.

- Ngoài ra, một số hàm Lambda được chạy mỗi phút thông qua Amazon EventBridge để theo dõi trạng thái của cơ sở hạ tầng hội thảo và kiểm tra các tác vụ đã hoàn thành.

- Không cần thay đổi bất kỳ thành phần nào của cơ sở hạ tầng bổ sung trong suốt hội thảo.