---
title : "Hướng dẫn Lab B: Triển khai tường lửa DNS"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 3.2.1 </b> "
---
### Hãy chắc chắn rằng bạn đang ở đúng khu vực

1. Điều hướng đến [trang chủ AWS Console](https://console.aws.amazon.com/console/home) .
2. Hãy đảm bảo ta đang làm việc ở đúng khu vực của mình

### Tạo danh sách miền Tường lửa DNS Route 53
1. Từ trang chủ của bảng điều khiển, nhấp vào hộp tìm kiếm và nhập `VPC`.
2. Từ bảng điều khiển VPC, chọn tùy chọn "Domain lists" từ menu bên trái.
3. Hãy đảm bảo bạn đang làm việc ở đúng khu vực của mình
4. Nhấp vào nút "Add domain lists"
5. Trong trình tạo danh sách miền, hãy đặt tên cho danh sách (trong ví dụ của chúng tôi là `netvpc`)
6. Thêm `bad.sa-demos.net.`(lưu ý dấu chấm), sau đó nhấp vào "Add domain lists"
![B2](/images/structure/B2.png)
{{% notice note %}}
Từ đây, bạn có thể sử dụng danh sách miền này trong nhóm quy tắc Tường lửa DNS.
{{% /notice %}}

### Tạo nhóm quy tắc Tường lửa DNS Route 53
1. Từ bảng điều khiển Amazon VPC, chọn tùy chọn "Rule groups" từ menu bên trái (bên dưới phần Tường lửa DNS)
2. Nhấp vào nút "Add rule group"
3. Đối với bước 1, hãy nhập tên cho nhóm quy tắc (trong ví dụ của tôi là , `netvpc`và mô tả nếu bạn muốn), sau đó nhấp vào Tiếp theo
4. Đối với bước 2, hãy chọn nút "Add rules"
5. Đặt tên cho quy tắc (trong ví dụ là: `block-bad-domain`), chọn thêm danh sách miền của riêng bạn và chọn danh sách miền bạn đã tạo ở các bước trước đó
6. Đối với hành động này, hãy chọn "Block", sau đó chọn "NXDOMAIN" làm phản hồi ưu tiên
7. Nhấp vào "Add rule", sau đó nhấp vào Tiếp theo
![B3](/images/structure/B3.png)
8. Đối với bước 3, hãy giữ nguyên mức độ ưu tiên của quy tắc và nhấp vào "Next"
9. Đối với bước 4, hãy thêm thẻ nếu bạn muốn và nhấp vào "Next"
10. Ở bước 5, hãy xem lại thông tin chi tiết rồi nhấp vào "Create rule group"
11. Từ danh sách nhóm quy tắc, hãy nhấp vào nhóm quy tắc mới tạo của bạn và chọn "View details"
12. Từ trang chi tiết, nhấp vào tab "Associate VPCs"
![B4](/images/structure/B4.png)
13. Nhấp vào "Associate VPC" và trong cửa sổ bật lên xuất hiện, hãy chọn tùy "LabVpc"chọn, sau đó nhấp vào Liên kết.
![B5](/images/structure/B5.png)
14. Chờ vài phút để quá trình liên kết hoàn tất.

### Kiểm tra xem nhóm quy tắc DNS có hoạt động không
    
Bạn có thể kiểm tra xem giải quyết DNS có bị lỗi không bằng cách kết nối với Trình `publicInstance`quản lý phiên - liên kết nhanh để thực hiện việc này có thể được tìm thấy trên [Bảng điều khiển CloudWatch của Lab B](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabB) - và sau khi kết nối, hãy thử lệnh: [](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabB)`nslookup bad.sa-demos.net`
    
Một cách tiếp cận khác là kiểm tra nhóm CloudWatch Logs; để thực hiện việc này, chúng ta có thể sử dụng truy vấn CloudWatch Logs Insights tương tự mà chúng ta đã sử dụng trong Phòng thí nghiệm 2:
    
1. Điều hướng đến bảng điều khiển CloudWatch và chọn tùy chọn "Logs Insights" từ menu bên trái.
2. Từ danh sách thả xuống, chọn `"Route53Resolver"`Nhóm nhật ký để truy vấn.
3. Xóa truy vấn mẫu (3 dòng) và thay thế bằng:
```markdown
filter query_name = 'bad.sa-demos.net.' and rcode = 'NXDOMAIN' 
| stats count(*) as numRequests by srcids.instance 
| sort numRequests desc 
| limit 10
```
4. Chạy truy vấn và xem kết quả
![B6](/images/structure/B6.png)
5. Bạn sẽ thấy ID phiên bản EC2 trong kết quả tìm kiếm; điều này cho thấy chúng tôi đã chặn thành công các truy vấn đến tên miền đó từ phiên bản đó.

**Chúc mừng bạn là hoàn thành Lab B!**