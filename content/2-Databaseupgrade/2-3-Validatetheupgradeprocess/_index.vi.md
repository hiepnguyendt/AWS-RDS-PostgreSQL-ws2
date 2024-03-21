---
title : "Kiểm tra quá trình nâng cấp"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

Sau vài phút bạn sẽ thấy cơ sở dữ liệu đã hoàn tất quá trình nâng cấp và trạng thái sẽ trở lại **Available**.
  ![validate_process](/images/2/2-3/1.png)

Nhấp vào instance, sau đó nhấp vào tab **Configuration** và bạn sẽ thấy database engine version đã chuyển từ **15.3** sang **15.4**.
  ![validate_process](/images/2/2-3/2.png)

Hoặc nếu bạn thích sử dụng lệnh **CLI** hơn, hãy nhập vào bảng điều khiển:

``````````
aws rds describe-db-instances --db-instance-identifier < your database name > --region < your region > --query DBInstances[*].EngineVersion

``````````

Bạn có thể thấy **engine version** sau khi chạy lệnh trên:
![validate_process](/images/2/2-3/3.png)
