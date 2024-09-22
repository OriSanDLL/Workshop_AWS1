---
title : "Lab 3 - Security controls (e.g., firewall/NAT-GW) in path"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---
### Mục tiêu
- Sử dụng Trình phân tích truy cập mạng, ta có thể đảm bảo rằng mình có các biện pháp kiểm soát mạng thích hợp như Network Firewall và cổng NAT trên tất cả các đường dẫn mạng giữa các tài nguyên của cá nhân.
- Trong phòng thực hành này, ta sẽ tạo một phạm vi truy cập mạng tùy chỉnh để kiểm tra xem các phiên bản trong VPC có định tuyến lưu lượng truy cập thông qua VPC kiểm tra có chứa Network Firewall hay không.

1. Quay về trang chính của "Network Access Analyze" rồi Click "Create Network Access Scope" để tạo.
![A8](/images/1/A8.png)
2. Chọn "Empty Template" và click "Next"
![A9](/images/1/A9.png)
3. Điền Name và Description
![A19](/images/1/A19.png)
4. Chọn "Add match condition"
- Làm theo hướng dẫn trên hình:
![A20](/images/1/A20.png)
5. Chọn "Next" và "Create Network Access Scope"
6. Chọn "Network Access Scope" vừa tạo và chọn "Analyze"
7. Sau khi hoàn thành, ta có thể thấy rằng có những phát hiện.
- Chúng ta quan tâm đến việc xem xét lưu lượng truy cập 'TCP' để có thể lọc các phát hiện như trong sơ đồ
![A21](/images/1/A21.png)

- Ta sẽ thấy các VPC Prod và Dev có một tuyến đi qua Network Firewall (Inspection VPC).

{{% notice note %}}
**Lưu ý**: Ta có thể lọc các phát hiện bằng cách sử dụng loại tài nguyên, giao thức, v.v.Chọn một trong các trường hợp. Ta có thể nhận thấy địa chỉ đích là số 0, tức là tuyến tới internet. 
{{% /notice %}}
- Kéo xuống khung bên phải, ta sẽ thấy đường dẫn đầy đủ kết thúc tại Central Egress VPC IGW. Để ý Network Firewall ở đường dẫn như hình bên dưới.
![A22](/images/1/A22.png)

8. Bây giờ ta có thể xác thực xem:
- Liệu việc tuân thủ có được đáp ứng hay không?
- Tức là tìm bất kỳ đường dẫn nào không có Network Firewall.
- Chúng ta sẽ sao chép và sửa đổi phạm vi mạng này cũng như phân tích nó.
9. Click "Actions", Chọn "Duplicate and modify"
![A23](/images/1/A23.png)
10. Điền Name và Description
![A24](/images/1/A24.png)
11. tại "Exclusion Conditions" chọn "Add exclusion condition" và thêm "AWS Network Firewalls", chọn "Duplicate and analyze Network Access Scope"
![A25](/images/1/A25.png)
12. Dúng như mong đợi, ta sẽ không thấy có phát hiện nào được tìm ra
- Vì lưu lương truy cập VPC Prod - Dev của ta đã đi qua Network Firewall và tuân thủ quy định nên không có phát hiện nào được tìm thấy.
![A26](/images/1/A26.png)

**Tóm tắt**: Hoàn thành phòng thí nghiệm 3.
- Trong lab này, bạn đã thực hiện các bước sau:
  - Tạo một custom network access scope: Ta phạm vi truy cập mạng tùy chỉnh để kiểm tra xem các Prod VPC và Dev VPC có định tuyến lưu lượng đến internet qua Network Firewall hay không.
  - Nhân bản phạm vi truy cập mạng: Sao chép phạm vi truy cập và thêm một exclusion để xác định xem có bất kỳ đường đi nào không đi qua Network Firewall.
  - Phát hiện các đường đi không mong muốn: Nếu có bất kỳ ai cố gắng tránh né Network Firewall, ta sẽ có thể phát hiện những trường hợp này thông qua phân tích, từ đó có thể thực hiện các hành động thích hợp để khắc phục.
{{% notice note %}}
Quá trình này giúp đảm bảo rằng lưu lượng mạng của bạn được bảo vệ và giám sát đúng cách, ngăn chặn các hành vi không tuân thủ chính sách bảo mật.
{{% /notice %}}