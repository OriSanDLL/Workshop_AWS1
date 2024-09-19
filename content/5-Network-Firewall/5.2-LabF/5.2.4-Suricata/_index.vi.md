---
title : "Using Suricata to inspect traffic content"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 5.2.4 </b> "
---
### **Hướng dẫn chi tiết**
- Quy Tắc: AWS Network Firewall sử dụng hai động cơ (stateless và stateful) để kiểm tra gói tin.
- Lưu Ý: Đảm bảo có quy tắc stateless để chuyển tiếp lưu lượng đến động cơ stateful.

#### Alert based on content header User Agent
1. Trên menu bên trái, đi đến Firewall policies dưới Network firewall. Bạn sẽ tìm thấy NFWPolicy, được tạo ra như một phần của thiết lập ban đầu. Nhấp vào nó và chuyển đến Stateful rule groups, nhấp vào Actions -> Create stateful rule group.
![F19](/images/structure/F19.png)
2. Chọn Suricata compatible rule string, và nhấp vào Next.
![F20](/images/structure/F20.png)
3. Enter the following values:
- **Name**: Suricata-Detect-User-Agent
- **Capacity**: 100
- Add under **Suricata compatible rule string**
    ```markdown
    drop http $HOME_NET any -> $EXTERNAL_NET any (msg:"ET USER_AGENTS Suspicious User-Agent (IE/1.0)    "; flow:established,to_server; threshold: type limit, count 2, track by_src, seconds 300; http. user_agent; content:"IE/1.0"; sid:333;)
    ```
![F21](/images/structure/F21.png)

#### Testing
![F22](/images/structure/F22.png)
- Vì chúng tôi đã định cấu hình quy tắc để *giảm lưu lượng truy cập nên yêu cầu này sẽ hết thời gian chờ.