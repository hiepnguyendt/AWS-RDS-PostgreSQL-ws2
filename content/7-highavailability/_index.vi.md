---
title : "Tính sẵn sàng cao"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 7. </b> "
---


***Tính sẵn sàng cao (HA) cho Amazon RDS PostgreSQL có thể đạt được bằng nhiều phương pháp khác nhau, bao gồm:***

- Multi-AZ deployments: Triển khai Multi-AZ tạo bản sao dự phòng của cơ sở dữ liệu ở Availability Zone (AZ) khác với primary database. Nếu primary database bị lỗi, bản sao dự phòng có thể được nâng cấp lên primary database, giảm thiểu thời gian ngừng hoạt động.
- Read replicas: Bản sao đã đọc là bản sao của cơ sở dữ liệu có thể được sử dụng để giảm tải lưu lượng đọc từ cơ sở dữ liệu chính. Điều này có thể cải thiện hiệu suất và khả năng mở rộng của cơ sở dữ liệu. Bản sao chỉ có quyền đọc cũng có thể được sử dụng để tạo kế hoạch khắc phục thảm họa trong trường hợp cơ sở dữ liệu chính bị lỗi.
- Database clusters: Database clusters là một nhóm các db instance hoạt động cùng nhau như một đơn vị logic duy nhất. Database clusters có thể được sử dụng để đạt được tính sẵn sàng và khả năng mở rộng cao bằng cách phân phối khối lượng công việc trên nhiều phiên bản.

**Multi-AZ deployments** là cách phổ biến nhất để đạt được độ sẵn sàng cao cho Amazon RDS PostgreSQL. Triển khai Multi-AZ rất dễ thiết lập và quản lý, đồng thời cung cấp mức độ sẵn sàng cao.

**Read replicas** cũng có thể được sử dụng để cải thiện tính khả dụng của Amazon RDS PostgreSQL database. Read replicas có thể được sử dụng để giảm tải lưu lượng đọc từ primary database, điều này có thể cải thiện hiệu suất và khả năng mở rộng của cơ sở dữ liệu. Read replicas cũng có thể được sử dụng để tạo kế hoạch khắc phục thảm họa trong trường hợp cơ sở dữ liệu chính bị lỗi.

**Database clusters** là tùy chọn nâng cao nhất để đạt được độ sẵn sàng và khả năng mở rộng cao cho Amazon RDS PostgreSQL. Việc thiết lập và quản lý các cụm cơ sở dữ liệu phức tạp hơn so với triển khai nhiều AZ, nhưng chúng có thể cung cấp mức độ sẵn sàng và khả năng mở rộng cao hơn.

**In this lab**, bạn sẽ thiết lập cấu hình Multi-AZ có tính sẵn sàng cao cho RDS PostgreSQL database instance của mình. Sau đó, bạn sẽ mô phỏng một sự kiện chuyển đổi dự phòng để xem nó hoạt động như thế nào. Cuối cùng, bạn mở rộng RDS instance của mình để có thêm dung lượng ghi.

#### Nội dung

1. [Thiết lập tính sẵn sàng cao cho RDS PostgreSQL (Multi-AZ)](7-1-setup/)
2. [Kết nối đến Multi-AZ endpoint](7-2-connect/)
3. [Thực hiện chuyển đổi dự phòng để xác minh tính sẵn sàng cao](7-3-perform/)


