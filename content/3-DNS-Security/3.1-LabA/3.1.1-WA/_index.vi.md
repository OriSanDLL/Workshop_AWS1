---
title : "Hướng dẫn Lab A: Tăng cường khả năng hiển thị hoạt động của VPC"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 3.1.1 </b> "
---
### Hãy chắc chắn rằng bạn đang ở đúng khu vực

1. Điều hướng đến [trang chủ AWS Console](https://console.aws.amazon.com/console/home) .
2. Hãy đảm bảo ta đang làm việc ở đúng khu vực của mình

### Bật ghi nhật ký truy vấn DNS

1. Từ trang chủ của bảng điều khiển, nhấp vào hộp tìm kiếm và nhập "Route 53".
2. Từ bảng điều khiển Route 53, chọn tùy chọn "Query logging" từ menu bên trái. Lưu ý - bạn có thể cần thực hiện thao tác này hai lần để tránh "splash page" của trình giải quyết Route 53.
3. Route 53 mặc định hoạt động ở khu vực US-East-1, vì vậy đảm bảo bạn đang làm việc ở đúng khu vực của mình.
4. Nhấp vào nút “Configure query logging”.
![A2](/images/structure/A2.png)
5. Nhập tên cho cấu hình ghi nhật ký truy vấn, chẳng hạn như `netvpc`, chọn CloudWatch Logs Log Group làm đích và chọn "Route53Resolver" nhóm nhật ký từ danh sách.
![A3](/images/structure/A3.png)
6. Cuộn xuống và sau đó nhấp vào "Add VPC"
7. Chọn "LabVpc" tùy chọn và nhấp vào Thêm.
![A4](/images/structure/A4.png)
8. Thêm các thẻ bổ sung nếu bạn muốn.
9. Nhấp vào nút "Configure query logging" để bật ghi nhật ký truy vấn.
10. Bây giờ bạn có thể nhấp vào cấu hình và sau đó kiểm tra xem việc ghi nhật ký có hoạt động hay không bằng cách làm theo liên kết "destination ARN"
![A5](/images/structure/A5.png)
11. Từ trang Nhật ký CloudWatch, bạn có thể xem luồng nhật ký và kiểm tra xem nhật ký truy vấn có đang được thu thập hay không.
![A6](/images/structure/A6.png)

### Tìm kiếm nhật ký bằng CloudWatch Logs Insights

Để tìm kiếm qua nhật ký truy vấn DNS, chúng ta có thể sử dụng CloudWatch Logs Insights.

1. Từ trang chủ của bảng điều khiển, nhấp vào hộp tìm kiếm và nhập "CloudWatch".
2. Từ bảng điều khiển CloudWatch, chọn tùy chọn "Logs Insights" từ menu bên trái.
3. Từ danh sách thả xuống, chọn "Route53Resolver" nhóm nhật ký để truy vấn.
![A7](/images/structure/A7.png)
4. Xóa truy vấn mẫu (3 dòng) và thay thế bằng:
    ```markdown
    stats count(*) as numRequests by query_name 
    | sort numRequests desc 
    | limit 10
    ```
5. Chạy truy vấn và xem kết quả; lưu ý rằng nếu bạn vừa cấu hình Nhật ký truy vấn, có thể mất vài phút để kết quả xuất hiện.
![A8](/images/structure/A8.png)
6. Lưu ý kết quả có "bad.sa-demos.net" - chúng ta sẽ điều tra thêm về tên miền này và xem có những instance EC2 nào đã truy vấn tên miền này. Chúng ta cũng sẽ đếm theo loại bản ghi, để xem các truy vấn này là dành cho bản ghi IPv4 (A) hay IPv6 (AAAA).
7. Cập nhật truy vấn Log Insights với đoạn mã sau:
    ```markdown
    filter query_name = 'bad.sa-demos.net.' 
    | stats count(*) as numRequests by srcids.instance, query_type, answers.0.Rdata as Response 
    | sort numRequests desc 
    | limit 10
    ```
8. Chạy truy vấn và xem kết quả
![A9](/images/structure/A9.png)
9. Bạn sẽ thấy một ID instance EC2 trong kết quả tìm kiếm; ID này cùng với tên miền đáng ngờ sẽ được sử dụng trong bài lab tiếp theo.

**Chúc mừng bạn đã hoàn thành Lab A!**