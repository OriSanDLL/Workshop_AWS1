---
title : "Lab G: Cấu hình dịch vụ Gateway Load Balancer"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 6.1 </b> "
---
###  Mục tiêu
- Nhóm bảo mật của bạn đã tạo một VPC Gateway Load Balancer (GwlbVpc) và cài đặt Suricata, một tường lửa mã nguồn mở trên một cụm ECS. Nhiệm vụ của bạn bây giờ là cấu hình dịch vụ Gateway Load Balancer, sẵn sàng để sử dụng trong LabVpc ở Lab H.

### Cơ sở hạ tậng đã được triễn khai sẵn
- Bạn sẽ tìm thấy một nhóm tự động mở rộng (netwks-Suricata...) kết nối với cụm Dịch vụ Container Co giãn (ECS) đang chạy tường lửa Suricata. Những thành phần này đã được triển khai vào một VPC riêng biệt (GwlbVpc).

### Dịch vụ được sủ dụng
- AWS Gateway Load Balancer and Amazon VPC

### Tiêu chí thành công
- Bạn đã tạo một nhóm mục tiêu (Target Group) sử dụng giao thức GENEVE (giao thức được sử dụng bởi AWS Gateway Load Balancer - GWLB).
- Bạn đã cấu hình thành công AWS Gateway Load Balancer trong VPC GWLB.
- Bạn đã liên kết thành công nhóm tự động mở rộng (ASG) với GWLB.
- Bạn đã tạo thành công một dịch vụ điểm cuối GWLB (GWLB endpoint service).

### Gợi ý
**Create Gateway Load Balancer**
- Configure the Gateway Load Balancer [GWLB](https://docs.aws.amazon.com/elasticloadbalancing/latest/gateway/create-load-balancer.html)

**Create GWLB endpoint Service**
- Create an endpoint service using your Gateway Load Balancer [Endpoint](https://docs.aws.amazon.com/elasticloadbalancing/latest/gateway/getting-started.html#create-endpoint-service) to the relevant subnets.

**Kiến trúc phòng thí nghiệm G**
![G1](/images/structure/G1.png)

[Hướng dẫn Lab G](6.1.1-WG/_index.vi.md)