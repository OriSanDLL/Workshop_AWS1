---
title : "Lab 1 - Set up Amazon VPC Network Access Analyzer and check Internet accessibility"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---
### Mục tiêu
- Trong phòng thí nghiệm này, chúng ta sẽ sử dụng Trình phân tích truy cập mạng Amazon VPC để xác định các tài nguyên trong môi trường của bạn có thể được truy cập từ các cổng internet và xác minh rằng chúng chỉ giới hạn ở những tài nguyên có nhu cầu chính đáng để có thể truy cập được từ internet.

### Enable Amazon VPC Network Access Analyzer
1. Chọn dịch vụ `VPC`
![A1](/images/1/A1.png)
2. Tại "VPC", Kéo xuống chọn "Network Manager"
![A2](/images/1/A2.png)
3. Tại "Network Manager" chọn "Network Access Analyzer"
![A3](/images/1/A3.png)
4. Tại "Network Access Analyzer" chọn "Get started"
![A4](/images/1/A4.png)
5. Tại đây thấy có 4 scopes đã được tạo sẵn
![A5](/images/1/A5.png)
6. Để phân tích tất cả các đường dẫn Ingress vào VPC của bạn, hãy nhấp vào ID phạm vi truy cập mạng **AWS-VPC-Ingress**. Nhấp vào **Analyze**.
![A6](/images/1/A6.png)
7. Cuộn xuống để khám phá những phát hiện sau khi hoàn thành phân tích.
- Mỗi phát hiện đều hiển thị đường dẫn mạng từ Internet qua cổng Internet tới các tài nguyên trong AWS (ví dụ: phiên bản EC2). Đây là nơi bạn sẽ xem xét từng finding and flag/take hành động đối với những findings/paths không nhằm mục đích truy cập internet.
![A7](/images/1/A7.png)
8. Chọn phát hiện đầu tiên bằng cách nhấp vào nút radio và trong khung bên phải, bạn có thể nhấp vào tài nguyên để xem thông tin bổ sung về tài nguyên nhất định
![A7](/images/1/A7.png)

**Tóm tắt**: Hoàn thành phòng thí nghiệm 1.
- Trong lab này, ta đã thực hiện các bước sau:
  + Kích hoạt Amazon VPC Network Access Analyzer: Bật tính năng này để phân tích các đường đi của lưu lượng mạng.
  + Phân tích lưu lượng vào (ingress traffic): Kiểm tra các đường đi từ internet gateway bằng cách sử dụng default Amazon network access scope.
{{% notice note %}}
Quá trình này giúp ta xác định các cấu hình mạng có thể dẫn đến truy cập không mong muốn vào tài nguyên trong VPC của cá nhân.
{{% /notice %}}