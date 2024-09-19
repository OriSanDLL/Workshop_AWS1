---
title : "Hướng dẫn Lab C: Phân tích cấu hình mạng VPC"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 4.1.1 </b> "
---
### Hãy chắc chắn rằng bạn đang ở đúng khu vực

1. Điều hướng đến [trang chủ AWS Console](https://console.aws.amazon.com/console/home) .
2. Hãy đảm bảo ta đang làm việc ở đúng khu vực của mình

### Chạy Network Access Analyzer

Chúng ta có thể sử dụng Network Access Analyzer để tự động kiểm tra mọi mô hình truy cập được phép theo cấu hình mạng VPC. Trong trường hợp này, chúng ta sẽ xác định tất cả lưu lượng có thể truy cập vào EC2 từ Internet, ngoại trừ lưu lượng đi qua bộ cân bằng tải WebAlb, vì đó là lưu lượng hợp lệ.

1. Từ trang chủ của bảng điều khiển, nhấp vào hộp tìm kiếm và nhập "Network Manager". Đây là một tính năng của bảng điều khiển VPC.
2. Từ bảng điều khiển Network Manager, chọn tùy chọn "Network access analyzer" từ menu bên trái và nhấp vào "Get started”
3. Sau vài phút, danh sách các phạm vi truy cập mạng đã được cấu hình sẵn sẽ xuất hiện. Mặc dù chúng ta có thể sử dụng một trong số đó, nhưng chúng ta sẽ tạo một phạm vi truy cập mạng riêng của mình.
4. Nhấp vào nút "Create Network Access Scope" (Tạo phạm vi truy cập mạng), sau đó chọn "Next" (Tiếp theo).
5. Chọn bắt đầu với một mẫu trống, sau đó chọn "Next" (Tiếp theo).
![C2](/images/structure/C2.png)
6. Đặt Tên của phạm vi (trong ví dụ là `netvpc`) và chọn "Add Match condition"
7. "Match findings - conditon 1"
- Source:
  + Resource selection: **"Resource IDs"**
  + Resource types: **"InternetGateway"**
  + Resource ID: **"LabVpc/IGW"**
![C3](/images/structure/C3.png)
8. "Match findings - conditon 1" 
- Destination:
  + Resource selection: **"Resource Types"**
  + Resource types: **"AWS::EC2::Instance"**
![C4](/images/structure/C4.png)
9. "Add exclusion condition"
- Through:
    + Resource selection: **"Resource IDs"**
    + Resource types: **"Elastic Load Balancers V2"**
![C5](/images/structure/C5.png)
10. Nhấn "Next" (Tiếp theo)
- Nhấn "Create Network Access Scope" (Tạo phạm vi truy cập mạng)
- Từ màn hình Network Access Scopes (Phạm vi truy cập mạng):
  + Chọn hàng chứa phạm vi bạn vừa tạo
  + Chọn "Analyze" (Phân tích)
![C6](/images/structure/C6.png)
11. Phân tích sẽ mất vài phút để hoàn tất. Sau khi hoàn tất, bạn sẽ có thể nhấp để xem kết quả.
- Bảng bên trái hiển thị tóm tắt các phát hiện.
- Sơ đồ bên phải màn hình cung cấp phân tích chi tiết về con đường lưu lượng mạng đến EC2 instance.
![C7](/images/structure/C7.png)
- Bảng bên trái có thể cuộn và điều chỉnh kích thước cột để xem chi tiết lưu lượng TCP được phép.
- Nhấp vào các điểm nhảy trong sơ đồ bên phải để xem thông tin chi tiết về nguồn gốc.
![C8](/images/structure/C8.png)
- Phân tích cho thấy EC2 instance nhận lưu lượng SSH từ internet từ hai địa chỉ: 203.0.113.0/24 (IPv4) và 2001:db8::/32 (IPv6). Phần cuối của phòng thí nghiệm là xóa các quy tắc SSH khỏi nhóm bảo mật.

{{% notice note %}}
Lưu ý: Đừng lo lắng - phiên bản của bạn chưa bị mở ra cho Internet trong suốt hội thảo này! Hai dải mạng này thực ra không thể định tuyến trên Internet; xem [RFC 5735](https://www.rfc-editor.org/rfc/rfc5735) cho IPv4 và [thẻ tham khảo IPv6 của RIPE](https://www.ripe.net/media/documents/ipv6_reference_card.pdf) cho IPv6.
{{% /notice %}}

### Xóa quy tắc nhóm bảo mật SSH
Bây giờ chúng ta đã xác định được quyền truy cập SSH đến, chúng ta nên xóa nó khỏi nhóm bảo mật.

1. Điều hướng đến bảng điều khiển VPC, sau đó tìm `LabVpc/publicInstanceSG`nhóm bảo mật
2. Nhấp vào tab "Inbound rules" và chọn "Edit inbound rules"
![C9](/images/structure/C9.png)
3. {{% notice note %}}
Lưu ý rằng mặc dù Network Access Analyser hiện chỉ hoạt động với IPv4, bạn có thể thấy rằng một quy tắc SSH cũng đã được cấu hình cho IPv6. 
{{% /notice %}}
- Chọn "Delete" cho cả quy tắc IPv4 và IPv6, sau đó chọn "Save rules".
![C10](/images/structure/C10.png)
4. Bây giờ:
- Quay lại Network Access Analyzer.
- Chạy lại phân tích cho phạm vi đã tạo.
- Kết quả: Không có phát hiện nào (vì không còn cách nào khác để lưu lượng Internet tiếp cận các phiên bản, ngoại trừ qua WebAlb).
![C11](/images/structure/C11.png)

**Chúc mừng bạn đã hoàn thành Lab C!**