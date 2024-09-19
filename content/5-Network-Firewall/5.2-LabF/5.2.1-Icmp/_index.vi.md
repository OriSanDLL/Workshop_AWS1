---
title : "Block outbound ICMP"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.2.1 </b> "
---
### **Hướng dẫn chi tiết**
AWS Network Firewall sử dụng hai rules engines để kiểm tra các gói tin:
- **Stateless Engine:**
  + Kiểm tra các gói tin dựa trên các quy tắc không duy trì trạng thái.
  + Hành động: Có thể loại bỏ gói tin, cho phép nó đi đến đích, hoặc chuyển tiếp nó đến Stateful Engine.
- **Stateful Engine:**
  + Kiểm tra các gói tin trong ngữ cảnh của dòng lưu lượng, dựa trên các quy tắc duy trì trạng thái.
  + Hành động: Có thể loại bỏ gói tin hoặc cho phép nó đi đến đích.
  + Ghi log: Gửi các log về lưu lượng và cảnh báo đến logs của firewall, nếu chức năng ghi log được cấu hình. Có thể gửi cảnh báo cho các gói tin bị loại bỏ và tùy chọn gửi cho các gói tin được cho phép.
![F1](/images/structure/F1.png)

#### Cấu hình Quy tắc Stateless để Loại bỏ Tất cả Lưu lượng ICMP
1. Vào VPC:
- Đi đến Firewall Policies:
  + Trong AWS Management Console, mở dịch vụ VPC.
- Chọn Firewall Policies:
  + Trong menu bên trái, chọn Firewall policies dưới Network firewall.
- Chọn NFWPolicy:
  + Tìm và chọn NFWPolicy đã được tạo trong phần cấu hình ban đầu
- Tạo Nhóm Quy tắc Stateless:
  + Trong tab Stateless rule groups, nhấp vào Actions.
  + Chọn Create stateless rule group.
![F2](/images/structure/F2.png)
2. Enter the following values:
- **Name**: DropICMPv6
- **Capacity**: 100
![F3](/images/structure/F3.png)
Under Add rule
- **Protocol**: Choose IPv6-ICMP and delete All
- **Source**: Any IPv6 adress
- **Destination**: Any IPv6 adress scroll down
- **Actions**: Drop
Click Add rule
![F4](/images/structure/F4.png)
{{% notice note %}}
Trước khi nhấp vào Next, hãy chắc chắn rằng bạn đã thấy quy tắc mới được tạo trong phần Rules.
{{% /notice %}}
Click Next for steps 4, 5 and Create rule group for step 5.

### Testing
**Remote Access via Session Manager**
![F5](/images/structure/F5.png)
![F6](/images/structure/F6.png)

  ```markdown
  ping6 tools.keycdn.com
  ```

![F7](/images/structure/F7.png)
**Nó sẽ không trả lại bất kỳ phản hồi nào.**