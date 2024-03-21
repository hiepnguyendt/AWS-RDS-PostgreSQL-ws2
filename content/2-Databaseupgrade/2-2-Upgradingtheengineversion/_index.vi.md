---
title : "Nâng cấp engine version"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

Minor version upgrades chỉ bao gồm những thay đổi tương thích ngược với các ứng dụng hiện có.

{{% notice warning %}}
Nếu PostgreSQL DB instance của bạn đang sử dụng Read Replicas, bạn phải nâng cấp tất cả các Read Replicas trước khi nâng cấp phiên bản nguồn. Bạn có thể làm theo các hướng dẫn tương tự bên dưới nhưng hãy áp dụng chúng trước để đọc bản sao.
{{% /notice %}}

Nếu phiên bản DB của bạn đang triển khai Multi-AZ thì cả bản sao trình ghi và bản sao dự phòng đều được nâng cấp. Phiên bản DB của bạn có thể không khả dụng cho đến khi quá trình nâng cấp hoàn tất.

Hãy nâng cấp db instance của chúng ta ngay bây giờ.

1. Trong ngăn điều hướng, hãy chọn **Databases**, sau đó chọn **DB instance** mà bạn muốn nâng cấp.

2. Chọn **Modify**. Trang **Modify DB Instance** sẽ xuất hiện.
    ![upgrade_engine](/images/2/2-2/1.png)

3. Tại mục **DB engine version**, chọn phiên bản mới.
    ![upgrade_engine](/images/2/2-2/2.png)

4. Chọn **Continue** và kiểm tra tóm tắt các sửa đổi.
    ![upgrade_engine](/images/2/2-2/3.png)

5. Để áp dụng các thay đổi ngay lập tức, hãy chọn **Apply immediately**.

6. Trên trang xác nhận, hãy xem lại các thay đổi của bạn. Nếu đúng, hãy chọn **Modify DB Instance** để lưu các thay đổi của bạn.
    ![upgrade_engine](/images/2/2-2/4.png)

Bạn có thể thấy phiên bản của mình đang được nâng cấp bằng cách quay lại trang RDS instance.
    ![upgrade_engine](/images/2/2-2/5.png)

#### (Không bắt buộc) AWS CLI
Ngoài ra, bạn có thể nâng cấp phiên bản bằng cách sử dụng **AWS CLI** như hiển thị bên dưới:
    {{%expand "AWS CLI" %}}
    Lệnh sau nâng cấp lên phiên bản 15.4.
    ```
    aws rds modify-db-instance --db-instance-identifier <your database name> --engine-version 15.4 --apply-immediately --region <your region>

    ```
    {{% /expand%}}
