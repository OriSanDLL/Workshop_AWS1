---
title : "Lab 2 - Network segmentation via AWS Transit Gateway"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---
### Mục tiêu
- Corporate network có thể trải rộng trên nhiều VPC thông qua VPC Peering hoặc Transit Gateway.
- Trong phòng thí nghiệm này, chúng ta sẽ tạo phạm vi truy cập mạng tùy chỉnh để kiểm tra phân đoạn mạng thông qua AWS Transit Gateway và xác minh rằng các VPC Prod và Dev được tách biệt với nhau. Tiếp theo, chúng ta sẽ xác minh xem Prod VPC có thể giao tiếp với VPC kiểm tra hay không.

1. Tại "Network Access Analyzer", chọn "Create Network Access Scope"
![A8](/images/1/A8.png)
2. Tại "Create Network Access Scope", chọn **Empty Template**, click **Next**
![A9](/images/1/A9.png)
3. Cung cấp Name and Description
![A10](/images/1/A10.png)
4. Chọn **Add match condition.**
- làm theo các bước trong hình:
![A11](/images/1/A11.png)
5. Chọn **Next**, chọn **Create Network Access Scope**
6. Chọn **Network Access Scope** vừa tạo và chọn **Analyze**
![A12](/images/1/A12.png)
7. Sau khi hoàn tất, ta có thể thấy rằng không có phát hiện nào xác nhận VPC vừa mới tạo - Prod và Dev được phân đoạn và lưu lượng truy cập không thể định tuyến giữa chúng.
![A13](/images/1/A13.png)

**Tiếp theo, hãy xác minh xem Dev VPC có giao tiếp thông qua Tường lửa mạng hay không bằng cách sửa đổi phạm vi truy cập mạng và phân tích lại nó.**

1. Chon **Actions** và chọn **Duplicate and modify**
![A14](/images/1/A14.png)
2. Điền Name và Description
![A15](/images/1/A15.png)
3. Tại **Destination**, đổi **Prod VPC TGW Attachment** thành **Inspection VPC TGW Attachment**.
![A16](/images/1/A16.png)
4. Chọn "Duplicate and analyze Network Access Scope"
![A17](/images/1/A17.png)
5. Ta sẽ thấy rằng có những phát hiện được phát hiện, đúng như mong đợi. Bởi vì các tài nguyên trong Dev VPC của bạn định tuyến lưu lượng truy cập thông qua VPC kiểm tra có chứa tường lửa mạng.
![A18](/images/1/A18.png)

**Tóm tắt**: Hoàn thành phòng thí nghiệm 2.
- Trong lab này, Ta đã thực hiện các bước sau:
  - Tạo một custom network access scope: Tạo một phạm vi truy cập mạng tùy chỉnh để phân tích việc phân đoạn VPC.
  - Xác nhận tính cách ly giữa các VPC: Xác nhận rằng các Prod VPC và Dev VPC là cách ly, không thể giao tiếp với nhau.
  - Nhân bản và sửa đổi phạm vi truy cập mạng: Sao chép và điều chỉnh phạm vi truy cập mạng để phân tích lại các đường đi lưu lượng từ Prod VPC đến Inspection VPC.
  - Xác minh kết nối hợp lệ: Kết quả cho thấy Prod VPC có thể giao tiếp với Inspection VPC, điều này là dự kiến trong trường hợp của cá nhân.
{{% notice note %}}
Nếu ta phát hiện ra các đường đi không mong muốn giữa hai VPC không nên giao tiếp với nhau, điều này sẽ được đánh dấu là một phát hiện (nghĩa là không tuân thủ), và ta sẽ cần thực hiện các hành động thích hợp để khắc phục vấn đề này.
{{% /notice %}}