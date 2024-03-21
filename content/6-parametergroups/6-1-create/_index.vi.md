---
title : "Tạo Custom Parameter Group"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 6.1. </b> "
---

1. Truy cập giao diện Amazon RDS [console](https://console.aws.amazon.com/rds/home#databases).
2. Trong ngăn điều hướng, chọn **Parameter groups**.
![pg](/images/6/1/1.png)

3. Chọn **Create parameter group**.
![pg](/images/6/1/2.png)

4. Tại mục **Parameter group family list**, chọn **database engine** và **version** cho parameter group của bạn.
5. Tại mục **Type**, chọn DB Parameter Group.
6. Tại mục **Group name**, đặt tên cho parameter group.
7. Tại mục **Description**, nhập mô tả cho parameter group.
8. Chọn **Create**.
![pg](/images/6/1/3.png)

9. Để xác minh, trong menu RDS, hãy nhấp vào **Parameter Groups** ở khung bên trái hoặc trên giao diện RDS .
![pg](/images/6/1/4.png)

#### (Không bắt buộc) AWS CLI

Ngoài ra, bạn có thể tạo **custom parameter group** bằng AWS CLI như dưới đây:
{{%expand "Code" %}}
Lệnh sau tạo ra một custom parameter group:

```

AWSREGION=`aws configure get region`

aws rds create-db-parameter-group \
	--db-parameter-group-name custom-pg \
	--db-parameter-group-family postgres15 \
	--description "custom-postgres parameter group" \
	--region $AWSREGION



```
{{% /expand%}}