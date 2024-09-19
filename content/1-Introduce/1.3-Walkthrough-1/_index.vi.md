---
title : "Hướng Dẫn Chi tiết"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 1.3 </b> "
---
### Finding your Gateway IP address (tìm địa chỉ IP Gateway cá nhân).
    
Nhiệm vụ đầu tiên là xác định địa chỉ IP Gateway mà trình duyệt của bạn sử dụng khi truy cập web. Địa chỉ này có thể không phải là địa chỉ IP trực tiếp của máy tính xách tay, tùy thuộc vào cấu hình của bạn. Sau khi có được địa chỉ này, nó sẽ được thêm vào các nhóm bảo mật của workshop để cho phép truy cập vào các trang web WebAlb và ExtAlb.
    
1. Mở trình duyệt web và điều hướng đến Trang **[web AWS Check IP](https://checkip.amazonaws.com/).**
2. Ghi lại địa chỉ IP được hiển thị **(ví dụ này là "14.164.224.243")**
![ip](/images/structure/1.png)

### Make sure you are in the correct region (Đảm bảo đang ở đúng region).
1. [**Điều hướng đến trang chủ AWS Console**](https://console.aws.amazon.com/console/home)
2. Đảm bảo bạn đang làm việc ở đúng vùng cho hội thảo của mình **(trong ví dụ này là "US-West-2 (Oregon)")**
![Region](/images/structure/region.png)

### Adding your IP to a managed prefix list (Thêm IP của cá nhân vào prefix list được quản lý).
1. Từ trang chủ bảng điều khiển, nhấp vào hộp tìm kiếm và nhập VPC. Hãy thoải mái "yêu thích" mục này vì chúng tôi sẽ sử dụng nó trong suốt hội thảo.
![Prereq](/images/structure/prereq-02.png)
2. Từ bảng điều khiển VPC, chọn tùy chọn "Danh sách tiền tố được quản lý" từ menu bên trái.
3. Đánh dấu vào ô lựa chọn bên cạnh danh sách tiền tố có tên **workshopPrefixListIPv4**.
4. Chọn tab "Entries" để xem các tiền tố hiện có trong danh sách (nên để trống)
![x](/images/structure/x.png)
5. Từ menu Actions, chọn "Modify prefix list”
![x1](/images/structure/5.png)
6. Từ màn hình tiếp theo, chọn "Add new entry”
7. Trong trường CIDR Blocks, hãy thêm địa chỉ IP cổng mà bạn tìm thấy (trong ví dụ này là "14.164.224.243")
8. Đảm bảo bạn thêm /32 sau khi kết thúc địa chỉ IP (để cho thấy đó là một địa chỉ IP duy nhất chứ không phải một phạm vi)
9. Thêm mô tả theo lựa chọn của bạn, chẳng hạn như Gateway IP address.
10. Click "Save prefix list”
![x2](/images/structure/7.png)
11. Đảm bảo rằng yêu cầu đã được gửi thành công. Lưu ý rằng màn hình ban đầu sau khi gửi vẫn có thể hiển thị "Mục nhập danh sách tiền tố (0)", tuy nhiên, nó sẽ hiển thị trên màn hình tiếp theo.
12. Sau đó, hãy quay lại danh sách prefixes được quản lý bằng cách nhấp vào breadcrumb.
13. Xác nhận rằng địa chỉ IP Gateway của bạn nằm trong danh sách prefixes.
![x3](/images/structure/8.png)
14. Kiểm tra xem bạn có thể truy cập vào trang web WebAlb không. Bạn có thể tìm URL bằng cách điều hướng đến [bảng điều khiển Intro CloudWatch](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=Intro).
15. Bạn sẽ có thể thấy một trang web tương tự như sau:
![x4](/images/structure/9.png)
{{% notice note %}}
Lưu ý: rằng các bản phân phối CloudFront vẫn sẽ trả về lỗi; điều này là bình thường vì đây là một phần của Phòng thí nghiệm I.
{{% /notice %}}
Nếu muốn, bạn có thể lặp lại quy trình để cấu hình danh sách tiền tố IPv6. Nếu bạn không có địa chỉ IPv6 trên máy tính xách tay, bạn có thể sử dụng địa chỉ ví dụ của `2001:0002:6c::301/128`thay thế.