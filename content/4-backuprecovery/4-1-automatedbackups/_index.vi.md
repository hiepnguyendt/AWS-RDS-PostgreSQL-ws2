---
title : "Tự động sao lưu dữ liệu"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 4.1.</b> "
---
#### Xem lại bản sao lưu

Hãy bắt đầu bằng cách xem các bản sao lưu của cơ sở dữ liệu bạn đã tạo. Bản sao lưu cơ sở dữ liệu đầy đủ được thực hiện ngay sau khi tạo cơ sở dữ liệu.

Truy cập giao diện [Amazon RDS Console](https://console.aws.amazon.com/rds/home#databases:)
Click on **rdspg-fcj-labs** or the instance name you specified to reveal its details. Then select the **Maintenance & backups** tab.
    ![automated backups](/images/4/4-1/0.png)

Kéo xuống phần sao lưu tự động và xem lại chi tiết. Lưu ý sao lưu cơ sở dữ liệu và thời gian khôi phục mới nhất có thể. Nhìn vào các snapshots có sẵn cho cơ sở dữ liệu. Snapshots tự động từ bản sao lưu cơ sở dữ liệu ban đầu đó cũng xuất hiện.
    ![automated backups](/images/4/4-1/1.png)

#### (Không bắt buộc) AWS CLI
Ngoài ra, bạn có thể xem các bản sao lưu của phiên bản bằng AWS CLI như dưới đây:
{{%expand "AWS CLI" %}}
```
AWSREGION=`aws configure get region`

# List the automated backups for the instance

aws rds describe-db-instance-automated-backups \
	--db-instance-identifier rdspg-fcj-labs \
	--region $AWSREGION

# List the snapshots for the instance

aws rds describe-db-snapshots \
	--db-instance-identifier rdspg-fcj-labs \
	--region $AWSREGION

# Check the Latest Restorable Time (LRT) of the instance

aws rds describe-db-instances \
	--db-instance-identifier rdspg-fcj-labs \
	--query 'DBInstances[].LatestRestorableTime' \
	--region $AWSREGION \
	--output text


```

{{% /expand%}}
