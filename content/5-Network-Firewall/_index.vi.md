---
title : "Theo dõi: Tường lửa mạng AWS (IPv6)"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
### Tổng quan về phòng thí nghiệm:
Công ty của bạn đã quyết định rằng tất cả lưu lượng IPv6 sẽ được kiểm tra bởi AWS Network Firewall. Ngoài ra, tất cả lưu lượng IPv4 sẽ được kiểm tra bởi một firewall mã nguồn mở của bên thứ ba (dựa trên Suricata), chạy phía sau Gateway Load Balancer (triển khai vào một VPC riêng biệt). Các phòng thí nghiệm E và F sẽ tập trung vào việc cấu hình AWS Network Firewall với IPv6.
- [Lab E:](5.1-LabE/_index.vi.md) sẽ cấu hình AWS Network Firewall để kiểm tra lưu lượng IPv6 đi vào và rời khỏi VPC.
- [Lab F:](5.2-LabF/_index.vi.md) sẽ bao gồm các tác vụ liên quan đến việc tạo quy tắc không trạng thái và có trạng thái trong AWS Network Firewall.