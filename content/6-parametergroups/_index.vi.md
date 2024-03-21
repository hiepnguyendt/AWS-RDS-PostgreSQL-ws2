---
title : "Parameter Groups"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---


**Parameter groups** là một tính năng dùng để quản lý cấu hình cho các dịch vụ cơ sở dữ liệu quan hệ, chẳng hạn như Amazon RDS (Relational Database Service). Một parameter group chứa một tập hợp các tham số (parameters) có thể được cấu hình để điều chỉnh các khía cạnh khác nhau của dịch vụ cơ sở dữ liệu.

**Parameter groups** rất hữu ích cho nhiều mục đích khác nhau, bao gồm:

- Tính nhất quán: Các nhóm tham số có thể giúp bạn đảm bảo rằng tất cả các db instance của bạn đều được cấu hình theo cùng một cách. Điều này có thể giúp giảm nguy cơ xảy ra lỗi và giúp khắc phục sự cố dễ dàng hơn.
- Điều chỉnh hiệu suất: Các nhóm tham số có thể được sử dụng để điều chỉnh cơ sở dữ liệu của bạn cho khối lượng công việc cụ thể. Ví dụ: bạn có thể tăng mức phân bổ bộ nhớ cho cơ sở dữ liệu đang chạy khối lượng công việc đọc lớn.
- Bảo mật: Các nhóm tham số có thể được sử dụng để triển khai các biện pháp bảo mật tốt nhất. Ví dụ: bạn có thể tắt một số tính năng nhất định không cần thiết cho ứng dụng của mình.

Dưới đây là một số mẹo để sử dụng nhóm tham số một cách hiệu quả:
- Sử dụng các nhóm tham số mặc định làm điểm bắt đầu nhưng hãy sửa đổi chúng để đáp ứng nhu cầu cụ thể cho ứng dụng của bạn.
- Tạo các nhóm tham số tùy chỉnh cho các loại db instance khác nhau, chẳng hạn như production instances và development instances..
- Kiểm tra mọi thay đổi đối với các nhóm tham số trong môi trường chạy thử trước khi triển khai chúng vào sản xuất.
- Giám sát các phiên bản cơ sở dữ liệu của bạn để đảm bảo rằng các nhóm tham số được cấu hình chính xác.
#### Nội dung
1. [Tạo Custom Parameter Group](6-1-create/)
2. [Điều chỉnh Custom Parameter Group](6-2-modify/)
3. [Áp dụng Custom Parameter Group](6-3-apply/)
