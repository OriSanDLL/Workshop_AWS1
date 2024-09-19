---
title : "Hướng dẫn Lab I: Bảo vệ ứng dụng web"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 7.1.1 </b> "
---
### Hãy chắc chắn rằng bạn đang ở đúng khu vực
1. Điều hướng đến [trang chủ AWS Console](https://console.aws.amazon.com/console/home) .
2. Hãy đảm bảo ta đang làm việc ở đúng khu vực của mình

### Adding a CloudFront global prefix list to the WebAlb
1. Đến VPC
2. Chọn "Managed prefix lists"
![I2](/images/structure/I2.png)
3. Tìm trong "Managed prefix lists" có dòng này:
`com.amazonaws.global.cloudfront.origin-facing`
![I3](/images/structure/I3.png)
4. Ghi lại một vài ký tự cuối cùng của ID danh sách tiền tố (bạn sẽ cần điều này sau) - trong ví dụ này, nó sẽ là `4526`
5. Từ VPC, chuyển đến Security Groups.
6. Chọn box bên cạnh "LabVpc/webAlbSG" trong security group
7. Chọn tab "Inbound rules" trong bảng bên dưới, sau đó nhấp vào "Edit inbound rules" (Chỉnh sửa các quy tắc inbound).
![I4](/images/structure/I4.png)
8. Nhấp vào "Add rule"
- Type: HTTP
- Source: Custom
- sau đó nhấp vào hộp tìm kiếm
- Tìm "Prefix lists" có ký tự như trong ví dụ này là `4526`
9. Chọn nó, sau đó thêm mô tả tùy chọn - trong ví dụ này là "Inbound TCP:80 from CloudFront".
10. Nhấp vào "Save rules" (Lưu quy tắc).
![I5](/images/structure/I5.png)
11. Xác nhận rằng quy tắc của bạn đã có trong danh sách Inbound rules (quy tắc đầu vào).

### Configuring AWS WAF
1. Từ trang chính của bảng điều khiển, nhấp vào ô tìm kiếm và gõ "WAF"
2. Từ bảng điều khiển WAF
- chọn tùy chọn "Web ACLs" từ menu bên trái
- Nhớ đảm bảo Region mà bạn đang thực hiện.
- Nhấp vào "Create web ACL".
3. Bước 1:
- Tên Web ACL: "netwks-webacl"
- Mô tả: Như bạn muốn
- Tên metric CloudWatch: "netwks-webacl"
- Loại tài nguyên: Regional resources
- Vùng: Đảm bảo là vùng bạn đang làm việc
- Nhấp vào "Next" để tiếp tục.
![I9](/images/structure/I9.png)
4. Bước 2: Thêm quy tắc mới.
- Từ phần Quy tắc, nhấp vào "Add rules".
- Chọn "Add my own rules and rule groups".
- Điều này sẽ đưa bạn đến màn hình "Add my own rules and rule groups".
- Đảm bảo rằng tùy chọn "Rule builder"  được chọn.
- Thêm tên cho quy tắc (ví dụ, "netwks-rule01").
- Đảm bảo quy tắc là "regular rule".
![I10](/images/structure/I10.png)
![I11](/images/structure/I11.png)
5. Tiếp tục xây dựng quy tắc:
- Cấu hình quy tắc để kích hoạt khi yêu cầu "matches the statement".
- Chọn kiểm tra một tiêu đề, và nhập "originsig" làm tên trường tiêu đề.
- Chọn loại so khớp là "Exactly matches string", và cung cấp "vpcsecurity" làm chuỗi để so khớp.
- Đảm bảo rằng biến đổi văn bản được đặt thành "Lowercase".
![I12](/images/structure/I12.png)
6. Phần cuối cùng của quy tắc:
- Chọn "Allow" (Cho phép) làm hành động, và để các phần khác ở giá trị mặc định.
- Nhấn "Add rule" (Thêm quy tắc) để quay lại trang Bước 2.
- Tại đây, cấu hình hành động mặc định của Web ACL:
  + Chặn bất kỳ yêu cầu nào không khớp với quy tắc.
  + Tùy chọn: Cấu hình mã phản hồi tùy chỉnh và thông điệp. Ví dụ, sử dụng mã phản hồi HTTP 403 và chọn tạo nội dung phản hồi tùy chỉnh.
![I13](/images/structure/I13.png)
7. Nội dung phản hồi tùy chỉnh:
- Bạn có thể trả về JSON, HTML hoặc văn bản thuần túy.
- Ví dụ, đã tạo một nội dung phản hồi tùy chỉnh gọi là "netwks-denied", trả về phản hồi HTML với thông điệp: `<strong>Invalid CloudFront distribution used to request this resource</strong>`.
- Nhấn "Save" (Lưu) và hoàn tất Bước 2 bằng cách nhấn "Next" (Tiếp theo).
![I14](/images/structure/I14.png)
8. Các bước tiếp theo:
- Bước 3: Với chỉ một quy tắc, không cần thay đổi ưu tiên. Nhấn "Next" (Tiếp theo).
- Bước 4 (Cấu hình số liệu): Để tùy chọn mặc định không thay đổi và nhấn "Next" (Tiếp theo).
- Bước 5: Xem lại các tùy chọn đã thiết lập và nhấn "Create web ACL" (Tạo web ACL). AWS WAF có thể mất vài phút để hoàn tất các nhiệm vụ cần thiết - không rời khỏi trang trong khi quá trình này đang diễn ra.
9. Bước tiếp theo: Để liên kết web ACL với tài nguyên, từ danh sách Web ACL, nhấn vào tên "netwks" để xem trang chi tiết của Web ACL. Trong tab "Associated AWS resources" (Tài nguyên AWS đã liên kết), nhấn vào "Add AWS resources" (Thêm tài nguyên AWS)
![I15](/images/structure/I15.png)
10. Bước tiếp theo: Chọn "Application Load Balancer", sau đó chọn "WebAlb" và nhấn "Add" (Thêm). Việc này có thể mất một hoặc hai phút để hoàn thành. Sau khi hoàn tất, bạn có thể kiểm tra web ACL bằng cách truy cập vào hai phân phối CloudFront khác nhau đã được tạo. 
![I16](/images/structure/I16.png)
![I17](/images/structure/I17.png)

**Chúc mừng bạn đã hoàn thành Lab I!**