---
title : "Theo dõi: Tường lửa ứng dụng web"
date :  "`r Sys.Date()`" 
weight : 7
chapter : false
pre : " <b> 7. </b> "
---
### Tổng quan về phòng thí nghiệm
[Lab I:](7.1-LabI/_index.vi.md) Để bảo vệ WebAlb (Application Load Balancer) khỏi các đợt tăng đột ngột về lưu lượng hoặc các cuộc tấn công DDoS, chúng ta muốn đảm bảo rằng tất cả lưu lượng đến WebAlb phải đi qua một phân phối Amazon CloudFront, và không trực tiếp từ người dùng cuối. Mục tiêu phụ là đảm bảo rằng chỉ lưu lượng từ một phân phối CloudFront cụ thể mới được phép, thay vì bất kỳ phân phối CloudFront nào.