---
title : "Thực hiện chuyển đổi dự phòng để xác minh tính sẵn sàng cao"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 7.3 </b> "
---

**RDS PostgreSQL** cung cấp cho khách hàng tùy chọn mô phỏng lỗi AZ và Tính sẵn sàng cao bằng cách cung cấp tùy chọn khởi động lại PostgreSQL instance bằng tùy chọn chuyển đổi dự phòng. Tùy chọn này sẽ bắt đầu chuyển đổi dự phòng cấp AZ cho instance - instance trong AZ phụ sẽ trở thành instance chính và instance trong AZ chính sẽ trở thành instance phụ mới.

1. Truy cập giao diện Amazon RDS [console](https://console.aws.amazon.com/rds/home#databases)  và chọn **rdspg-fcj-labs** instance. Click vào **Actions** và chọn **Reboot**.
![HA](/images/7/3/1.png)


2. Đảm bảo kiểm tra tùy chọn **Reboot With Failover**, và click **Reboot**.
![HA](/images/7/3/2.png)

3. Tiếp theo, nhấp vào tab **Logs & Events** để kiểm tra trạng thái chuyển đổi dự phòng. Quan sát sự thay đổi của các sự kiện khi quá trình chuyển đổi dự phòng trải qua các bước khác nhau.
![HA](/images/7/3/3.png)

4. Bây giờ, hãy quay lại với terminal của EC2 instance, nơi lệnh vòng lặp while vẫn đang chạy. Bạn sẽ nhận thấy sự thay đổi trong ***địa chỉ IP*** như hình bên dưới. Bạn cũng sẽ nhận thấy một thông báo ngắn gọn về sự gián đoạn ngay trong thời điểm chuyển đổi dự phòng. Sau khi phiên bản cơ sở dữ liệu khả dụng trở lại và hoạt động đối với cơ sở dữ liệu hoàn tất thành công.
![HA](/images/7/3/4.png)


Cũng lưu ý rằng RDS endpoint đã bắt đầu phân giải thành địa chỉ IP mới (từ một AZ khác)\
***(Note: In your lab the IP will be different).***

{{% notice note %}}
Bạn cũng sẽ thấy **status và AZ** trong Amazon RDS service console được cập nhật khi quá trình chuyển đổi dự phòng diễn ra nhưng có thể có độ trễ ngắn trước khi **status và AZ** được làm mới.
{{% /notice %}}


#### (Không bắt buộc) AWS CLI

Ngoài ra, bạn có thể khởi động lại instane với chuyển đổi dự phòng bằng AWS CLI như dưới đây:
{{%expand "Code" %}}
Lệnh sau khởi động lại instane có chuyển đổi dự phòng.
```

AWSREGION=`aws configure get region`

aws rds reboot-db-instance \
	--db-instance-identifier rdspg-fcj-labs \
	--region $AWSREGION \
	--force-failover



```
{{% /expand%}}