---
title : "Theo dõi: Bảo mật DNS"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 3. </b> "
---
### Tổng quan về phòng thí nghiệm:

- [**Lab A**](3.1-LabA/_index.vi.md): Bạn đã nhận được báo cáo từ bên thứ ba về hoạt động đáng ngờ đến từ một phần nào đó trong hạ tầng của bạn. Công việc đầu tiên của bạn là kích hoạt việc ghi nhật ký truy vấn DNS để có thể ghi lại các yêu cầu này - điều này sẽ giúp xác định cả nguồn và đích của các truy vấn.

- [**Lab B**](3.2-LabB/_index.vi.md): Sau khi xác định các yêu cầu đi từ VPC của bạn đến một trang web đáng ngờ, nhiệm vụ tiếp theo là chặn nó. Bạn sẽ xem xét cách Amazon Route 53 DNS Firewall có thể ngăn tài nguyên trong VPC giải quyết địa chỉ IP của tên miền đáng ngờ này.
{{% notice note %}}
Lưu ý: Thỉnh thoảng bạn có thể thấy bảng điều khiển AWS chuyển về khu vực US East (N Virginia), đặc biệt khi làm việc với Route 53; hãy đảm bảo rằng bạn chuyển về khu vực US West (Oregon) (hoặc khu vực bạn được yêu cầu làm việc).
{{% /notice %}}
