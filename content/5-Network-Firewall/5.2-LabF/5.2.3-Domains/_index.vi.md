---
title : "Drop specific Domains"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 5.2.3 </b> "
---
### **Hướng dẫn chi tiết**
- AWS Network Firewall: Sử dụng hai động cơ quy tắc để kiểm tra gói tin.
- Cần Làm: Tạo quy tắc Stateless để chuyển tiếp tất cả lưu lượng đến động cơ Stateful.
![F13](/images/structure/F13.png)
1. Đi tới Console VPC và trên menu bên trái, chọn Firewall policies dưới Network firewall. Bạn sẽ tìm thấy một NFWPolicy, được tạo ra trong cấu hình ban đầu. Nhấp vào nó và chuyển đến Stateless rule groups, nhấp vào Actions -> Create stateless rule group.
2. Ở bước 1 - nhấp vào Tiếp theo. Ở bước 2, nhập các giá trị sau:
- **Name**: FwdToStateful
- **Capacity**: 100
- Click Next
- Under Add rule
  + **Priority**: 3
  + **Protocol**: Leave All selected
  + **Source**: Any IPv6 adress
  + **Destination**: Any IPv6 adress scroll down
  + **Rule action**: Forward to stateful rule groups
- Đảm bảo DropPingRule có mức ưu tiên 1, DropDNS có mức ưu tiên 2 và FwDtoStateful có mức ưu tiên 3.
![F14](/images/structure/F14.png)
3. Cuộn xuống phần Stateful rule groups và nhấp vào Actions -> Create stateful rule group.
![F15](/images/structure/F15.png)
4. Chọn Danh sách tên miền theo định dạng nhóm Quy tắc
![F16](/images/structure/F16.png)
5. 5. Ở bước 2. Nhập tên và Dung lượng = 100.
6. Ở bước 3, hãy hoàn thành như sau:
    - Tên miền: [www.amazon.com](http://www.amazon.com/)
    - Phạm vi CIDR: Mặc định
    - Giao thức: HTTP và HTTPs
    - Hành động: Từ chối
    - Nhấp vào **Tiếp theo**
![F17](/images/structure/F17.png)
- Bạn sẽ thấy 3 quy tắc Không trạng thái và 1 quy tắc ở trạng thái Trạng thái.
![F18](/images/structure/F18.png)
- Điều này sẽ không trả lại bất kỳ phản hồi nào