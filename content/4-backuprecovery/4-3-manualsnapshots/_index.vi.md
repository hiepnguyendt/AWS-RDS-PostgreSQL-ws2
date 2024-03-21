---
title : "Snapshots thủ công"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 4.3. </b> "
---


#### Thực hiện sao lưu thủ công cơ sở dữ liệu của bạn.

Ngoài các bản sao lưu cơ sở dữ liệu tự động, đôi khi bạn muốn thực hiện một bản sao lưu cụ thể của cơ sở dữ liệu, ngay trước khi phát hành phần mềm lớn hoặc để làm mới môi trường tổ chức của mình. Với AWS RDS, những bản sao lưu này được gọi là snapshots thủ công. RDS tạo storage volume snapshot của DB instance, sao lưu toàn bộ DB instance chứ không chỉ các cơ sở dữ liệu riêng lẻ. Chúng được lưu trữ trong Amazon S3 nhưng chúng không nằm trong vùng lưu trữ mà khách hàng có thể truy cập.

1. Với instance của bạn được chọn từ [danh sách cơ sở dữ liệu](https://console.aws.amazon.com/rds/home#databases:). Chọn **Actions** -> **Take Snapshot**
    ![manual snapshot](/images/4/4-3/1.png)

2. Trên màn hình **Take DB Snapshot**, nhập tên cho snapshot của bạn (ví dụ: ``manual-snapshot-rdspg-fcj-labs``) và nhấp vào **Take Snapshot**.
    ![manual snapshot](/images/4/4-3/2.png)

3. Khi tạo snapshot, bạn sẽ được đưa đến trang **Snapshots** trong AWS RDS Console. Hãy đi tới [danh sách cơ sở dữ liệu](https://console.aws.amazon.com/rds/home#databases:) và xem trạng thái của instance. Bạn sẽ thấy nó sao lưu.
    ![manual snapshot](/images/4/4-3/3.png)

4. Sau khi trạng thái cơ sở dữ liệu trở về **available** hãy quay lại [danh sách snapshots](https://console.aws.amazon.com/rds/home#snapshots-list:) . Xem lại tất cả các snapshots.
    ![manual snapshot](/images/4/4-3/4.png)

#### (Không bắt buộc) AWS CLI
Ngoài ra, bạn có thể snapshot thủ công instance bằng AWS CLI như dưới đây:

{{%expand "AWS CLI" %}}
Lệnh sau sẽ snapshot thủ công instance.
```
AWSREGION=`aws configure get region`

aws rds create-db-snapshot \
	--db-instance-identifier rdspg-fcj-labs \
	--db-snapshot-identifier manual-snapshot-rdspg-fcj-labs \
	--region $AWSREGION


```

{{% /expand%}}