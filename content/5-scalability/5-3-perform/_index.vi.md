---
title : "Perform vertical scaling"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

1. Truy cập giao diện [Amazon RDS console](https://console.aws.amazon.com/rds/home).

2. Chọn **rdspg-fcj-labs** instance và click **Modify**.

3. Tại trang **Modify DB Instance** - Chọn **DB instance class or DB instance size:** thích hợp. Sau đó click **Continue**

4. Tại **Summary of modifications** chú ý instance type thay đổi và chọn **Apply immediately** để áp dụng các thay đổi và nhấp vào **Modify DB instance**.

5. Instacnce sẽ bắt đầu quá trình nâng cấp DB instance, quá trình này có thể mất 10-15 phút, sau đó bạn sẽ thấy trong RDS console instance đã cập nhật:

#### (Không bắt buộc) AWS CLI
Ngoài ra, bạn có thể mở rộng quy mô instance bằng AWS CLI như dưới đây:
{{%expand "Code" %}}
Lệnh sau thay đổi instance class của instance.
```
AWSREGION=`aws configure get region`

aws rds modify-db-instance \
	--db-instance-identifier rds-pg-labs \
	--db-instance-class db.t3.large \
	--apply-immediately \
	--region $AWSREGION
```
{{% /expand%}}
