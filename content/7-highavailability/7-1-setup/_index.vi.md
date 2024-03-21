---
title : "Thiết lập tính sẵn sàng cao cho RDS PostgreSQL (Multi-AZ)"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 7.1. </b> "
---

#### Xác nhận đã cài đặt Multi-AZ cho RDS PostgreSQL

Truy cập giao diện [Amazon RDS console](https://console.aws.amazon.com/rds/home#databases). Kéo sang bên phải phần cài đặt cơ sở dữ liệu dành cho cơ sở dữ liệu **rdspg-fcj-labs** của bạn để xem lại cấu hình Multi-AZ của nó.
![HA](/images/7/1/1.png)
*Chúng ta có thể xác nhận rằng Multi-AZ chưa được định cấu hình, vì vậy, chúng tôi cần thiết lập cơ sở dữ liệu của mình để có tính sẵn sàng cao trong phần tiếp theo.*

#### Thiết lập tính sẵn sàng cao cho RDS PostgreSQL (Multi-AZ)

Amazon RDS Multi-AZ deployments cung cấp tính sẵn sàng cao và khả năng khôi phục tự động cho cơ sở dữ liệu của bạn, giúp giảm thiểu thời gian chết của hệ thống và hạn chế mất dữ liệu trong trường hợp sự cố xảy ra. Mỗi AZ chạy trên cơ sở hạ tầng độc lập, riêng biệt về mặt vật lý và được thiết kế để có độ tin cậy cao. Khi có một sự cố xảy ra với primary instance, Amazon RDS tự động chuyển dữ liệu sang secondary instance và thay thế primary instance bằng secondary instance trong quá trình phục hồi. Việc này giúp đảm bảo rằng ứng dụng của bạn vẫn có thể tiếp tục hoạt động mà không cần thao tác thủ công để khôi phục từ một bản sao phục hồi. Trong trường hợp xảy ra lỗi cơ sở hạ tầng, Amazon RDS sẽ tự động chuyển đổi dự phòng sang bản dự phòng (hoặc sang bản sao đọc trong trường hợp Amazon Aurora), để bạn có thể tiếp tục các hoạt động của cơ sở dữ liệu ngay khi quá trình chuyển đổi dự phòng hoàn tất. Vì DB instance endpoint của bạn vẫn giữ nguyên sau khi chuyển đổi dự phòng nên ứng dụng của bạn có thể tiếp tục hoạt động cơ sở dữ liệu mà không cần can thiệp quản trị thủ công. Có thể xem thêm chi tiết [here](https://aws.amazon.com/blogs/database/amazon-rds-under-the-hood-multi-az/).

1. Để định cấu hình RDS PostgreSQL instance có tính sẵn sàng cao, chúng ta cần sửa đổi instance đó. Chọn **rdspg-fcj-labs** instance và nhấn nút **Modify** ở trên cùng.

2. Tại trang **Modify DB Instance: rdspg-fcj-labs** , kéo xuống phần **Availability & durability** và chọn **Create a standby instance**.
![HA](/images/7/1/2.png)

3. Kéo xuống phía dưới và nhấp vào **Continue**. Trên cửa sổ tiếp theo, bạn sẽ nhận thấy thuộc tính **Multi-AZ deployment** được thay đổi thành **Yes** trong cột Value mới. Trong phần Scheduling of modifications, chọn **Apply immediately** và nhấp vào **Modify DB Instance** ở dưới cùng.
![HA](/images/7/1/3.png)

***Bây giờ instance sẽ thay đổi thành trạng thái **Modifying** và chúng ta cần đợi cho đến khi thao tác này hoàn tất - quá trình này thường mất khoảng 5-10 phút.***

4. Khi trạng thái trở lại **Available**, hãy nhấp vào **rdspg-fcj-labs** instance để xem lại cài đặt của nó.
![HA](/images/7/1/4.png)

***Vì vậy, chúng tôi đã định cấu hình RDS PostgreSQL instance của mình để có tính sẵn sàng cao và trong phần tiếp theo, chúng tôi sẽ kiểm tra khả năng này.***


#### (Không bắt buộc) AWS CLI

Ngoài ra, bạn có thể chuyển đổi db instance sang Multi-AZ bằng AWS CLI như minh họa bên dưới:
{{%expand "Code" %}}
Lệnh sau chuyển đổi db instance thành Multi-AZ.

```

AWSREGION=`aws configure get region`

aws rds modify-db-instance \
	--db-instance-identifier rdspg-fcj-labs \
	--multi-az \
	--apply-immediately \
	--region $AWSREGION


```
{{% /expand%}}