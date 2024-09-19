---
title : "Hướng dẫn Lab D: Phản chiếu lưu lượng mạng VPC"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 4.2.1 </b> "
---
### Hãy chắc chắn rằng bạn đang ở đúng khu vực
1. Điều hướng đến [trang chủ AWS Console](https://console.aws.amazon.com/console/home).
2. Hãy đảm bảo ta đang làm việc ở đúng khu vực của mình

### Configure the mirror target
1. Ghi lại ID ENI của mirrorInstance từ [Lab D CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DLabD%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=wQc7X0stB0KmBa7-wT73ACi4nL4ZpsXY_MmpWkVJXFc). để sử dụng làm mục tiêu phản chiếu.
![D2](/images/structure/D2.png)
2. Từ trang chủ của bảng điều khiển, nhấp vào hộp tìm kiếm và nhập `VPC`.
3. Từ bảng điều khiển VPC, chọn tùy chọn "Mirror targets" trong menu, sau đó chọn "Create traffic mirror target”
![D3](/images/structure/D3.png)
4. Thêm tên và mô tả cho mục tiêu, chọn loại mục tiêu là "Network Interface" và chọn ENI đã xác định trước, sau đó nhấp vào "Create".
![D4](/images/structure/D4.png)

### Configure the mirror filters
1. Ghi lại địa chỉ IP riêng (IPv4 và IPv6) của EC2 mirrorInstance và sử dụng thông tin này khi tạo các quy tắc bộ lọc mirror, với thông tin được lấy từ [Lab D CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DLabD%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=wQc7X0stB0KmBa7-wT73ACi4nL4ZpsXY_MmpWkVJXFc).
![D5](/images/structure/D5.png)
2. Từ bảng điều khiển VPC, chọn tùy chọn "Mirror filters" trong menu, sau đó nhấp vào liên kết ID bộ lọc của bộ lọc được tạo sẵn (có tên "LabDMirrorFilter") để chỉnh sửa bộ lọc đó.
![D6](/images/structure/D6.png)
3. Đã có bốn quy tắc được cấu hình; hai quy tắc inbound từ chối (không phản chiếu) lưu lượng phản hồi HTTPS đến từ khối CIDR của VPC (cả IPv4 và IPv6) tới publicInstance; và hai quy tắc outbound từ chối lưu lượng yêu cầu HTTPS từ publicInstance tới khối CIDR của VPC (cả IPv4 và IPv6). Điều này nhằm loại bỏ một số nhiễu phục vụ cho mục đích của bản demo.
4. Chúng ta sẽ thêm bốn quy tắc nữa; hai quy tắc outbound chấp nhận (phản chiếu) các yêu cầu HTTPS từ publicInstance đến Internet (IPv4 và IPv6), và hai quy tắc inbound chấp nhận các phản hồi HTTPS từ Internet (IPv4 và IPv6) đến publicInstance.
5. Truy cập tab "Outbound rules" và nhấp vào "Add outbound rule" để thêm các quy tắc outbound.
6.  Giữ nguyên số quy tắc là 210 (chúng ta muốn quy tắc này thực thi sau các quy tắc hiện có), giữ hành động của quy tắc là "accept". Đặt giao thức là TCP và đặt phạm vi cổng nguồn từ 1024-65535, phạm vi cổng đích là 443. Nhập địa chỉ IPv4 riêng (kèm /32 ở cuối) của publicInstance vào trường CIDR block nguồn, sau đó thêm 0.0.0.0/0 vào trường CIDR block đích, sau đó nhấp vào "Add rule".
![D7](/images/structure/D7.png)
7. Thêm một quy tắc Outbound thứ hai (lần này cho IPv6). Giữ nguyên số quy tắc là 310, giữ hành động của quy tắc là "accept". Đặt giao thức là TCP và đặt phạm vi cổng nguồn từ 1024-65535, phạm vi cổng đích là 443. Nhập địa chỉ IPv6 riêng (kèm /128 ở cuối) của publicInstance vào trường CIDR block nguồn, sau đó thêm ::0/0 vào trường CIDR block đích, rồi nhấp vào "Add rule".
![D8](/images/structure/D8.png)
8. Nhấp vào tab "Inbound rules" và chọn "Add inbound rule" để tạo các quy tắc inbound.
9. Để tạo quy tắc inbound, giữ số quy tắc là 210 (chúng ta muốn quy tắc này chạy sau các quy tắc hiện có), giữ hành động quy tắc là "accept". Đặt giao thức là TCP, và thiết lập phạm vi cổng nguồn là 443, và phạm vi cổng đích là 1024-65535. Nhập 0.0.0.0/0 vào trường CIDR block nguồn, và sau đó thêm địa chỉ IPv4 riêng (với /32 ở cuối) của publicInstance vào trường CIDR block đích, sau đó nhấp vào "Add rule".
![D9](/images/structure/D9.png)
10. Giữ số quy tắc là 310 (chúng ta muốn quy tắc này chạy sau các quy tắc hiện có), giữ hành động quy tắc là "accept". Đặt giao thức là TCP, và thiết lập phạm vi cổng nguồn là 443, và phạm vi cổng đích là 1024-65535. Nhập ::0/0 vào trường CIDR block nguồn, và sau đó thêm địa chỉ IPv6 riêng (với /128 ở cuối) của publicInstance vào trường CIDR block đích, sau đó nhấp vào "Add rule".
![D10](/images/structure/D10.png)

### Configure the mirror session
1. Ghi lại ID ENI của publicInstance và ID của Virtual Network (308308) từ [Lab D CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DLabD%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=wQc7X0stB0KmBa7-wT73ACi4nL4ZpsXY_MmpWkVJXFc) để sử dụng khi đăng ký làm nguồn phản chiếu.
![D11](/images/structure/D11.png)
2. Từ bảng điều khiển VPC, chọn tùy chọn "Mirror session" trong menu, sau đó nhấp vào nút "Create traffic mirror session".
3. Cài đặt tên và mô tả cho phiên, chọn nguồn phản chiếu là ENI của publicInstance và mục tiêu phản chiếu là mục tiêu bạn đã tạo trước đó.
![D12](/images/structure/D12.png)
4. Đặt Session number là 1, VNI là "308308", để trống packet length, chọn bộ lọc đã cập nhật và nhấn "Create".
![D13](/images/structure/D13.png)

### View the packet captures
1. Kết nối với mirrorInstance qua Session Manager bằng liên kết trong phần Liên kết hữu ích của [Lab D CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DLabD%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=wQc7X0stB0KmBa7-wT73ACi4nL4ZpsXY_MmpWkVJXFc).
![D14](/images/structure/D14.png)
2. Kết nối với mirrorInstance, chuyển sang quyền root bằng lệnh sudo bash, và kiểm tra các giao diện mạng bằng lệnh ifconfig -a để xác nhận rằng giao diện vxlan0 đã được cấu hình.
![D15](/images/structure/D15.png)
3. Bạn có thể chạy lệnh:
    ```
    tshark -i vxlan0 -Y http -T fields -e ipv6.src -e ipv6.dst -e http.request.method -e http.request.  uri -e http.file_data
    ```
Để xem dữ liệu gói tin HTTP qua IPv6 được phản chiếu đến mirrorInstance. Để lệnh chạy khoảng một phút trước khi thấy lưu lượng, và nhấn Ctrl-C để dừng khi đã có đủ dữ liệu.
![D17](/images/structure/D17.png)
4. Bạn thấy một số gói tin được truyền dưới dạng văn bản rõ ràng thay vì được mã hóa.
![D18](/images/structure/D18.png)
"publicInstance" đang sử dụng cổng 443 cho kết nối không mã hóa HTTP, và trong các phòng thí nghiệm E và F, chúng ta sẽ tìm hiểu cách cấu hình AWS Network Firewall để phát hiện lưu lượng này tự động.

**Chúc mừng bạn đã hoàn thành Lab D!**