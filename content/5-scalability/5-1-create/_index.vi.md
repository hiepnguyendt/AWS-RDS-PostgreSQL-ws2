---
title : "Tạo Read-replica để cung cấp khả năng mở rộng đọc"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

**Amazon RDS Read Replicas** cung cấp hiệu suất và độ bền nâng cao cho các RDS instance. Bản sao chỉ có quyền đọc giúp dễ dàng tăng quy mô theo phiên bản cao hơn giới hạn công suất một cách linh hoạt của một phiên bản CSDL cho những khối lượng công việc cơ sở dữ liệu có tần suất đọc nhiều. Bạn có thể tạo một hay nhiều bản sao của một phiên bản CSDL nguồn cho trước và phục vụ lưu lượng đọc của ứng dụng có dung lượng lớn từ nhiều bản sao dữ liệu của bạn, qua đó tăng tổng thông lượng đọc. Khi cần, cũng có thể nâng cấp bản sao chỉ có quyền đọc để trở thành phiên bản CSDL độc lập. Tìm hiểu thêm các tính năng của [RDS Read replicas](https://aws.amazon.com/rds/features/read-replicas/).
![create](/images/5/rdspg.png)

**Amazon RDS tạo DB instance thứ hai** bằng cách sử dụng snapshot của DB instance gốc. Nó sẽ sử dụng bản sao không đồng bộ gốc của công cụ để cập nhật bản sao chỉ có quyền đọc bất cứ khi nào có thay đổi đối với DB instance nguồn. Bản sao chỉ có quyền đọc hoạt động như một DB instance chỉ cho phép các kết nối chỉ đọc; các ứng dụng có thể kết nối với một bản sao chỉ có quyền đọc giống như với bất kỳ DB instance nào.
1. Tìm RDS PostgreSQL instance **rdspg-fcj-labs** trong menu **Databases** và nhấp vào nút phía trước db instance. Nhấp vào **Actions** ở bên phải và chọn **Create Read Replica.**
![create](/images/5/1/1.png)

2. Tại mục **Settings**, bạn phải cung cấp DB Instance Identifier (tức là tên cho instance này). Một cái gì đó như "rdspg-fcj-labs-read".
![create](/images/5/1/2.png)

3. Tại mục **Instance Configurations** và **AWS Region**, giữ các giá trị mặc định. **Uncheck** tùy chọn **Enable storage autoscaling**.
{{% notice note %}}
Lưu ý rằng bạn có thể tùy ý chỉnh kích thước Read Replica bằng cách sử dụng cấp instance, khác với instance chính của mình. Trong trường hợp của bạn, Destination Region có thể sẽ khác với những gì bạn thấy trong ảnh chụp màn hình và điều đó không sao cả.
{{% /notice %}}
![create](/images/5/1/3.png)

4. Tại mục **Connectivity**, giữ các giá trị mặc định.
{{% notice note %}}
Giữ lại cấu hình mặc định cho Network Security.
{{% /notice %}}

5. Tại mục **Availability**, chọn Multi-AZ DB instance.
![create](/images/5/1/4.png)

6. Để đơn giản, chúng tôi sẽ để phần còn lại ở chế độ mặc định. Kéo xuống phía dưới và nhấp vào **Create read replica**.
![create](/images/5/1/7.png)

*Sẽ mất khoảng 10 phút để tạo read replica . Cơ sở dữ liệu chính của bạn sẽ vẫn available khi điều này xảy ra.*

7. Khi trạng thái của read replica là **available** trên trang **Databases** (bạn có thể làm mới lại trang để xem **Status** mới nhất), hãy nhấp vào ead replica instance mới.
![create](/images/5/1/8.png)

8. Trên trang chi tiết về read replica, trước tiên hãy chú ý đến endpoint. Read replica có endpoint khác với  primary instance của bạn
![create](/images/5/1/9.png)

9. Tiếp theo, kéo xuống để xem chi tiết bản sao. Bạn có thể thấy rằng instance chính của chúng ta đang **replicating** sang read replica instance.
![create](/images/5/1/10.png)
Sau đó, bạn có thể kết nối với read replica bằng endpoint của nó cũng như tên người dùng và mật khẩu giống như primary instance. Ví dụ: kết nối và thực hiện truy vấn sau để kiểm tra độ trễ sao chép giữa bản chính và bản sao:
```
SELECT extract(epoch from now() - pg_last_xact_replay_timestamp()) AS replica_lag;

```
![create](/images/5/1/11.png)


RDS PostgreSQL cũng cho phép bạn đưa read replica thành một instance độc lập. Chúng ta sẽ làm điều đó trong bài thực hành tiếp theo.

**Xin chúc mừng**: Trong bài thực hành này, bạn đã thêm một read replica vào cấu hình của mình để cung cấp thêm khả năng mở rộng khả đọc cho ứng dụng của bạn.


#### (Không bắt buộc) AWS CLI
Ngoài ra, bạn có thể tạo read replica bằng AWS CLI như dưới đây:
{{%expand "Code" %}}
Lệnh sau tạo read replica:
```
AWSREGION=`aws configure get region`

aws rds create-db-instance-read-replica \
	--db-instance-identifier rdspg-fcj-labs-read \
	--source-db-instance-identifier rdspg-fcj-labs \
	--db-instance-class db.t3.medium \
	--region $AWSREGION
```
{{% /expand%}}
