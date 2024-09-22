---
title : "Lab 8 - Use Route Analyzer to validate the routing on TGW"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---
### Mục tiêu
- Trong phòng thí nghiệm này, ta sẽ sử dụng tính năng Trình phân tích tuyến đường của AWS Network Manager để khắc phục sự cố kết nối giữa Dev và Prod VPC".

{{% notice note %}}
Tách (Diassociate) Network Firewall Rule Group khỏi Firewall Policy:
Trước khi tiến hành vào mô-đun này, chúng ta cần tách rule group khỏi firewall policy. Xin tham khảo các bước sau
{{% /notice %}}

{{% notice note %}}
Trong cơ sở hạ tầng cơ bản, AWS Network Firewall với quy tắc TCP được thiết lập để từ chối tất cả lưu lượng từ mọi nguồn đến mọi đích trên cổng 443 đã được triển khai để hoàn thành Lab 6 của VPC Reachability Analyzer. Chúng ta cần tắt quy tắc này cho lab này vì ta sẽ cần truy cập EC2 Instance để kiểm tra kết nối giữa Prod và Dev instance.
{{% /notice %}}

### Tách nhóm quy tắc firewall khỏi Firewall Policy.
1. Chọn dịch vụ VPC
![F1](/images/1/F1.png)
2. Tại đây, chọn **Firewall policies**, tích vào trang `Inspection-Fireewall-Policy`
![F2](/images/1/F2.png)
3. Tại đây tìm **stateful rule groups** và tích vào `TCP-Alert-Rule-Group`, chọn "Actions" và chọn "Diassociate from policy".
![F3](/images/1/F3.png)
![F4](/images/1/F4.png)

### Phân tích Route
1. Chọn dịch vụ VPC
![F1](/images/1/F1.png)
2. Qua lại **Network Manager**, chọn **Global Networks** và chọn đường dẫn ID tại đây.
![F5](/images/1/F5.png)
3. Chọn Global network mà ta đã tạo trong Lab 7 của mô-đun này và nhấp vào Transit Gateway Network
![F6](/images/1/F6.png)
7. Chọn **Route Analyzer**, Ta đang khắc phục sự cố kết nối giữa Dev và Prod VPC.
![F7](/images/1/F7.png)
8. Tìm IP riêng của Dev - Prod tại EC2 instance.
![F8](/images/1/F8.png)
![F9](/images/1/F9.png)
8. Sau đó hãy nhập IP riêng, và tích vào ô **Middlebox Appliance** và chọn **Run Route Analysis**
![F10](/images/1/F10.png)
9. Phát hiện kết nối không thành công và ta xem nguyên nhân từ đâu. Vì có một tuyến đường blackhole được thêm cho Prod VPC CIDR trong Transit Gateway Route Table.
![F11](/images/1/F11.png)
10. Ta đến Transit Gateway Route Table của Dev VPC và xóa đi tuyến đường blackhole để khắc phục sự cố.
![F12](/images/1/F12.png)
![F13](/images/1/F13.png)
11. Quay lại, và chạy **Route Analysis**.
![F14](/images/1/F14.png)
{{% notice note %}}
Lần này, cảnh báo cho Blackhole route sẽ biến mất, tuy nhiên trạng thái vẫn sẽ là Not Connected. Vì lưu lượng giữa Dev và Prod VPC được kiểm tra bởi AWS Network Firewall trong Inspection VPC, do đó chúng ta cần kích hoạt middle box appliance.
{{% /notice %}}
![F15](/images/1/F15.png)
12. Sau khi kích hoạt middle box appliance:
- Hãy chạy lại "Route analysis", lần này nó sẽ hiển thị toàn bộ đường đi là connected ở cả hai hướng. Ta có thể đăng nhập vào Dev hoặc Prod Instance thông qua Session Manager và kiểm tra kết nối bằng cách sử dụng lệnh "Ping".
![F16](/images/1/F16.png)
{{% notice note %}}
Để đăng nhập vào Instance bằng Session Manager, ta cần dừng và khởi động lại instance.
{{% /notice %}}

**Tóm tắt** Hoàn thành phòng thí nghiệm 8.
- Trong lab này, ta đã sử dụng Route Analyzer để khắc phục sự cố kết nối giữa Prod VPC và Dev VPC thông qua TGW và Inspection VPC.