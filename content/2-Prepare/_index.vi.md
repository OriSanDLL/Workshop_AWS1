---
title : "Các bước chuẩn bị"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2. </b> "
---
### Chuẩn bị
- Triển khai các tài nguyên bằng cách tạo CloudFormation từ file liên kết [**tại đây**](https://static.us-east-1.prod.workshops.aws/public/a0087a8b-4167-471e-bba8-3828b833ae22/static/netwks.yaml) trong tài khoản AWS cá nhân (chi phí ước tính $1,50-$2,00 mỗi giờ).
- Thực hiện dọn dẹp sau khi hoàn thành hội thảo để tránh phát sinh chi phí.
- Sử dụng tài khoản AWS dành riêng cho việc học tập và thử nghiệm, không phải tài khoản chứa tài nguyên sản xuất.
- Các quyền IAM cần thiết để triển khai và dọn dẹp được liệt kê [**tại đây**](https://static.us-east-1.prod.workshops.aws/public/a0087a8b-4167-471e-bba8-3828b833ae22/static/netwks-iam-policy.json).
- Khi triển khai mẫu CloudFormation, chấp nhận tất cả thiết lập mặc định.
- Quá trình triển khai mất 15-20 phút. Khi **the stacks** hiển thị trạng thái **Create_Complete**, tiếp tục đến phần Tổng quan về Hội thảo.

**Lưu ý về chi phí tài nguyên:**

- Nếu chúng ta sử dụng tài khoản AWS của cá nhân, phải thực hiện các bước dọn dẹp vào cuối hội thảo để ngừng các khoản phí cho các dịch vụ đã sử dụng trong hội thảo.

**Thiết bị:**

- Để hoàn thành hội thảo này, chúng ta sẽ cần có máy tính xách tay cá nhân. Máy tính này cần có quyền truy cập Internet để kết nối qua HTTPS và HTTP.