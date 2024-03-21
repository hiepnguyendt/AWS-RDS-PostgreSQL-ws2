---
title : "Tìm hiểu thêm về PostgreSQL"
date :  "`r Sys.Date()`" 
weight : 8
chapter : false
pre : " <b> 8. </b> "
---

#### Benechmarking PostgreSQL Server
**Pgbench** là một công cụ benchmarking. Nó được sử dụng để kiểm tra hiệu năng của máy chủ PostgreSQL trong nhiều khối lượng công việc khác nhau. Pgbench có thể được sử dụng để kiểm tra nhiều yếu tố, bao gồm:

- Transaction throughput
- Latency
- Memory usage
- CPU usage
- Disk I/O


**Pgbench** hoạt động bằng cách tạo một số phiên máy khách đồng thời thực thi một loạt câu lệnh SQL. Các câu lệnh SQL thường đại diện cho các loại giao dịch được thực thi trong ứng dụng trong thế giới thực. Sau đó, pgbench đo hiệu suất của máy chủ PostgreSQL dưới tải do phiên máy khách tạo ra.

**Pgbench** có thể được sử dụng để kiểm tra nhiều khối lượng công việc khác nhau, bao gồm:

- **TPC-B:**  mô phỏng khối lượng công việc xử lý giao dịch trực tuyến (OLTP Online transactional processing) phức tạp.
- **TPROC:**  mô phỏng khối lượng công việc OLTP đơn giản.
- **TPC-C:**  mô phỏng khối lượng công việc của hệ thống quản lý.
- **Custom workloads**: PGbench cũng có thể được sử dụng để kiểm tra khối lượng công việc tùy chỉnh bằng cách chỉ định tệp tập lệnh chứa các câu lệnh SQL sẽ được thực thi.

Để chạy **pgbench**, bạn chỉ cần chỉ định các tham số sau:

- Số lượng phiên khách hàng đồng thời
- Thời gian của bài kiểm tra benchmark
- Cơ sở dữ liệu để kết nối
- Kịch bản giao dịch được sử dụng

**Pgbench** sau đó sẽ tạo một báo cáo cho thấy hiệu suất của máy chủ PostgreSQL dưới tải do các phiên máy khách tạo ra.
Báo cáo sẽ bao gồm các số liệu sau:
- Số giao dịch mỗi giây (tps Transactions per second ): Số lượng giao dịch trung bình được hoàn thành mỗi giây.
- Độ trễ (Latency): Thời gian trung bình để hoàn thành một giao dịch.
- Mức sử dụng bộ nhớ (Memory usage): Dung lượng bộ nhớ đã được máy chủ PostgreSQL sử dụng trong quá trình đo điểm chuẩn.
- Mức sử dụng CPU (CPU usage:): Lượng thời gian CPU đã được máy chủ PostgreSQL sử dụng trong quá trình đo điểm chuẩn.
- Disk I/O: Số lượng disk I/O được máy chủ PostgreSQL thực hiện trong quá trình đo điểm chuẩn.

Dưới đây là một số mẹo sử dụng pgbench cho người mới:

- **Bắt đầu bằng benchmark đơn giản**: Nếu bạn chưa quen với pgbench, tốt nhất nên bắt đầu bằng benchmark đơn giản, chẳng hạn như benchmark TPROC. Điều này sẽ giúp bạn làm quen với công cụ này và học cách diễn giải kết quả.
- **Sử dụng một số lượng nhỏ client**: Tốt nhất là nên bắt đầu với một số lượng nhỏ phiên client đồng thời khi chạy pgbench benchmark. Sau đó, bạn có thể tăng dần số lượng máy khách để xem hiệu suất của máy chủ PostgreSQL thay đổi như thế nào khi tải.
- **Giám sát hiệu suất của máy chủ PostgreSQL**: Khi chạy pgbench benchmark, điều quan trọng là phải giám sát hiệu suất của máy chủ PostgreSQL. Bạn có thể sử dụng các công cụ như pgAdmin hoặc pgBouncer để theo dõi các số liệu như mức sử dụng CPU, mức sử dụng bộ nhớ và I/O disk.
- **Phân tích kết quả**: Khi bạn đã chạy pgbench benchmark , điều quan trọng là phải phân tích kết quả. Điều này sẽ giúp bạn xác định mọi tắc nghẽn về hiệu suất và thực hiện các thay đổi để cải thiện hiệu suất của máy chủ PostgreSQL.

Bây giờ, chúng tôi sẽ giải thích về pgbench benchmark của bài thực hành này:
```
pgbench -i --fillfactor=90 --scale=500 --host=rdspg-fcj-labs.cssuddr073hp.us-east-1.rds.amazonaws.com --username masteruser pglab
```
- **-i**: tạo 4 bảng `pgbench_accounts`, `pgbench_branches`, `pgbench_history`, và `pgbench_tellers`, **xóa mọi bảng hiện có của những tên này**. 
- **-f (--fillfactor=90)**: Chỉ định rằng dữ liệu thử nghiệm phải được chèn bằng hệ số lấp đầy (fill factor) là 90%. Điều này có nghĩa là mỗi trang của cơ sở dữ liệu sẽ được lấp đầy tới 90% dung lượng trước khi chuyển sang trang tiếp theo.
- **-s (--scale=500)**: Chỉ định rằng dữ liệu thử nghiệm phải được chia tỷ lệ gấp 500 lần kích thước của tập dữ liệu thử nghiệm mặc định
- **-h (--host=rdspg-fcj-labs.cssuddr073hp.us-east-1.rds.amazonaws.com)**: Chỉ định tên của máy chủ PostgreSQL để kết nối.
- **-U (--username=masteruser)**: Chỉ định tên người dùng sẽ sử dụng để kết nối với máy chủ PostgreSQL.
- **pglab**: Tên của cơ sở dữ liệu cần tạo.

```
pgbench --host=rdspg-fcj-labs.cssuddr073hp.us-east-1.rds.amazonaws.com --username masteruser --protocol=prepared -P 30 --time=300 --client=200 --jobs=200 pglab
```

- **-h (--host=rdspg-fcj-labs.cssuddr073hp.us-east-1.rds.amazonaws.com)**: Chỉ định tên máy chủ của máy chủ PostgreSQL để kết nối.
- **-U (--username masteruser)**: Chỉ định tên người dùng sẽ sử dụng để kết nối với máy chủ PostgreSQL.
- **-p (--protocol=prepared)**: Chỉ định rằng các câu lệnh đã chuẩn bị sẽ được sử dụng để thực thi khối lượng công việc chuẩn.
- **--progress (-P 30)**: Chỉ định rằng điểm chuẩn sẽ chạy trong 30 giây.
- **-T (--time=300)**: Chỉ định rằng benchmark sẽ chạy trong 300 giây.
- **-c (--client=200)**: Chỉ định rằng 200 phiên máy khách đồng thời sẽ được sử dụng để thực thi khối lượng công việc chuẩn.
- **-j (--jobs=200)**: Chỉ định rằng 200 job sẽ được tạo để thực thi khối lượng công việc chuẩn.
- **pglab**: Tên của cơ sở dữ liệu cần tạo.