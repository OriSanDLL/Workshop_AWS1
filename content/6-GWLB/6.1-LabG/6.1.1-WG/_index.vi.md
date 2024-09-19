---
title : "Hướng dẫn Lab G: Cấu hình dịch vụ Gateway Load Balancer"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 6.1.1 </b> "
---
Chúng ta muốn thiết lập AWS Gateway Load Balancer (GWLB) để kiểm tra tất cả lưu lượng vào và ra trước khi nó đến các subnet công cộng và riêng tư. Để làm điều đó, chúng ta cần tạo một GWLB trong VPC GWLB, và GWLB này sẽ trỏ đến AutoScaling Group (ASG) chạy Suricata. Sau cấu hình này, các bảng định tuyến sẽ được cập nhật để đảm bảo tất cả lưu lượng đi qua GWLB.
![G1](/images/structure/G1.png)

### Create Target Group
1. Trong AWS Management Console, gõ "EC2" vào hộp tìm kiếm nhanh và nhấn Enter:
2. Đi tới Load Balancing → Target Group và nhấp vào *Create Target Group*.
3. Dưới phần Basic configuration:
- **Choose a target type**: Instances
- **Target group name**: netvpc-TG-GWLB
- **Protocol: Port**: GENEVE
- **VPC**: GwlbVpc
4. Trong Health check protocol - chọn HTTP và đặt / dưới phần path.
- Nhấp vào Next.
![G2](/images/structure/G2.png)
5. Trong mục Available instances, không chọn bất kỳ thứ gì, sau đó nhấp vào Create target group.
- Nhấp vào Create target group.

### Create Gateway Load Balancer
1. Trong AWS Management Console, bắt đầu nhập "EC2" vào hộp tìm kiếm nhanh và nhấn Enter.
2. Đi tới Load Balancing → Load Balancer và nhấp vào Create load balancer.
![G3](/images/structure/G3.png)
3. Chọn Gateway Load Balancer và nhấp vào Create.
![G4](/images/structure/G4.png)
4. Thiết lập các chi tiết sau:
- **Load balancer name**: GWLB
- **IP address type**: IPV4
- **VPC**: GwlbVpc
- **Subnets**: GwlbVpc/PriSubnet1 & GwlbVpc/PriSubnet2
- **IP listener routing**: netvpc-TG-GWLB (the one created in the previous step)
- Click in **Create load balancer**
![G5](/images/structure/G5.png)

### Associate the ASG with the Gateway Load Balancer
1. Trong AWS Management Console, bắt đầu gõ EC2 vào ô tìm kiếm nhanh và nhấn Enter:
2. Nhấp vào Autoscaling Groups trong menu bên trái.
3. Nhấp vào netwks-SuricataAutoScalingGroup.
4. Trong tab Details, cuộn xuống mục Load balancing và nhấp vào Edit.
5. Cuộn xuống mục Load balancers:
   - Nhấp vào Application, Network or Gateway Load Balancer target groups.
   - Chọn GWLB đã tạo ở bước trước.
![G6](/images/structure/G6.png)
6. Click **Update**

### Create GWLB Endpoint Service
1. Đi đến VPC → Endpoint Services và nhấp vào Create endpoint service ở góc trên bên phải.
- **Name**: GWLB-Endpoint
- **Load balancer type**: Gateway
- **Available load balancers**: GWLB (load balancer created in previous step)
- **Acceptance required**: should be un-ticked
- **Supported IP address types**: IPv4
![G7](/images/structure/G7.png)
2. Nhấp vào Create. Sau khi tạo xong, sao chép tên dịch vụ vào một ứng dụng ghi chú; bạn sẽ cần nó cho phòng thí nghiệm tiếp theo.
![G8](/images/structure/G8.png)

**Chúc mừng bạn đã hoàn thành Lab G!**