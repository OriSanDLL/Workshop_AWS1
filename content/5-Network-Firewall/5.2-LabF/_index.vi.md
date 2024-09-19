---
title : "Lab F: Cấu hình các quy tắc của AWS Network Firewall"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---
### Mục tiêu
Phòng thí nghiệm bao gồm 4 phần nhỏ:
- Chặn lưu lượng ICMP outbound.
- Chặn lưu lượng DNS đến máy chủ ngoài VPC.
- Chặn truy cập HTTP đến các miền cụ thể.
- Cảnh báo dựa trên nội dung tiêu đề HTTP.

### Nội Dung
1. [Block outbound ICMP](5.2.1-Icmp/_index.vi.md)
2. [Drop DNS Traffic](5.2.2-DNS/_index.vi.md)
3. [Drop specific Domains](5.2.3-Domains/_index.vi.md)
4. [Using suricata to inspect traffic content](5.2.4-Suricata/_index.vi.md)