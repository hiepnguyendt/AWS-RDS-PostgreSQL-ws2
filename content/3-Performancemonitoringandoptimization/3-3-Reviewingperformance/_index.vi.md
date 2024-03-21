---
title : "Đánh giá hiệu năng"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

Giờ đây khi đã có khối lượng công việc chạy trên cơ sở dữ liệu AWS RDS PostgreSQL, chúng ta có thể xem xét các số liệu và bảng thông tin có sẵn trong CloudWatch và tìm hiểu sâu hơn với Performance Insights.

1. Tại [RDS console](https://console.aws.amazon.com/rds/home#databases)], hãy điều hướng đến trang **Database**. Để xem số liệu **CloudWatch**, hãy nhấp vào tab **Monitoring**. Khám phá nhiều biểu đồ khác nhau.
    ![reviewing](/images/3/3-3/1.png)

2. Bạn có thể hiển thị biểu đồ ở chế độ toàn màn hình và có quyền truy cập vào các tùy chỉnh khác nhau của biểu đồ.
    ![reviewing](/images/3/3-3/2.png)
3. Để xem các số liệu **Enhanced monitoring**, hãy nhấp vào nút **Monitoring** ở bên phải màn hình và chọn **Enhanced monitoring** và khám phá các biểu đồ khác nhau.
    ![reviewing](/images/3/3-3/3.png)
4. Bây giờ hãy tìm **Performance Insights** trong RDS console. Nhấp chuột phải vào liên kết và mở **Performance Insights** trong tab trình duyệt mới.
    ![reviewing](/images/3/3-3/4.png)
5. Chọn database instance của bạn và bạn sẽ thấy dữ liệu đang được tạo trên RDS instance của bạn
    ![reviewing](/images/3/3-3/5.png)
    ![reviewing](/images/3/3-3/5.1.png)
6. Tìm hiểu các chế độ chờ khác nhau (waits) trong **Performance Insights**
    ![reviewing](/images/3/3-3/6.png)
Tiếp theo, chúng ta sẽ tải 1 lượng dữ liệu lớn lên cơ sở dữ liệu và truy cập lại các trang tổng quan này.

