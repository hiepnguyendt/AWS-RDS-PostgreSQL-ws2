---
title : "Di chuyển sang DB Multi-AZ cluster bằng read replica"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

**Amazon RDS Read Replicas** cung cấp hiệu suất và độ bền nâng cao cho các RDS instance. Bản sao chỉ có quyền đọc giúp dễ dàng tăng quy mô theo phiên bản cao hơn giới hạn công suất một cách linh hoạt của một phiên bản CSDL cho những khối lượng công việc cơ sở dữ liệu có tần suất đọc nhiều. Bạn có thể tạo một hay nhiều bản sao của một phiên bản CSDL nguồn cho trước và phục vụ lưu lượng đọc của ứng dụng có dung lượng lớn từ nhiều bản sao dữ liệu của bạn, qua đó tăng tổng thông lượng đọc. Khi cần, cũng có thể nâng cấp bản sao chỉ có quyền đọc để trở thành phiên bản CSDL độc lập. Tìm hiểu thêm các tính năng của [RDS Read replicas](https://aws.amazon.com/rds/features/read-replicas/).


Amazon RDS tạo DB instance thứ hai bằng cách sử dụng snapshot của primary db instance. Nó sẽ sử dụng bản sao không đồng bộ gốc của công cụ để cập nhật read replica bất cứ khi nào có thay đổi đối với primary db instance. Read replica hoạt động như một phiên bản DB chỉ cho phép các kết nối chỉ đọc; các ứng dụng có thể kết nối với read replica giống như với bất kỳ DB instance nào.

Để ***di chuyển Single-AZ deployment hoặc Multi-AZ DB instance deployment sang Multi-AZ DB cluster deployment với thời gian ngừng hoạt động giảm xuống, bạn có thể tạo Multi-AZ DB cluster read replica***. Vui lòng truy cập [Làm việc với Multi-AZ DB cluster read replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_MultiAZDBCluster_ReadRepl.html) để biết thêm chi tiết.


1. Tìm RDS PostgreSQL instance ***rdspg-fcj-labs*** trong **Databases** và nhấp vào nút phía trước DB Instance. Nhấp vào menu **Actions** ở bên phải và chọn **Create Read Replica**
![migrating](/images/5/3/1.png)

2. Tại mục **Settings**, bạn phải cung cấp DB Instance Identifier (i.e. the name for this instance).
![migrating](/images/5/3/2.png)

3. Tại mục **Availability**, chọn ***Multi-AZ DB Cluster*** - tùy chọn triển khai mới.
![migrating](/images/5/3/4.png)


4. Để đơn giản, chúng ta sẽ để phần còn lại ở chế độ mặc định. Kéo xuống phía dưới và nhấp vào **Create read replica**

Sau khi nhấp vào **Create read replica**, bạn sẽ được đưa trở lại trang **Databases**. Làm mới trang và bạn sẽ thấy read replica đang được tạo.
![migrating](/images/5/3/5.png)

*Sẽ mất khoảng 10 phút để tạo read replica của bạn. Primary database của bạn tiếp tục có **available** khi điều này xảy ra.*

5. Khi trạng thái của Read Replica là **Available** trên trang **Databases**, hãy nhấp vào Read Replica mới.

![migrating](/images/5/3/6.png)

6. Trên trang chi tiết về Multi-AZ DB replica cluster, trước tiên hãy chú ý đến các **endpoint**. Bản sao có các endpoint khác với primary instance của bạn.
![migrating](/images/5/3/7.png)

7. Tiếp theo, kéo xuống để xem chi tiết bản sao. Bạn có thể thấy rằng primary instance của chúng ta đang sao chép sang read replica instance của chúng ta.
![migrating](/images/5/3/8.png)

Sau đó, bạn có thể kết nối với read replica bằng endpoint của nó cũng như tên người dùng và mật khẩu giống như primary instance. Ví dụ: kết nối và thực hiện truy vấn sau để kiểm tra độ trễ sao chép giữa nguồn và bản sao:
```
SELECT extract(epoch from now() - pg_last_xact_replay_timestamp()) AS replica_lag;

```
![migrating](/images/5/3/9.png)

***Bạn cũng có thể nâng cấp Multi-AZ read replica cluster này thành một cụm độc lập. Chúng tôi sẽ làm điều đó trong bước tiếp theo.***

8. Click vào **Actions**, và chọn **Promote** để tách ra và chuyển đối ***rdspg-fcj-labs-read-test*** Multi-AZ DB Cluster bằng cách chọn **Promote read replica**
![migrating](/images/5/3/10.png)

Sẽ mất khoảng 1-2 phút để read replica của bạn được tách ra và được chuyển đổi dưới dạng RDS DB Cluster với tùy chọn Multi-AZ DB Cluster deployment.

Tại thời điểm này, bạn đã di chuyển thành công RDS PostgreSQL instance có chế độ Multi-AZ DB Instance deployment sang tùy chọn RDS PostgreSQL Multi-AZ DB Cluster deployment.

**Xin chúc mừng**: Trong bài thực hành này, bạn đã thêm một Multi-AZ DB Cluster read replica instance vào cấu hình của mình để di chuyển ứng dụng/cơ sở dữ liệu từ Multi-AZ DB Instance deployment sang Multi-AZ DB Cluster deployment.

#### (Không bắt buộc) AWS CLI

Ngoài ra, bạn có thể chuyển đổi read replica bằng AWS CLI như dưới đây:
{{%expand "Code" %}}
Lệnh sau đây tạo ra read replica: 
```
ARN=`aws rds describe-db-instances --db-instance-identifier rdspg-fcj-labs --query 'DBInstances[].DBInstanceArn' --output text --region $AWSREGION`

aws rds create-db-cluster \
        --db-cluster-identifier rdspg-fcj-labs-read-test \
        --engine postgres \
        --replication-source-identifier $ARN \
        --db-cluster-instance-class db.r5d.large \
        --storage-type io1 --iops 1000 \
        --region $AWSREGION 

aws rds promote-read-replica-db-cluster \
        --db-cluster-identifier rdspg-fcj-labs-read-test

```
{{% /expand%}}
