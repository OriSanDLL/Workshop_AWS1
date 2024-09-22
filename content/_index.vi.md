---
title : "Quan sát và Chẩn đoán Mạng của bạn trên AWS"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
---
# Quan sát và Chẩn đoán Mạng của bạn trên AWS

### Tổng Quan
- Network Access Analyzer là một tính năng giúp xác định truy cập mạng ngoài ý muốn vào các tài nguyên của bạn trên AWS. Bạn có thể sử dụng Network Access Analyzer để chỉ định các yêu cầu truy cập mạng của mình và xác định các đường dẫn mạng tiềm năng không đáp ứng các yêu cầu đã chỉ định.
- Reachability Analyzer là một công cụ phân tích cấu hình cho phép thực hiện kiểm tra kết nối giữa một tài nguyên nguồn và một tài nguyên đích trong các virtual private clouds (VPCs) của bạn. Khi tài nguyên đích có thể truy cập, Reachability Analyzer cung cấp chi tiết từng bước của đường dẫn mạng ảo giữa nguồn và đích. Khi tài nguyên đích không thể truy cập, Reachability Analyzer xác định thành phần đang chặn.
- AWS Network Manager cho phép bạn quản lý tập trung mạng AWS Cloud WAN core network và mạng AWS Transit Gateway của bạn trên các AWS accounts, Regions, và các địa điểm on-premises.

### Nội Dung
1. [Giới thiệu](/1-Introduction/_index.vi.md)
2. [Network Access Analyzer Labs](/2-NAA/_index.vi.md)
3. [VPC Reachability Analyzer Labs](/3-RA/_index.vi.md)
4. [AWS Network Manager Labs](/4-NM/_index.vi.md)
5. [Dọn dẹp](/5-Cleaning/_index.vi.md)