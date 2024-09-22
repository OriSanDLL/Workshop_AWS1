---
title : "Lab 4 - Trusted network access"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---
## Mục tiêu
- Sử dụng Trình phân tích truy cập mạng, ta có thể xác minh rằng tài nguyên của mình chỉ có quyền truy cập mạng từ dải địa chỉ IP đáng tin cậy, qua các cổng và giao thức cụ thể.
- Trong phòng thực hành này, ta sẽ tạo phạm vi truy cập mạng tùy chỉnh để xác thực yêu cầu tuân thủ sau:
  + Lưu lượng truy cập đi từ Phiên bản Pro tới Internet chỉ được phép tải xuống các bản cập nhật từ dải địa chỉ IP công cộng 55.3.0.0/16 qua cổng 443.

1. Quay lại "Network Access Analyzer", chọn "Create Network Access Scope"
![A8](/images/1/A8.png)
2. Chọn "Empty Template" và click "Next"
![A9](/images/1/A9.png)
3. Điền Name và Description
![A27](/images/1/A27.png)
4. Chọn "Add match condition"
- Làm theo hướng dẫn trên hình:
![A28](/images/1/A28.png)
5. Tại "Exclusion", chọn "Add exclusion condition" và mở rộng "Traffic type" dưới "Destination". Điền thông tin sau:
+ Enter `55.3.0.0/16` for the Destination address.
+ Enter `443` for the Destination port.
+ Select `TCP` for the Traffic type.
![A29](/images/1/A29.png)
6. Chọn "Next" và "Create Network Access Scope"
7. Chọn "Network Access Scope" vừa tạo và chọn "Analyze"
8. Sau khi hoàn tất:
- Ta có thể thấy rằng có những phát hiện cho thấy đường dẫn mạng tồn tại giữa EC2 và cổng internet ngoài các hạn chế về ip và cổng mà chúng tôi đã xác định trong phạm vi mạng.
- Điều này là do ta có thể thấy rằng nhóm bảo mật cho phép lưu lượng truy cập tới 0.0.0.0/0 trên tất cả các cổng.
- Chọn kết quả đầu tiên và xem chi tiết
![A30](/images/1/A30.png)
9.Chọn "sg-..." để dẫn đến trang "security group" và làm theo hướng dẫn trên hình sau:
![A31](/images/1/A31.png)
10 Sau khi chỉnh sửa, quay lại "Network Access Analyzer" và chọn "Analyze"
- Sau khi hoàn tất, ta có thể thấy rằng không có phát hiện nào xác nhận rằng chúng tôi tuân thủ. (tức là phiên bản ec2 của chúng ta chỉ có thể tải xuống các bản cập nhật từ 55.3.0.0/16 qua cổng TCP 443).
![A32](/images/1/A32.png)
![A33](/images/1/A33.png)

**Tóm tắt**: Hoàn thành phòng thí nghiệm 4.
- Trong lab này, ta đã thực hiện các bước sau:
  - Tạo một custom network access scope: Tạo phạm vi truy cập mạng tùy chỉnh để phân tích các đường đi lưu lượng ra (outbound traffic) từ một EC2 instance đến internet.
  - Chỉnh sửa security group: Điều chỉnh security group để giới hạn quyền truy cập ra chỉ đến một IP/port cụ thể.
  - Phân tích lại các đường đi lưu lượng: Sau khi thay đổi, ta đã phân tích các đường đi lưu lượng một lần nữa để xác nhận rằng các yêu cầu tuân thủ đã được đáp ứng.
{{% notice note %}}
Quá trình này giúp đảm bảo rằng lưu lượng ra từ EC2 instance được kiểm soát đúng cách, giảm thiểu rủi ro bảo mật.
{{% /notice %}}