---
title : "Điều chỉnh bản sao lưu"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 4.2.</b> "
---

#### Điều chỉnh Backup window và Backup retention
{{% notice note %}}
 - **Backup window**: Backup window là khoảng thời gian mà hệ thống sao lưu dữ liệu hoặc ứng dụng được chạy để tạo bản sao lưu
 - **Backup retention**: Backup retention là khoảng thời gian mà các bản sao lưu được lưu trữ và giữ lại trước khi chúng bị xóa hoặc thay thế bằng các bản sao lưu mới hơn.
{{% /notice %}}
Tiếp theo, chúng tôi sẽ thay đổi thời gian sao lưu thành 22:30 (UTC) và đặt thời gian lưu giữ bản sao lưu thành 3 ngày.

1. Ở phía trên bên phải của màn hình, chọn **Modify**
    ![Modify Backups](/images/4/4-2/0.png)

2. Trên trang **Mondify DB Instance**, kéo xuống phần sao lưu trong phần "**Additional Configuraiton**". Sử dụng menu kéo xuống để thay đổi thời gian lưu giữ bản sao lưu thành 3 ngày. Thời gian lưu giữ tối đa để sao lưu tự động là 35 ngày. *Manual snapshots có thể được giữ lại vô thời hạn*

3. Cập nhật thời gian sao lưu diễn ra lúc 22:30 (UTC), vì vậy hãy lên kế hoạch cho phù hợp
    ![Modify Backups](/images/4/4-2/1.png)

4. Khi bạn đã thực hiện những thay đổi thích hợp, hãy nhấp vào nút tiếp tục. Xác nhận các sửa đổi trên màn hình tiếp theo và chọn tùy chọn **Apply immediately**. Nhấp vào **Modify DB Instance**
    ![Modify Backups](/images/4/4-2/2.png)

Khi các thay đổi đang được áp dụng, bạn sẽ được đưa trở lại trang database instance . Lưu ý: thời gian bắt đầu chính xác của quá trình sao lưu sẽ được chọn ngẫu nhiên trong khoảng thời gian 30 phút.

#### (Không bắt buộc) AWS CLI

Ngoài ra, bạn có thể thay đổi thời gian sao lưu của db instance bằng cách sử dụng **AWS CLI** như hiển thị bên dưới:

{{%expand "AWS CLI" %}}

```
AWSREGION=`aws configure get region`

aws rds modify-db-instance \
	--db-instance-identifier rds-pg-labs \
	--preferred-backup-window 22:00-22:30 \
	--apply-immediately \
	--region $AWSREGION

```

{{% /expand%}}
