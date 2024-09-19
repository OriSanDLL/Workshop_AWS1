---
title : "Lab D: Phản chiếu lưu lượng mạng VPC"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---
### Mục tiêu
- Điều tra thêm publicInstance đã mở ra cho Internet.
- Kiểm tra các yêu cầu HTTP trên cổng TCP 443 để phát hiện hoạt động đáng ngờ.
- Xem xét cả lưu lượng IPv6 và IPv4.

### Khởi điểm
- Nhật ký lưu lượng VPC chỉ cung cấp thông tin về lưu lượng ở cấp độ luồng.
- AWS cung cấp các công cụ để thu thập và phân tích gói tin từ các Elastic Network Interfaces.

### Cơ sở hạ tầng đã được triễn khai
- Phiên bản EC2 mirrorInstance đã được cấu hình để nhận gói tin phân tích.
- Sử dụng ID mạng 308308 cho bao bọc VXLAN.
- Bộ lọc phản chiếu đã được tạo trước để loại trừ lưu lượng nội bộ VPC.
- Cần thêm quy tắc vào bộ lọc để phản chiếu lưu lượng HTTPS (TCP:443) và phản hồi của nó từ internet.

### Dịch vụ được sử dụng
- Amazon VPC Traffic Mirroring - [Documentation](https://docs.aws.amazon.com/vpc/latest/mirroring/what-is-traffic-mirroring.html)

### Tiêu chí thành công:
- Cấu hình mirrorInstance như một mục tiêu phản chiếu cho việc phản chiếu lưu lượng VPC.
- Thêm các quy tắc bổ sung vào bộ lọc phản chiếu đã được tạo trước đó để bao gồm:
  + Lưu lượng gửi đến Internet (Cổng TCP đích 443) từ publicInstance (bao gồm cả lưu lượng IPv4 và IPv6).
  + Lưu lượng trả về từ Internet (Cổng TCP nguồn 443) đến publicInstance (bao gồm cả lưu lượng IPv4 và IPv6).
{{% notice note %}}
Lưu ý: IPv4 và IPv6 yêu cầu các quy tắc riêng biệt trong bộ lọc phản chiếu.
Tạo một phiên bản phản chiếu để bắt đầu thu thập các gói tin và gửi chúng đến mirrorInstance.
{{% /notice %}}

- Hướng dẫn kết nối và sử dụng tshark:
  1. Kết nối với mirrorInstance:
    - Sử dụng [Session Manager để kết nối với mirrorInstance](https://ap-southeast-2.signin.aws.amazon.com/oauth?client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fec2-tb&code_challenge=ArCf5Hse0GaVbDJyAKIyALiVL7PO-8C_tQnfaaZYPvM&code_challenge_method=SHA-256&response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fec2%2Fhome%3FhashArgs%3D%2523Instances%253A%26isauthcode%3Dtrue%26oauthStart%3D1726678112171%26state%3DhashArgsFromTB_ap-southeast-2_c3e90e272fa4fd26).
  2. Chạy lệnh tshark:
    - Mở terminal và chạy lệnh sau để xem lưu lượng được phản chiếu:
    ```markdown
    sudo tshark -i vxlan0 -Y http -T fields -e ipv6.src -e ipv6.dst -e http.request.method -e http.request.uri -e http.file_data
    ```

### Gợi ý
**Configuring a Mirror Target**
- Mục tiêu phản chiếu có thể là các phiên bản EC2, Network Load Balancers hoặc Gateway Load Balancers. Trong hội thảo này, chúng ta sẽ sử dụng phiên bản EC2 làm mục tiêu.
- Giao diện Traffic Mirroring yêu cầu bạn chỉ định một Elastic Network Interface (ENI) thay vì chỉ định trực tiếp phiên bản EC2.
- Bạn có thể tìm ID của ENI của mirrorInstance trong [Lab D CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DLabD%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=wQc7X0stB0KmBa7-wT73ACi4nL4ZpsXY_MmpWkVJXFc).

**Configuring a Mirror Filter**
- Bộ lọc phản chiếu giúp xác định loại lưu lượng nào sẽ được phản chiếu đến mục tiêu. Bạn có thể bao gồm và/hoặc loại trừ các loại lưu lượng cụ thể (dựa trên cổng nguồn/đích, IP nguồn/đích, giao thức) thông qua các quy tắc.
- Bộ lọc sẽ xử lý các quy tắc theo thứ tự chỉ định, dừng lại khi tìm thấy sự khớp
- Trong bài lab này, chúng ta muốn phản chiếu lưu lượng rời khỏi LabVpc đến cổng 443 trên internet. Cần 2 quy tắc:
  + Một quy tắc cho yêu cầu ra ngoài đến cổng 443.
  + Một quy tắc cho phản hồi từ cổng 443 về cổng nguồn ban đầu.
- Mỗi quy tắc phải cụ thể cho IPv4 hoặc IPv6 nên tổng cộng cần 4 quy tắc.

**Starting a Mirror Session**
- Mỗi phiên phản chiếu đại diện cho một nguồn phản chiếu duy nhất, gửi lưu lượng khớp với một bộ lọc phản chiếu đến một mục tiêu phản chiếu duy nhất.
- Bộ lọc phản chiếu có thể được tái sử dụng trong nhiều phiên, và một mục tiêu phản chiếu có thể được dùng bởi nhiều phiên phản chiếu. Tuy nhiên, một nguồn chỉ có thể liên kết với một phiên phản chiếu duy nhất.
- Nguồn phản chiếu được xác định bởi Elastic Network Interface (ENI), thay vì ID của instance. Bạn có thể tìm ENI ID của publicInstance trong [Lab D CloudWatch Dashboard](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DLabD%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=wQc7X0stB0KmBa7-wT73ACi4nL4ZpsXY_MmpWkVJXFc).

**Kiến trúc phòng thí nghiệm D**
![D1](/images/structure/D1.png)

[Hướng dẫn Lab D](4.2.1-WD/_index.vi.md)