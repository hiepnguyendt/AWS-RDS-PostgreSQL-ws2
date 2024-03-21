---
title : "Tạo một số hoạt động cho cơ sở dữ liệu "
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

#### Tạo một số hoạt động cho cơ sở dữ liệu
- Sử dụng **MobaXterm** để kết nối đến app server mà bạn đã tạo tại **workshop 1**
- Sau đó chạy lệnh sau để tạo một số hoạt động trên RDS instance của bạn.

    ```
    pgbench -i --fillfactor=90 --scale=500 --host=rdspg-fcj-labs.cssuddr073hp.us-east-1.rds.amazonaws.com --username masteruser pglab
    ```
    *Thao tác này sẽ tạo một cơ sở dữ liệu mới có tên ***pglab*** trên PostgreSQL server tại ***rdspg-fcj-labs.cssuddr073hp.us-east-1.rds.amazonaws.com*** và điền vào đó kích thước gấp 500 lần dữ liệu thử nghiệm mặc định. Dữ liệu thử nghiệm sẽ được chèn với hệ số lấp đầy (fill factor) là 90%, nghĩa là mỗi trang của cơ sở dữ liệu sẽ được lấp đầy tới 90% dung lượng trước khi chuyển sang trang tiếp theo.*

- Khi lệnh **pgbench** bắt đầu, bạn có thể chuyển sang phần tiếp theo.

    ![genate_activities](/images/3/3-2/1.png)