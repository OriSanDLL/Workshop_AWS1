---
title : "Hướng dẫn Lab E: Thiết lập định tuyến cho Network Firewall"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 5.1.1 </b> "
---
### Hãy chắc chắn rằng bạn đang ở đúng khu vực
1. Điều hướng đến [trang chủ AWS Console](https://console.aws.amazon.com/console/home) .
2. Hãy đảm bảo ta đang làm việc ở đúng khu vực của mình

### NFW routing for IPv6
- Mục tiêu: Đảm bảo rằng tất cả lưu lượng vào và ra đều được AWS Network Firewall kiểm tra trước khi đến các subnet công cộng và riêng tư.
- Công việc đã thực hiện: Đã cấu hình một subnet firewall với AWS Network Firewall đã được triển khai.
- Cần làm tiếp: Thay đổi các bảng định tuyến IPv6 để đảm bảo tất cả lưu lượng vào và ra đều đi qua Network Firewall.

**Đây là cách các bảng định tuyến sẽ xử lý cấu hình sau**
![E2](/images/structure/E2.png)
{{% notice note %}}
Lưu ý: Khi bạn thêm các điểm kết thúc Network Firewall vào một VPC, chúng xuất hiện dưới dạng các điểm kết thúc Gateway Load Balancer (về mặt kỹ thuật, AWS Network Firewall sử dụng Gateway Load Balancer). Bảng điều khiển CloudWatch sẽ giúp bạn xác định các ID điểm kết thúc chính xác.
{{% /notice %}}

### Add default route via IGW to the FW route tables
1. 1. Trong AWS Management Console, hãy bắt đầu nhập **VPC** vào hộp tìm kiếm nhanh và nhấn **Enter** :
2. Vào VPC → Route Tables và lọc theo *LabVpc* ở hộp bên trái.
3. Chọn **LabVpc/FwSubnet1RouteTable** và nhấp vào tab Routes. Nhấp vào *Edit Routes*
![E3](/images/structure/E3.png)
4. Thêm tuyến đường và cấu hình sao cho tất cả lưu lượng phải đi qua cổng internet.
- **Destination:** ::/0
- **Target:** igw-xxxx..
![E4](/images/structure/E4.png)
5. Bây giờ, hãy lặp lại các bước từ 1 đến 4 cho bảng định tuyến "LabVpc/FwSubnet2RouteTable".

### Create and Configure Edge Route Table
1. Vào VPC → Bảng định tuyến và nhấp vào Tạo bảng định tuyến ở góc trên bên phải.
- Tên: LabVpc/Edge
- VPC: vpc-xxx (LabVpc)
- Nhấp vào Tạo bảng định tuyến.
![E5](/images/structure/E5.png)
2. Click on Edit on routes
![E6](/images/structure/E6.png)
3. Thêm 2 tuyến đường mới
    - Điểm đến: *Kiểm tra Bảng điều khiển CloudWatch cho Public Subnet CIDR (IPv6) trong AZ1*
    - Mục tiêu: *Kiểm tra Bảng điều khiển CloudWatch để biết ID Điểm cuối NFW trong AZ1*
    - Điểm đến: *Kiểm tra Bảng điều khiển CloudWatch để biết Public Subnet CIDR (IPv6) trong AZ2*
    - Mục tiêu: *Kiểm tra Bảng điều khiển CloudWatch để biết ID Điểm cuối NFW trong AZ2*
    
    Theo cách thực hành tốt nhất, định tuyến từ một mạng con nên đi qua điểm cuối GWLB trong cùng một Vùng khả dụng (AZ). Đi đến [Bảng điều khiển Lab E CloudWatch](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabE) để kiểm tra điểm cuối GWLB chính xác cho mỗi mạng con.
    
    Bạn sẽ thấy hai tuyến mới với điểm cuối GWLB VPC của Tường lửa mạng làm mục tiêu.
![E7](/images/structure/E7.png)
4. Trên tab Liên kết cạnh, hãy nhấp vào **Edit edge association**
![E8](/images/structure/E8.png)
Sau đó chọn Cổng Intenet và **Save Changes**

### Modify Public subnets route tables
1. Vào VPC → Bảng định tuyến và lọc theo LabVpc.
2. Chọn LabVpc/PubSubnet1RouteTable và nhấp vào tab Routes.
Nhấp vào Chỉnh sửa Định tuyến.
![E9](/images/structure/E9.png)
3. Chúng ta cần sửa đổi quy tắc ::/0. Xóa IGW làm mục tiêu và nhấp vào Điểm cuối của Bộ cân bằng tải cổng.
- Để thực hành tốt nhất, điểm cuối được chọn cần phải nằm trong cùng AZ với mạng con mà bảng định tuyến được liên kết. Đi đến [Bảng điều khiển Lab E CloudWatch](https://console.aws.amazon.com/cloudwatch/home?#dashboards:name=LabE) để kiểm tra điểm cuối GWLB chính xác cho mỗi mạng con.
![E10](/images/structure/E10.png)
4. Thêm tuyến đường và bây giờ cấu hình để tất cả lưu lượng truy cập phải đi đến Tường lửa mạng trước khi rời khỏi VPC
- **Destination**: ::/0
- **Target**: vpce-xxxx
5. Đây là cách bảng định tuyến của bạn sẽ trông như thế nào.
{{% notice note %}}
Xin lưu ý rằng chúng tôi chỉ cấu hình các tuyến đường cho IPv6, do đó IPv4 vẫn trỏ đến IGW, trong khi IPv6 trỏ đến điểm cuối NFW.
{{% /notice %}}
![E11](/images/structure/E11.png)
6. Bây giờ, lặp lại các bước 1-4 cho LabVpc/PubSubnet2RouteTable.

**Chúc mừng bạn đã hoàn thành Lab E!**