---
title : "Tạo DB instance read replica từ a Multi-AZ DB cluster"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

**Amazon RDS Read Replicas** cung cấp hiệu suất và độ bền nâng cao cho các RDS instance. Bản sao chỉ có quyền đọc giúp dễ dàng tăng quy mô theo phiên bản cao hơn giới hạn công suất một cách linh hoạt của một phiên bản CSDL cho những khối lượng công việc cơ sở dữ liệu có tần suất đọc nhiều. Bạn có thể tạo một hay nhiều bản sao của một phiên bản CSDL nguồn cho trước và phục vụ lưu lượng đọc của ứng dụng có dung lượng lớn từ nhiều bản sao dữ liệu của bạn, qua đó tăng tổng thông lượng đọc. Khi cần, cũng có thể nâng cấp bản sao chỉ có quyền đọc để trở thành phiên bản CSDL độc lập. Tìm hiểu thêm các tính năng của [RDS Read replicas](https://aws.amazon.com/rds/features/read-replicas/).

Please visit [Working with Multi-AZ DB cluster read replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_MultiAZDBCluster_ReadRepl.html) for additional details.

Read replica của DB Multi-AZ cluster khác với DB instances đọc của Multi-AZ DB cluster theo như sau:
- Các reader DB instances đóng vai trò là mục tiêu chuyển đổi dự phòng tự động, trong khi các read replicas thì không.
- Các Reader DB instances phải xác nhận một thay đổi so với writer DB instance trước khi thay đổi đó có thể được thực hiện. Tuy nhiên, đối với các DB instance read replicas, các bản cập nhật được sao chép không đồng bộ sang read replica mà không yêu cầu xác nhận.
- Reader DB instances luôn chia sẻ cùng một instance class, storage type và engine version như writer DB instance của the Multi-AZ DB cluster. Tuy nhiên, DB instance read replicas không nhất thiết phải chia sẻ cùng cấu hình với cụm nguồn.
- Bạn có thể nâng cấp bản sao đọc phiên bản DB lên phiên bản DB độc lập. Bạn không thể nâng cấp phiên bản DB đọc của cụm DB Multi-AZ thành phiên bản độc lập.
- Reader endpoint chỉ định tuyến các yêu cầu đến các reader DB instances của Multi-AZ DB cluster. Nó không bao giờ định tuyến các yêu cầu đến DB instance read replica.

Để biết thêm thông tin, hãy xem [Tổng quan về Multi-AZ DB clusters](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/multi-az-db-clusters-concepts.html#multi-az-db-clusters-concepts-overview).


Để tạo read replica, hãy chỉ định Multi-AZ DB cluster làm replication source. Một trong các reader instances của the Multi-AZ DB luôn là nguồn sao chép chứ không phải writer instance. Điều kiện này đảm bảo rằng bản sao luôn đồng bộ với cụm nguồn, ngay cả trong trường hợp failover.

1. Tìm RDS PostgreSQL instance ***rdspg-fcj-labs-read-test*** trong **Databases** và nhấp vào nút phía trước DB Instance. Nhấp vào **Actions** ở bên phải và chọn **Create Read Replica.**
![outbound](/images/5/4/1.png)

2. Tại mục **Settings**, bạn phải cung cấp DB Instance Identifier (i.e. tên của instance). 
![outbound](/images/5/4/2.png)

3. Tại mục the **Availability**, chọn ***Multi-AZ DB instance*** deployment.
![outbound](/images/5/4/3.png)

4. Để đơn giản, chúng tôi sẽ để phần còn lại ở chế độ mặc định. Kéo xuống phía dưới và click **Create read replica**.

Sau khi nhấp vào **Create read replica**, bạn sẽ được đưa trở lại trang **Databases**. Làm mới trang và bạn sẽ thấy read replica đang được tạo.

*Sẽ mất khoảng 10 phút để read replica của bạn. Cơ sở dữ liệu chính của bạn tiếp tục **available** khi điều này xảy ra*

5. Khi trạng thái của Read Replica là **Available** trên trang **Databases**, hãy nhấp vào read replica instance mới.
![outbound](/images/5/4/5.png)

6. Trên trang chi tiết về read replica, trước tiên hãy chú ý đến **endpoint**. Read replica có endpoint khác với primary instance của bạn.

7. Tiếp theo, kéo xuống để xem chi tiết bản sao. Bạn có thể thấy rằng cụm chính của chúng ta đang sao chép sang read replica instance của chúng ta.

Sau đó, bạn có thể kết nối với read replica bằng cách sử dụng endpoint của nó cũng như tên người dùng và mật khẩu giống như cụm nguồn. Ví dụ: kết nối và thực hiện truy vấn sau để kiểm tra độ trễ sao chép giữa nguồn và bản sao:
```
SELECT extract(epoch from now() - pg_last_xact_replay_timestamp()) AS replica_lag;

```

#### (Không bắt buộc) AWS CLI

Ngoài ra, bạn có thể chuyển đổi read replica bằng AWS CLI như dưới đây:
{{%expand "Code" %}}
Lệnh sau tạo read replica:
```

aws rds create-db-instance-read-replica \
   --db-instance-identifier taz-maz-replica \
   --source-db-cluster-identifier rdspg-fcj-labs-read-test


```
{{% /expand%}}