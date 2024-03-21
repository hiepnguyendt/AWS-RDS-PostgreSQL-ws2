---
title : "Kiểm tra khả năng chịu tải của cơ sở dữ liệu"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 3.4 </b> "
---

#### kiểm tra hiệu năng chịu tải của cơ sở dữ liệu (database) trong điều kiện tải cao.

Bây giờ, hãy chạy khối lượng công việc giao dịch kiểm tra sức chịu tải trên RDS database instance. Khối lượng công việc này sẽ mở ra 200 kết nối tới cơ sở dữ liệu của bạn và mỗi kết nối đó sẽ liên tục cập nhật các bảng. Để khởi chạy khối lượng công việc giao dịch, hãy quay lại AWS CLI và chạy lệnh này:

```
pgbench --host <your database endpoint>--username masteruser --protocol=prepared -P 30 --time=300 --client=200 --jobs=200 <your database name>

```
*Điều này sẽ bắt đầu 200 client sessions và đồng thời thực thi pgbench benchmark dựa trên cơ sở dữ liệu được chỉ định bởi tham số <tên cơ sở dữ liệu của bạn>. Benchmark sẽ chạy trong 300 giây và kết quả sẽ được in ra.*

Hãy cùng xem lại một số trang tổng quan mà chúng ta đã xem xét ở phần trước.

1. Bắt đầu bằng cách xem tab monitoring. Bạn sẽ thấy một số thay đổi trong dữ liệu khi load test, ví dụ: bây giờ bạn sẽ thấy số lượng kết nối cơ sở dữ liệu tăng lên đáng kể.
    ![data load test](/images/3/3-4/1.png)
    ![data load test](/images/3/3-4/2.png)

2. Chúng ta hãy truy cập vào **Performance Insights** và xem các biểu đồ khác nhau khi thực hiện kiểm tra chịu tải.

    *Counter metrics*
    ![data load test](/images/3/3-4/3.png)

    *Database load*
    ![data load test](/images/3/3-4/4.png)

    *Top SQL*
    ![data load test](/images/3/3-4/5.png)

    Lưu ý rằng trong ví dụ trên, có một vùng lớn màu xanh da trời trên biểu đồ tải Cơ sở dữ liệu. Vùng màu xanh da trời tương ứng với một số lượng lớn các phiên cơ sở dữ liệu đang chờ trong sự kiện **WALWriteLock** & **DataFileRead**.

    Từ quan điểm tối ưu hóa hiệu suất, nếu chúng tôi có thể tìm ra cách giảm số phiên chờ trong sự kiện **WALWriteLock** & **DataFileRead**, khối lượng công việc của chúng tôi có thể chạy nhanh hơn.
{{% notice info %}}
Bạn có thể tìm hiểu thêm về **Performance Insights** [tại đây](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_PerfInsights.UsingDashboard.html) .
{{% /notice %}}

3. Kiểm tra email của bạn. Bạn sẽ đã nhận được thông báo qua email về cảnh báo **CPU Utilization** mà bạn đã thiết lập trước đó ở bài thực hành này.
    ![data load test](/images/3/3-4/6.png)