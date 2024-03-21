---
title : "Áp dụng Custom Parameter Group"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 6.3 </b> "
---


Bây giờ chúng ta sẽ áp dụng Custom Parameter Group cho các phiên bản chính và bản sao của mình.

1. Nhấp vào menu **Databases** ở bên trái và chọn nút **rdspg-fcj-labs** và nhấp vào **Modify** ở trên cùng.
2. Kéo xuống **Additional Configuration**->**TDatabase options** và chọn custom parameter group mà bạn đã tạo trước đó trong tài khoản của mình (ví dụ: custom-pg) như hiển thị bên dưới.
![sửa đổi](/images/6/3/1.png)

3. Kéo xuống phía dưới và nhấp vào **Continue**.
4. Ở phần tiếp theo bạn chọn **Apply Immediately** như hình bên dưới.
![modify](/images/6/3/2.png)

5. Chọn nút radio bên cạnh DB identifier **rdspg-fcj-labs** như hiển thị bên dưới và xác minh rằng **Status** của cơ sở dữ liệu là **Modifying**.
![modify](/images/6/3/3.png)

6. Một số thay đổi tham số nhất định yêu cầu khởi động lại RDS instance, do đó bạn có thể xác minh xem có cần khởi động lại hay không bằng cách chọn primary instance **rdspg-fcj-labs** và trong tab **Configuration**, ở phía dưới bên trái đối diện với nhóm tham số đã sửa đổi **(custom-pg)** bạn sẽ thấy **Pending reboot** được viết cho biết cần phải khởi động lại phiên bản để áp dụng các thay đổi.
![modify](/images/6/3/4.png)

7. Vì vậy, bạn sẽ phải khởi động lại các instances để Parameter group hoạt động ( **In-sync** ).
8. Bằng cách chọn cơ sở dữ liệu **rdspg-fcj-labs** và bằng cách vào tab **Configuration**, giờ đây bạn có thể thấy sau khi khởi động lại instances, parameter group hiển thị là **In Sync**.
![sửa đổi](/images/6/3/5.png)

#### (Không bắt buộc) AWS CLI

Ngoài ra, bạn có thể áp dụng nhóm tham số tùy chỉnh bằng AWS CLI như dưới đây:
{{%expand "Code" %}}
Lệnh sau để áp dụng custom parameter group.
```

AWSREGION=`aws configure get region`

# Sửa đổi instance và thay đổi parameter group
aws rds modify-db-instance \
	--db-instance-identifier rdspg-fcj-labs \
	--db-parameter-group-name custom-pg \
	--apply-immediately \
	--region $AWSREGION

# Reboot instance 

aws rds reboot-db-instance \
	--db-instance-identifier rdspg-fcj-labs \
	--region $AWSREGION

```
{{% /expand%}}


Bạn có thể xác minh các thay đổi tham số từ câu lệnh psql. Để làm như vậy, hãy kết nối với instance:

```
psql -h rdspg-fcj-labs.cssuddr073hp.us-east-1.rds.amazonaws.com -U masteruser pglab

```

Sau đó chạy các lệnh sau

```
show log_connections;
show  log_min_duration_statement;

```

Output sẽ trông như dưới đây:

![modify](/images/6/3/6.png)


**Chúc mừng**, bạn đã hoàn thành xong bài thực hành về Parameter Groups.



