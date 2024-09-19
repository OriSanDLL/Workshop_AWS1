---
title : "Các phương pháp tiếp cận bảo mật nhiều lớp cho Amazon VPC"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
---
# Các phương pháp tiếp cận bảo mật nhiều lớp cho Amazon VPC

### Mục đích

Trong Workshop này, sẽ đề cập đến hướng dẫn thực tế để bảo mật Amazon VPC. Nó nhắm đến các kiến trúc sư đám mây, quản trị viên hệ thống, quản trị viên mạng và quản trị viên bảo mật, những người chịu trách nhiệm thiết kế, phát triển và chạy các dịch vụ trong AWS.

Vì vậy, bạn phải biết những điều cơ bản như subnet, security groups, VPC flow logs. Trọng tâm sẽ là phạm vi dịch vụ và tính năng bổ sung mà AWS cung cấp cho phép bạn vận hành VPC một cách an toàn. Tại đây, sẽ đề cập đến AWS Network Firewall, VPC Network Access Analyzer, Amazon Route 53 Resolver DNS Firewall, AWS Systems Manager, Traffic Mirroring, AWS WAF, Gateway Load Balancer, v.v. Đồng thời, sẽ sử dụng IPv6 trong một số phòng thí nghiệm - nhưng đừng lo lắng, hãy đọc và làm theo từng bước sẽ giúp bạn hiểu hơn và lắm vững kiến thức!

Tất cả các ví dụ được đưa ra sẽ tuân theo các biện pháp thực hành tốt nhất về bảo mật, thiết kế và quản lý Amazon VPC. Cuối cùng, bạn nên có một sự hiểu biết về cách thức và lý do tại sao mỗi trong số này có thể được sử dụng, và kiến thức cần thiết để áp dụng những gì bạn đã học vào môi trường của riêng bạn.

### Nội dung

 1. [Tổng quan về Hội Thảo](1-Introduce/_index.vi.md)
 2. [Các bước chuẩn bị](2-Prepare/_index.vi.md)
 3. [Theo dõi: Bảo mật DNS](3-DNS-Security/_index.vi.md)
 4. [Theo dõi: Phân tích mạng](4-Network-Analysis/_index.vi.md)
 5. [Theo dõi: Tường lửa mạng AWS (IPv6)](5-Network-Firewall/_index.vi.md)
 6. [Theo dõi: Tường lửa của bên thứ 3 sử dụng Gateway Load Balancer (GWLB)](6-GWLB/_index.vi.md)
 7. [Theo dõi: Tường lửa ứng dụng web](7-WAF/_index.vi.md)
 8. [Dọn dẹp](8-Cleanups/_index.vi.md)