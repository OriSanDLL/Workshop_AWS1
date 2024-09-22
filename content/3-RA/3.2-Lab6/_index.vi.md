---
title : "Lab 6 - Troubleshoot the connectivity issue between Prod EC2 instance in Prod VPC and Public IP address"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---
### Mục tiêu
- Trong phòng thí nghiệm này, ta sẽ sử dụng Trình phân tích  Amazon VPC Reachability Analyzer để troubleshoot and fix sự cố kết nối giữa phiên bản Prod EC2 và địa chỉ IP công cộng bị AWS Network Firewall chặn.

### Tạo và Phân tích đường dẫn
1. Chọn dịch vụ VPC
![B4](/images/1/B4.png)
2. Tại VPC, chọn "Network Manager", chọn "Reachability Analyzer" và chọn "Create and analyze path".
![C1](/images/1/C1.png)
3. Nhập *Name tag*: `EC2-Public-IP-via-ANF-Tin`, chọn *Source type*: `Instances` và chọn *Source*: `Prod Instance`.
![C2](/images/1/C2.png)
4. Chọn *Destination type*L `IP Address`, *Destination address*: `55.3.0.1 `.
![C3](/images/1/C3.png)
5. Tại *Additional packet header configuration at destination* nhập *Destination port*: `443`.
![C4](/images/1/C4.png)
6. Nhấp vào "Create and analyze path".
![C5](/images/1/C5.png)
7. Sau khi tạo xong, ta kiểm tra và thấy **Reachability status** là Not reachable.
![C6](/images/1/C6.png)
8. ở phần "Explanations" và chọn mũi tên bên canh "Details" để kiểm tra thông tin chi tiết về thành phần hoặc tổ hợp các thành phần đang chặn đường dẫn.
(Ví dụ: trong phần giải thích sau, vì AWS Network Firewall đã được cấu hình để không cho phép lưu lượng truy cập đến destination)
![C7](/images/1/C7.png)
9. Kéo xuống và kiểm tra phần **Path details**, để xem một biểu diễn của lộ trình ngắn nhất giữa *Source* và *Destination*, cùng phần tích cách thành phần xuất hiện trong quá trình này.
![C8](/images/1/C8.png)

### Khắc phục sự cố
1. Chọn dịch vụ VPC
![B4](/images/1/B4.png)
2. Tại VPC, chọn **Network Firewall rule groups**.
![C9](/images/1/C9.png)
3. Chọn *Your rule group* để vào chi tiết.
![C10](/images/1/C10.png)
4. Tìm đến Rules và chọn "Edit rules".
![C11](/images/1/C11.png)
5. Tại đây, tích vào Rule cần sửa và chọn "Edit".
![C12](/images/1/C12.png)
6. Tại *Traffic direction* tích "Forward" và *Action* tích "Alert" và chọn "Save".
![C13](/images/1/C13.png)
7. Chọn *Save rule group*.
![C14](/images/1/C14.png)

### Phân tích đường dẫn lại
1. Quay lại **VPC Reachability Screen** và chọn  "Analyze path".
![C15](/images/1/C15.png)
2. Chọn "Confirm".
![C16](/images/1/C16.png)
3. Sau khi xác nhận, ta kiểm tra và thấy **Reachability status** là `Reachable`.
![C17](/images/1/C17.png)

**Tóm tắt** Hoàn thành phòng thí nghiệm 6.
- Trong Lab này, ta đã học cách VPC Reachability Analyzer giúp xác định các quy tắc của ANS Firewall đang chặn lưu lượng mạng.
- Các điểm chính:
  - Sử dụng VPC Reachability Analyzer: Bạn đã thực hiện phân tích để kiểm tra đường đi của lưu lượng từ một nguồn đến một đích.
  - Phát hiện quy tắc chặn: Công cụ này đã giúp bạn xác định quy tắc nào trong ANS Firewall gây cản trở lưu lượng mạng, từ đó dễ dàng tìm ra nguyên nhân của vấn đề kết nối.
  - Khắc phục sự cố: Nhờ có thông tin chi tiết từ VPC Reachability Analyzer, bạn có thể thực hiện các điều chỉnh cần thiết cho các quy tắc firewall để khôi phục kết nối hợp lệ.
{{% notice note %}}
Công cụ này rất hữu ích trong việc đảm bảo rằng các quy tắc bảo mật không cản trở hoạt động của hệ thống mạng.
{{% /notice %}}