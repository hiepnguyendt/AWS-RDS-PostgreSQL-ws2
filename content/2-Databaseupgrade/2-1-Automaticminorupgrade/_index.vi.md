---
title : "Tự động nâng cấp phiên bản nhỏ"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

Khi tạo db instance, bạn sẽ thấy checkbox cho phép bật tính năng tự động nâng cấp phiên bản nhỏ như hình ảnh sau.
      ![auto_minor](/images/2/2-1/1.png)

Chúng ta có thể tìm hiểu xem tùy chọn này có được bật hay không bằng cách chạy lệnh sau:
- Thay thế cơ sở dữ liệu và khu vực của bạn
 ```
      -query 'DBInstances[*].AutoMinorVersionUpgrade'
 ```
![auto_minor](/images/2/2-1/2.png)

Or by going to the **RDS console**, clicking on the db instance and select the **Maintenance & backups** tab.

![auto_minor](/images/2/2-1/3.png)

In this case, you have not enabled **Auto minor version upgrade** in the first create database.\
Now, you can enable by click **Modify** button, then scroll down and select **Enabled Auto minor version upgrade**

![auto_minor](/images/2/2-1/4.png)

A PostgreSQL DB instance is automatically upgraded during your maintenance window if the following criteria are met:

- The DB instance has the Auto minor version upgrade option enabled.
- The DB instance is running a minor DB engine version that is less than the current automatic upgrade minor version.