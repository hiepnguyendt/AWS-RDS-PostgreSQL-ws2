---
title : "Kết nối đến Multi-AZ endpoint"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 7.2. </b> "
---



**RDS PostgreSQL** cung cấp cho khách hàng tùy chọn mô phỏng lỗi AZ và Tính sẵn sàng cao bằng cách cung cấp tùy chọn khởi động lại Postgres Instance bằng tùy chọn chuyển đổi dự phòng. Tùy chọn này sẽ bắt đầu chuyển đổi dự phòng cấp AZ cho Instance, Instance trên AZ phụ sẽ trở thành Instance chính và Instance trên AZ chính sẽ trở thành Instance phụ mới.


1. Trong EC2 instance terminal, hãy nhập và chạy các lệnh này để hiển thị kết nối với cơ sở dữ liệu trong khoảng thời gian 10 giây:
```
while true;
do
psql -h rdspg-fcj-labs.cssuddr073hp.us-east-1.rds.amazonaws.com -U masteruser pglab; 
echo -e "\n\n"
sleep 10
done

```

Quan sát output dưới đây. Output này hiển thị cho bạn địa chỉ IP hiện tại của RDS PostgreSQL instance chính. Trong nhiệm vụ tiếp theo, chúng tôi sẽ thực hiện chuyển đổi dự phòng và quan sát sự thay đổi trong địa chỉ IP khi instance chính thay đổi. Hiện tại, hãy để terminal này mở và để vòng lặp lệnh chạy. Tiến hành nhiệm vụ tiếp theo.
![HA](/images/7/2/1.png)

#### Cấu hình Event subscription cho Failover event

Để nhận thông báo về các sự kiện Failover (hoặc các sự kiện khác), chúng tôi tạo RDS Event subscription cho bài lab này.

1. Truy cập giao diện Amazon RDS [console](https://console.aws.amazon.com/rds/home#event-subscriptions:)  và chọn **Event Subscriptions**.

2. Chọn **Create event subscription**.	
![HA](/images/7/2/2.png)
3. Chúng ta sử dụng **email subscription** trong ví dụ này - để nhận thông báo qua email bạn cung cấp:
![HA](/images/7/2/3.png)

Và chọn **rdspg-fcj-labs** instance từ danh sách hoặc sử dụng tùy chọn **All instances** và chọn **Failover event**:	
![HA](/images/7/2/4.png)

{{% notice note %}}
Trước khi có thể bắt đầu nhận thông báo sự kiện qua email, bạn cần phải xác minh địa chỉ email của mình. Bạn sẽ nhận được email xác minh ngay sau khi tạo đăng ký.
![HA](/images/7/2/5.png)
{{% /notice %}}


#### (Không bắt buộc) AWS CLI

Ngoài ra, bạn có thể tạo Event Subscription bằng AWS CLI như dưới đây:
{{%expand "Code" %}}
Đoạn code sau tạo SNS Topic và RDS Event Subscription.

```

AWSREGION=`aws configure get region`

# Tạo SNS Topic cho Event Subscription

SNSTOPICARN=$(aws sns create-topic \
    --name rdspg-fcj-email \
    --region $AWSREGION \
    --output text)

# Đăng ký ID E-mail cho SNS Topic

aws sns subscribe \
	--topic-arn $SNSTOPICARN \
	--protocol email \
	--no-return-subscription-arn \
	--notification-endpoint fcj@gmail.com \
	--region $AWSREGION

# Tạo RDS Event Subscription

aws rds create-event-subscription \
	--subscription-name subscr-rdspg-fcj-labs \
	--sns-topic-arn $SNSTOPICARN \
	--source-type db-instance \
	--event-categories failover \
	--source-ids rdspg-fcj-labs \
	--region $AWSREGION \
	--enabled

```
{{% /expand%}}
