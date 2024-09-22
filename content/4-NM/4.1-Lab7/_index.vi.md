---
title : "Lab 7 - Create and visualize the Global Network"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---
### Mục tiêu
- Hình dung kết nối VPC: Xem cách các VPC được gán cho Transit Gateway và cách chúng tương tác với nhau.
- Mở rộng hình dung: Khám phá khả năng hình dung các loại gán khác của TGW như AWS Direct Connect, Connect attachments, và Peering attachments.

Công cụ này giúp ta có cái nhìn tổng quan về kiến trúc mạng và các kết nối giữa các tài nguyên trong môi trường AWS.
### Kích hoạt Trình quản lý mạng AWS
1. Chọn dịch vụ VPC
![D1](/images/1/D1.png)
2. Tại VPC, chọn "Network Manager", chọn "Global Network" tại **Connectivity** và chọn "Create Global Network".
![D2](/images/1/D2.png)
3. Tại đây, nhập *Name*: `Global-Network-Connectivity-Tin` và *Description* `Global Network Connectivity Tin`, chọn "Next".
![D3](/images/1/D3.png)
4. Tại **Include core nextwork** bỏ tích và chọn "Next".
![D4](/images/1/D4.png)
5. Tai **Review** chọn  "Create Global Network".
![D5](/images/1/D5.png)
6. Tạo thành công! và truy cập vào ID của Global Network.
![D6](/images/1/D6.png)
7. Bây giờ, Chon **Transit Gateway** và bắt đầu với chọn **Register transit gateway**
![D7](/images/1/D7.png)
8. Tích vào Id của **Transit Gateway** và chọn **Register transit gateway**
![D8](/images/1/D8.png)
9. Sau khi Transit Gateway đã sẵn sàng, ta thực hiện các bước sau:
- Nhấn vào "Transit Gateways" từ bảng điều khiển bên trái.
- Mở Transit Gateway đã được đăng ký trước đó.
- Hình dung chế độ xem topo của mạng của cá nhân. Ta sẽ thấy một biểu đồ minh họa cấu trúc kết nối giữa các tài nguyên.
- Kiểm tra tất cả các gán VPC để đảm bảo rằng các kết nối giữa Transit Gateway và các VPC đang hoạt động đúng cách.
- Bằng cách này, ta có thể xác định rõ ràng các kết nối và trạng thái của từng gán VPC trong mạng của cá nhân.
![D9](/images/1/D9.png)
10. Tiêp theo:
- Chọn Transit Gateway Network để xem cấu trúc mạng một cách trực quan.
- Trong Lab tiếp theo, bạn sẽ sử dụng Route Analyzer để hiểu cách thức hoạt động của định tuyến trên Transit Gateway (TGW).
![D10](/images/1/D10.png)

**Tóm tắt** Hoàn thành phòng thí nghiệm 7.
- Trong lab này, ta đã thực hiện các bước sau:
  + Tạo mạng toàn cầu: Tạo một global network trong AWS.
  + Đăng ký Transit Gateway: Đăng ký Transit Gateway vào mạng toàn cầu.
  + Hình dung cấu trúc mạng: Sử dụng Network Manager để hình dung topology của mạng và kiểm tra khả năng kết nối giữa các tài nguyên.
{{% notice note %}}
Quá trình này giúp ta có cái nhìn tổng quan về kiến trúc mạng và các kết nối, đồng thời giúp quản lý và tối ưu hóa mạng lưới của mình hiệu quả hơn.
{{% /notice %}}