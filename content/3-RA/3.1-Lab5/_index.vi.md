---
title : "Lab 5 - Troubleshoot the connectivity issue between App server and Database Server"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---
### Mục tiêu
- Trong phòng thí nghiệm này, ta sẽ sử dụng Trình phân tích VPC Reachability Analyzer để khắc phục sự cố kết nối giữa ứng dụng và cơ sở dữ liệu.

### Điều kiện tiên quyết
- Tìm id **Network Interface** cho **database-server**

1. Sử dụng dịch vụ EC2.
![B3](/images/1/B3.png)
2. Kéo xuống, Chọn "Network Interfaces" tại "Network & Security" của EC2.
![B1](/images/1/B1.png)
3. Tìm kiếm type **RDS** và chọn **RDSNetworkInterface.**
4. Note lại **Network Interface ID**
![B2](/images/1/B2.png)

### Tạo và Phân tích đường dẫn
1. Chọn dịch vụ VPC
![B4](/images/1/B4.png)
2. Tại VPC, chọn "Network Manager".
3. Tại đây chọn "Reachability Analyzer" và chọn "Create and analyze path"
![B5](/images/1/B5.png)
4. Điền các thông tin sau
![B6](/images/1/B6.png)
![B7](/images/1/B7.png)
5. Chọn *Destination type*: **Network Interfaces** và *Destination*: điền **Network Interface ID** là **RDSNetworkInterface** đã được Note lúc trước.
![B8](/images/1/B8.png)
6. Nhập *Destination port*: `5432` và chọn "Create and analyze path"
![B9](/images/1/B9.png)
7. Sau khi tạo xong, ta kiểm tra và thấy **Reachability status** là Not reachable.
![B10](/images/1/B10.png)
8. ở phần "Explanations" và chọn mũi tên bên canh "Details" để kiểm tra thông tin chi tiết về thành phần hoặc tổ hợp các thành phần đang chặn đường dẫn.
(Ví dụ: trong phần giải thích sau, vì security group không có quy tắc inbound phù hợp, lưu lượng truy cập từ các nguồn bên ngoài sẽ không thể tiếp cận tài nguyên đích, dẫn đến việc chặn kết nối mạng.)
![B11](/images/1/B11.png)
9. Kéo xuống và kiểm tra phần **Path details**, để xem một biểu diễn của lộ trình ngắn nhất giữa *Source* và *Destination*.
![B12](/images/1/B12.png)

### Khắc phục sự cố
1. Nhấp vào địa chỉ được cung cấp dưới **Explanations**.
![B13](/images/1/B13.png)
2. Sau chọn vào đia chi trên, thì sẽ chuyển dẫn đến trang **Security Group** của EC2. ta chọn **Outbound Rules** và chọn **Edit Outbound rules**.
![B14](/images/1/B14.png)
3. Nhấp và "Add rule" và chọn Type là `Custom TCP`, Port range là `5432`, Destinatin IP address range là `10.1.0.0/16` và nhấp vào **Save rules**.
![B15](/images/1/B15.png)
4. Sau khi hiện xong, ta quay lại trang **Reachability Analyzer** và chọn **Analyze path**, chọn **Confirm**.
![B16](/images/1/B16.png)
![B17](/images/1/B17.png)
5. Ta thấy **Reachability status** là `Reachable`.
![B18](/images/1/B18.png)

**Tóm tắt**: Hoàn thành phòng thí nghiệm 5.
- Trong lab này, ta đã học cách tạo một phân tích trong VPC Reachability Analyzer và cách công cụ này hỗ trợ trong việc xác định các vấn đề kết nối giữa hai tài nguyên AWS.
- Các điểm chính: 
  - Tạo phân tích: Sử dụng VPC Reachability Analyzer để kiểm tra kết nối giữa hai tài nguyên, chẳng hạn như EC2 instances hoặc VPCs.
  - Xác định vấn đề kết nối: Công cụ giúp phát hiện các vấn đề như:
    - Quy tắc security group chặn lưu lượng.
    - Cấu hình route table không đúng.
    - Các thành phần mạng khác có thể gây cản trở kết nối.
  - Cung cấp thông tin chi tiết: VPC Reachability Analyzer cung cấp thông tin rõ ràng về các đường đi lưu lượng và các thành phần liên quan, giúp ta dễ dàng chẩn đoán và khắc phục sự cố.
{{% notice note %}}
Công cụ này rất hữu ích trong việc duy trì tính khả dụng và hiệu suất của các dịch vụ AWS.
{{% /notice %}}