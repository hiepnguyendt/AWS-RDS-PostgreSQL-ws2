---
title : "Khôi phục Snapshot"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 4.4. </b> "
---

#### Khôi phục từ snapshot thủ công

Các bản sao lưu cơ sở dữ liệu có rất ít giá trị trừ khi chúng có thể được sử dụng để khôi phục cơ sở dữ liệu. Trong phần này, chúng ta sẽ dùng snapshot thủ công vừa tạo và khôi phục cơ sở dữ liệu PostgreSQL.
1. Chọn snapshot bạn đã tạo ở phần trước từ [danh sách](https://console.aws.amazon.com/rds/home#snapshots-list:) và chọn **Actions**, sau đó nhấp vào **Restore Snapshot**
	![Restore snapshot](/images/4/4-4/1.png)
{{% notice info %}}
*Khi khôi phục từ snapshot, **db instance RDS mới** được tạo, instance gốc sẽ tiếp tục chạy bình thường*
{{% /notice %}}

2. Hoàn tất trang Restore DB Instance bằng cách sử dụng các giá trị mặc định ngoại trừ DB Instance Identifier nơi bạn có thể nhập ``rdspg-fcj-labs-restore-manual-snapshot``, Chọn ``db.t3.meidum`` làm instance type rồi chọn Restore DB Instance ở cuối biểu mẫu.
	![Restore snapshot](/images/4/4-4/2.png)

Khi quá trình khôi phục bắt đầu, bạn sẽ được đưa đến danh sách cơ sở dữ liệu. Bạn có thể theo dõi trạng thái khôi phục và làm mới danh sách cho đến khi trạng thái của cơ sở dữ liệu được khôi phục là **Available**.


#### (Không bắt buộc) AWS CLI

Ngoài ra, bạn có thể khôi phục một instance từ snapshot thủ công bằng AWS CLI như minh họa bên dưới:
{{%expand "AWS CLI" %}}
The following command takes a manual snapshot of the instance.
```
AWSREGION=`aws configure get region`

aws rds restore-db-instance-from-db-snapshot \
	--db-instance-identifier rdspg-fcj-labs-restore-manual-snapshot \
	--db-snapshot-identifier manual-snapshot-rdspg-fcj-labs \
	--db-instance-class db.t3.medium \
	--region $AWSREGION

```

{{% /expand%}}