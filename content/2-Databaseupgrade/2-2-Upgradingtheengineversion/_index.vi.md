---
title : "Quản lý session logs"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 4. </b> "
---


Với Session Manager chúng ta có thể xem được lịch sử các kết nối tới các instance thông qua **Session history**. Tuy nhiên chúng ta chưa xem được chi tiết các câu lệnh được sử dụng.

![S3](/images/4.s3/001-s3.png)---
title : "Upgrading the engine version"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

Minor version upgrades include only changes that are backward-compatible with existing applications.

{{% notice warning %}}
If your PostgreSQL DB instance is using Read Replicas, you must upgrade all of the read replicas before upgrading the source instance. You could follow the same instructions below, but apply them first to read replicas.
{{% /notice %}}

If your DB instance is in a Multi-AZ deployment, both the writer and standby replicas are upgraded. Your DB instance might not be available until the upgrade is complete.

Let's upgrade our database instance now.

1. In the navigation pane, choose **Databases**, and then choose the **DB instance** that you want to upgrade.

2. Choose **Modify**. The **Modify DB Instance** page appears.

![upgrade_engine](/images/2/2-2/1.png)

3. For **DB engine version**, choose the new version.

![upgrade_engine](/images/2/2-2/2.png)

4. Choose Continue and check the summary of modifications.

![upgrade_engine](/images/2/2-2/3.png)

5. To apply the changes immediately, choose **Apply immediately**. Choosing this option can cause an outage in some cases.

6. On the confirmation page, review your changes. If they are correct, choose **Modify DB Instance** to save your changes.

![upgrade_engine](/images/2/2-2/4.png)

You can see your instance being upgraded by going back to the RDS instances page.

![upgrade_engine](/images/2/2-2/5.png)

#### (OPTIONAL) AWS CLI
Alternatively you can upgrade the instance using the **AWS CLI** as shown below:

{{%expand "AWS CLI" %}}
The following command upgrades the instance to version 13.11.
```
AWSREGION=`aws configure get region`
aws rds modify-db-instance 
	--db-instance-identifier rdspg-fcj-labs 
	--engine-version 15.4 
	--apply-immediately 
	--region $AWSREGION
```

{{% /expand%}}


Trong phần này chúng ta sẽ tiến hành tạo S3 bucket và thực hiện cấu hình lưu trữ các session logs để xem được chi tiết các câu lệnh được sử dụng trong session.

![port-fwd](/images/arc-log.png) 

### Nội dung:

  - [Cập nhật IAM Role](./4.1-updateiamrole/)
  - [Tạo **S3 Bucket**](./4.2-creates3bucket/)
  - [Tạo S3 Gateway endpoint](./4.3-creategwes3)
  - [Cấu hình **Session logs**](./4.4-configsessionlogs/)
