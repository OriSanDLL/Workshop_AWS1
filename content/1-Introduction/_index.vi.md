---
title : "Giới thiệu"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
### VPC Network Access Analyzer là gì?
- Network Access Analyzer là một tính năng giúp xác định truy cập mạng ngoài ý muốn vào các tài nguyên của bạn trên AWS:
1. Hiểu, xác minh, và cải thiện tư thế bảo mật mạng của bạn:
   + Network Access Analyzer giúp bạn xác định các truy cập mạng ngoài ý muốn so với các yêu cầu bảo mật và tuân thủ của bạn, cho phép bạn thực hiện các bước cải thiện bảo mật mạng.
2. Chứng minh tuân thủ:
   + Bạn có thể sử dụng Network Access Analyzer để chứng minh rằng mạng của bạn trên AWS đáp ứng các yêu cầu tuân thủ nhất định.
3. Xác minh tư thế bảo mật mạng của bạn:
   + Network Access Analyzer cho phép bạn chỉ định các yêu cầu bảo mật mạng và xác định các đường dẫn mạng tiềm năng không đáp ứng các yêu cầu đó.

### Tại sao bạn cần Network Access Analyzer?
1. Vấn đề:
- Khi các tổ chức mở rộng môi trường cloud của họ và các nhóm ứng dụng phân tán bắt đầu vận hành hạ tầng mạng, việc xác định liệu có đủ các biện pháp kiểm soát mạng thích hợp để đạt được các mục tiêu bảo mật và tuân thủ của tổ chức trở nên khó khăn.
2. Xác thực kiểm soát mạng:
- Khách hàng gặp khó khăn trong việc xác thực kiểm soát mạng vì phải thực hiện kiểm tra và kiểm toán thủ công các thiết kế và cấu hình mạng, điều này không thể mở rộng quy mô và tốn nhiều thời gian.
- Các nhóm vận hành và mạng cần phải chứng minh sự hiện diện của các biện pháp kiểm soát mạng cho các yêu cầu bảo mật, đặc biệt trong các cuộc kiểm toán bên ngoài, khiến cho quy trình trở nên tẻ nhạt và không hiệu quả.
3. Các biện pháp kiểm soát mạng nhanh chóng chở nên lỗi thời:
- Do môi trường khách hàng luôn thay đổi với các yêu cầu bảo mật phát triển, thay đổi trong thiết kế mạng và sự ra mắt liên tục của các dịch vụ mạng AWS mới.
4. Các nhóm vận hành cố gắng đảm bảo môi trường tuân thủ bằng cách hạn chế nhà phát triển sử dụng một số dịch vụ AWS hoặc áp dụng quy trình phê duyệt thủ công cho các thay đổi cấu hình. Cả hai phương pháp này đều tạo áp lực cho nhóm vận hành và khiến các nhóm phát triển cảm thấy không thể di chuyển nhanh như mong muốn.

### VPC Reachability Analyzer là gì?
- Reachability Analyzer là một công cụ phân tích cấu hình cho phép bạn thực hiện kiểm tra kết nối giữa một tài nguyên nguồn và một tài nguyên đích trong các virtual private clouds (VPCs) của bạn.
- Chức năng chính:
  + Kiểm tra kết nối: Khi tài nguyên đích có thể truy cập, Reachability Analyzer cung cấp chi tiết từng bước của đường dẫn mạng ảo giữa nguồn và đích.
  + Xác định vấn đề: Nếu tài nguyên đích không thể truy cập, Reachability Analyzer xác định thành phần đang chặn. Các vấn đề này có thể do lỗi cấu hình trong security group, network ACL, route table hoặc load balancer.

### Sơ đồ kiến trúc
Kiến trúc này được thông qua AWS CloudFormation đã được cấu hình sẵn và sẽ sử dụng trong phòng thí nghiệm.
![image](/images/1/NetworkAccessAnalyzer.png)