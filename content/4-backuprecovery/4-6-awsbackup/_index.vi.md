---
title : "AWS Backup"
date :  "`r Sys.Date()`" 
weight : 6
chapter : false
pre : " <b> 4.6. </b> "
---

[AWS Backup](https://aws.amazon.com/backup/)  là dịch vụ sao lưu được quản lý toàn phần giúp dễ dàng tập trung và tự động hóa việc sao lưu dữ liệu trên các dịch vụ AWS. Với AWS Backup, bạn có thể tạo các chính sách sao lưu được gọi là **backup plans**. Bạn có thể sử dụng các gói này để xác định các yêu cầu sao lưu của mình, chẳng hạn như tần suất sao lưu dữ liệu và thời gian lưu giữ các bản sao lưu đó.

**AWS Backup** cho phép bạn áp dụng **backup plans** cho tài nguyên AWS của mình bằng cách **simply tagging them**. AWS Backup sẽ tự động sao lưu tài nguyên AWS của bạn theo kế hoạch sao lưu mà bạn đã xác định.

{{% notice note %}}
*RDS/PostgreSQL sẽ tự động sao lưu cơ sở dữ liệu của bạn và giữ lại các bản sao lưu đó trong khoảng thời gian lưu giữ của bạn, tối đa 35 ngày. Các bản sao lưu được tạo trước thông qua **AWS Backup** được coi là snapshot thủ công và sẽ tồn tại cho đến khi bị xóa.*
{{% /notice %}}

#### Tạo Service Role cho AWS Backup
Để **AWS Backup** thay mặt bạn thực hiện các hoạt động, chúng tôi cần gán cho nó 1 service role.
1. Tại giao diện [IAM Console](https://console.aws.amazon.com/iamv2/home#/roles)  chọn **Create role**
    ![aws backups](/images/4/4-6/1.png)

2. Chọn **AWS Service** và sau đó chọn **AWS Backup**, cuối cùng click **Next**. 
![aws backups](/images/4/4-6/2.png)

3. Trong bước thêm quyền, hãy sử dụng bộ lọc bằng cách nhập **AWSBackupServiceRole** và chọn: **AWSBackupServiceRolePolicyForBackup** và **AWSBackupServiceRolePolicyForRestore**, sau đó click vào **Next**.
![aws backups](/images/4/4-6/3.png)

4. Đặt tên cho role, ``rdspg-AWSBackupServiceRole``, xem lại chi tiết rồi bấm vào **Create Role**.
![aws backups](/images/4/4-6/4.png)
![aws backups](/images/4/4-6/5.png)
![aws backups](/images/4/4-6/6.png)
![aws backups](/images/4/4-6/7.png)

#### Sao lưu theo yêu cầu từ AWS Backup

Trở lại giao diện [AWS Backup Console](https://console.aws.amazon.com/backup/home) . 
1. Đầu tiên tạo **Backup Vault**, Nó cho phép tổ chức quản lý và tổ chức các bản sao lưu một cách hiệu quả, giúp đảm bảo tính toàn vẹn và khả năng khôi phục dữ liệu trong trường hợp xảy ra sự cố, mất mát dữ liệu hoặc các vấn đề khác. Click vào **Backup Vaults** từ menu bên trái, sau đó chọn **Create Backup vault**.
![aws backups](/images/4/4-6/8.png)

2. Đặt tên cho vault là ``rdspg-backup-vault`` và nhấp vào **Create Backup vault**.
![aws backups](/images/4/4-6/9.png)

3. Với vault đã được tạo, giờ đây bạn có thể tạo bản sao lưu theo yêu cầu. Chọn **Protected resources** từ menu bên trái, sau đó nhấp vào **Create on-demand backup**.
![aws backups](/images/4/4-6/10.png)

4. Hoàn tất hộp thoại bằng cách chọn RDS và loại tài nguyên của bạn, sau đó chọn cơ sở dữ liệu **rdspg-fcj-labs**. Chọn **backup vault** bạn vừa tạo. Chọn chọn **IAM role** và chọn **rdspg-AWSBackupServiceRole** của bạn từ danh sách thả xuống và cuối cùng nhấn **Create on-demand backup**.
![aws backups](/images/4/4-6/11.png)

5. Bạn sẽ thấy bản sao lưu trong danh sách sao lưu của mình. Điều này giống như snapshot thủ công nhưng bản sao lưu của bạn được tổ chức thành một kho lưu trữ dự phòng.
![aws backups](/images/4/4-6/12.png)


#### Thiết lập Backup Plan trong AWS Backup

{{% notice note %}}
*Việc lựa chọn tài nguyên từ backup plan có thể được thực hiện bằng cách sử dụng resource tags hoặc tham chiếu trực tiếp.*
{{% /notice %}}

Bây giờ hãy thiết lập backup plan bằng cách sử dụng resource tags. Sử dụng **AWS Backup** theo cách này sẽ đảm bảo rằng các tài nguyên mới tạo được gắn thẻ đúng cách sẽ được sao lưu thông qua **AWS Backup**

1. Bắt đầu bằng cách thêm thẻ tài nguyên vào cơ sở dữ liệu của bạn từ [RDS Console](https://console.aws.amazon.com/rds/home#databases:) , nhấp vào **rdspg-fcj-labs** instance, sau đó chọn tab **Tags** và chọn **Add**.
![aws backups](/images/4/4-6/13.png)

2. Thêm ``Environment`` cho ``production`` và ``ResourceType`` cho ``rdspg-fcj`` gắn thẻ vào **rdspg-fcj-labs** database.

3. Tại giao diện [AWS Backup Console](https://console.aws.amazon.com/backup/home) chọn **Backup plans** từ menu bên trái, sau đó chọn **Create backup plan**.
![aws backups](/images/4/4-6/14.png)

4. Chọn **Build new plan**, đặt tên cho Backup plan ``rdspg-backup-plan``, đặt tên cho Backup Rule ``DailyBackups``, chọn Backup vault **rdspg-backup-vault**, để mọi thứ khác ở mặc định, sau đó click **Create plan**.
![aws backups](/images/4/4-6/15.png)
![aws backups](/images/4/4-6/16.png)

5. Hoàn tất hộp thoại bằng cách nhập tên gán tài nguyên, ``rdspg-bp-resource-selection``, chọn **IAM role** và chọn IAM role đã tạo trước đó **rdspg-AWSBackupServiceRole**. 
![aws backups](/images/4/4-6/17.png)
6. Cuối cùng thêm các thẻ **Environment** và **ResoureType** như trong ảnh chụp màn hình.
![aws backups](/images/4/4-6/18.png)

**Chúc mừng!** Bây giờ bạn đã tạo gói dự phòng dựa trên thẻ, mọi cơ sở dữ liệu bạn tạo trong tương lai với các thẻ này sẽ được tự động sao lưu bằng gói này.


