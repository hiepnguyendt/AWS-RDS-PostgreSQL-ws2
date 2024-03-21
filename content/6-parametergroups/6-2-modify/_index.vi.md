---
title : "Điều chỉnh Custom Parameter Group"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 6.2. </b> "
---


Để sửa đổi Custom Parameter Group trong Amazon RDS, bạn có thể sử dụng AWS Console, AWS CLI hoặc AWS SDK.

Sử dụng AWS Console

1. Truy cập giao diện Amazon RDS [console](https://console.aws.amazon.com/rds/home#databases).
2. Tại ngăn điều hướng, chọn **Parameter groups**.
3. Chọn parameter group mà bạn muốn sửa đổi.
4. Dưới **Actions**, chọn **Edit**.
![modify](/images/6/2/1.png)

5. Trong hộp tìm kiếm parameters, hãy tìm tham số ``log_connections``.
![modify](/images/6/2/2.png)

6. Click vào cột **Values**.
![modify](/images/6/2/3.png)

7. Thay đổi giá trị thành 1 và nhấp vào **Save changes**.
8. Bây giờ hãy xóa hộp tìm kiếm và tìm tham số khác bằng cách nhập ``log_min_duration_statement``.
![modify](/images/6/2/4.png)

9. Trong hộp **Values**, nhập ``4000``.
![modify](/images/6/2/5.png)

10. Click **Save changes**. 

{{% notice note %}}
Lưu ý nếu bạn nhận được thông báo lỗi như “Error saving: This parameter group cannot be modified because it is currently being applied to DB Instance”, vui lòng đợi vài phút và thử lại.
{{% /notice %}}

11. Nhấp vào hộp kiểm tra đối với nhóm tham số mới và đi tới menu **actions** của Parameter group (ở trên cùng bên phải), chọn **Compare**.
![modify](/images/6/2/6.png)

12. Xác minh các thay đổi từ nhóm tham số mặc định và sau đó nhấp vào **Close**.
![modify](/images/6/2/7.png)

#### (Không bắt buộc) AWS CLI

Ngoài ra, bạn có thể sửa đổi custom parameter group bằng AWS CLI như dưới đây:
{{%expand "Code" %}}
Lệnh sau sửa đổi custom parameter group:
```

AWSREGION=`aws configure get region`

aws rds modify-db-parameter-group \
	--db-parameter-group-name custom-pg \
	--parameters "ParameterName='log_connections',ParameterValue=1,ApplyMethod=immediate"    "ParameterName='log_min_duration_statement',ParameterValue=4000,ApplyMethod=immediate" \
	--region $AWSREGION
```
{{% /expand%}}