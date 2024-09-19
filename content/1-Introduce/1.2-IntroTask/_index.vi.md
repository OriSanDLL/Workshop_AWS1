---
title : "Nhiệm Vụ Giới Thiệu: Quản Lý Kiểm Soát Truy Cập VPC Ở Quy Mô Lớn"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 1.2 </b> "
---
### Mục tiêu:

- Trước khi bắt đầu các bài lab, bạn cần cấu hình các kiểm soát bảo mật cho VPC của mình để bộ cân bằng tải WebAlb có thể nhận yêu cầu HTTP/HTTPS từ kết nối internet của máy tính xách tay của bạn. Hội thảo này sử dụng các danh sách tiền tố quản lý trong VPC để hỗ trợ bạn thực hiện việc này. IPv6 đã được kích hoạt trên một số tài nguyên triển khai, và mặc dù máy tính của bạn có thể chưa được cấu hình để kết nối IPv6, chúng tôi đã bao gồm tùy chọn cho phép kết nối IPv6 nếu có sẵn.

- Lưu ý rằng trong nhiều trường hợp, lưu lượng có thể không đến từ địa chỉ IP thực tế của máy tính của bạn mà từ một cổng HTTP hoặc NAT của mạng kết nối. Chúng tôi gọi đây là "Địa chỉ IP Gateway", và địa chỉ này sẽ khác nhau cho IPv4 và IPv6.

### Điểm bắt đầu:

- Mục tiêu của bạn trong bài lab này là cập nhật các kiểm soát bảo mật VPC của bạn để bộ cân bằng tải WebAlb có thể nhận lưu lượng HTTP (cổng TCP 80) từ địa chỉ IP Gateway. Chúng tôi đã thêm các quy tắc vào một số nhóm bảo mật của WebAlb tham chiếu đến [**VPC prefix lists**](https://docs.aws.amazon.com/vpc/latest/userguide/managed-prefix-lists.html) để giải quyết vấn đề này. Sau khi thêm địa chỉ IP Gateway của bạn vào danh sách tiền tố quản lý đúng, bạn sẽ có thể duyệt từ máy tính của bạn đến bộ cân bằng tải WebAlb.

### Cơ Sở Hạ Tầng Được Triển Khai Trước

Các tài nguyên sau đã được triển khai sẵn để hỗ trợ bạn:

- **Bộ cân bằng tải WebAlb ("webAlb")**: Bao gồm một vài phiên bản EC2 phía sau nó, cung cấp một số thông tin hữu ích.
- **Nhóm bảo mật cho WebAlb ("LabVpc/webAlbSG")**: Đã chứa các quy tắc inbound cho "workshopPrefixListIPv4" và "workshopPrefixListIPv6".
- **Danh sách tiền tố được quản lý của VPC (IPv4) ("workshopPrefixListIPv4")**: Danh sách này hiện đang trống. Đối với IPv4, sử dụng định dạng CIDR như `x.x.x.x/32`.
- **Danh sách tiền tố được quản lý của VPC (IPv6) ("workshopPrefixListIPv6")**: Danh sách này hiện cũng đang trống. Đối với IPv6, sử dụng định dạng `xxxx:xxxx:xxxx:xxxx::/128`.

Bạn có thể tìm thấy các liên kết đến WebAlb và các điểm cuối CloudFront trên [bảng điều khiển CloudWatch giới thiệu.](https://ap-southeast-2.signin.aws.amazon.com/oauth?response_type=code&redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fcloudwatch%2Fhome%3Fstate%3DhashArgs%2523dashboards%253Aname%253DIntro%26isauthcode%3Dtrue&code_challenge_method=SHA-256&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fcloudwatch&code_challenge=3E143Zf0PtHRf11obIDAEqRxb7AxZ3S246oVgrOe1vQ)

### Các Dịch Vụ Được Sử Dụng:

1. **Amazon VPC Managed Prefix Lists**:
    - **Mô tả**: Danh sách tiền tố được quản lý trong Amazon VPC cho phép bạn tạo và quản lý danh sách các khối CIDR mà bạn có thể tham chiếu trong các quy tắc bảo mật của nhóm bảo mật và các bảng định tuyến.
    - **Tài liệu**: [Amazon VPC Managed Prefix Lists Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/managed-prefix-lists.html)
2. **Amazon VPC Security Groups**:
    - **Mô tả**: Nhóm bảo mật trong Amazon VPC hoạt động như một bức tường lửa ảo để kiểm soát lưu lượng vào và ra từ các tài nguyên Amazon EC2 trong VPC.
    - **Tài liệu**: [Amazon VPC Security Groups Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)
3. **AWS CheckIp**:
    - **Mô tả**: Một dịch vụ đơn giản của AWS cho phép bạn kiểm tra địa chỉ IP công khai của bạn, giúp xác định địa chỉ IP Gateway mà bạn cần thêm vào danh sách tiền tố.
    - **Trang web**: [AWS CheckIp](https://checkip.amazonaws.com/)

### **Gợi ý:**

**What are Prefix lists?**

   - Danh sách tiền tố (prefix lists) là tập hợp các khối CIDR, giúp đơn giản hóa việc cấu hình và quản lý các nhóm bảo mật (security groups) và bảng định tuyến (route tables). Thay vì tham chiếu từng địa chỉ IP riêng lẻ, bạn có thể sử dụng danh sách tiền tố để gom các địa chỉ IP thường dùng vào một tập hợp và áp dụng chúng trong các quy tắc bảo mật hoặc định tuyến. Điều này giúp hợp nhất các quy tắc có cùng cổng và giao thức nhưng khác CIDR vào một quy tắc duy nhất.
   - [Tài liệu tham khảo.](https://docs.aws.amazon.com/vpc/latest/userguide/managed-prefix-lists.html)

**Finding your Gateway IP address**

   - Để giới hạn quyền truy cập vào máy tính xách tay, bạn cần tìm địa chỉ IP "gateway" mà mạng của bạn sử dụng để kết nối internet, có thể thực hiện qua trang [web AWS CheckIp](https://checkip.amazonaws.com/). Địa chỉ này là một IP duy nhất, và để biến thành dải CIDR hợp lệ, bạn cần thêm **/32** vào cuối. Nếu không tìm được, bạn có thể sử dụng **0.0.0.0/0** (IPv4) hoặc **::/0** (IPv6) để cho phép truy cập từ bất kỳ đâu, nhưng đây là giải pháp cuối cùng.

**Managing your own prefix lists**

   - Amazon VPC cho phép bạn tạo và quản lý prefix lists của riêng mình, có thể được tham chiếu trong các cấu trúc VPC khác như nhóm bảo mật và bảng định tuyến. Bạn có thể sử dụng prefix lists có sẵn "workshopPrefixListIPv4", thêm địa chỉ IP gateway của bạn vào đó và sử dụng nó để kiểm soát quyền truy cập thông qua nhóm bảo mật WebAlb ("LabVpc/webAlbSG").

**Introductory task completed architecture**

![IntroArchitecture](/images/structure/architecture.png)