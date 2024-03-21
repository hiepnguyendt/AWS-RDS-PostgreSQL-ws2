---
title : "Scalability"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

**Scalability** là khả năng để xử lý lượng dữ liệu và lưu lượng ngày càng tăng mà không ảnh hưởng đến hiệu suất của một hệ thống. Amazon RDS PostgreSQL cung cấp nhiều tùy chọn về khả năng mở rộng, bao gồm:

- Vertical scaling: Việc tăng kích thước của DB instance class có thể cung cấp thêm CPU, bộ nhớ và dung lượng I/O.
- Horizontal scaling: Việc thêm read replicas vào phiên bản DB của bạn có thể phân phối lưu lượng đọc trên nhiều phiên bản và cải thiện hiệu suất.
- Aurora: Amazon RDS Aurora là cơ sở dữ liệu quan hệ tương thích với MySQL và PostgreSQL được quản lý toàn phần, được xây dựng cho đám mây. Aurora cung cấp khả năng mở rộng và hiệu suất cao, đồng thời cũng có tính sẵn sàng cao.

**Trong bài thực hành này**, bạn sẽ thêm một phiên bản bản sao chỉ có quyền đọc vào cấu hình của mình để cung cấp thêm khả năng mở rộng khả năng đọc cho ứng dụng của bạn. Và mô phỏng kịch bản chuyển đổi dự phòng đọc-bản sao.

#### Nội dung:
1. [Tạo Read-replica để cung cấp khả năng mở rộng đọc](5-1-create/)
2. [Chuyển đổi Read Replica thành instance độc lập](5-2-promote/)
3. [Perform vertical scaling](5-3-perform/)
4. [Di chuyển sang DB Multi-AZ cluster bằng read replica](5-4-migrating/)
5. [Tạo DB instance read replica từ a Multi-AZ DB cluster](5-5-multiazdbcluster/)
