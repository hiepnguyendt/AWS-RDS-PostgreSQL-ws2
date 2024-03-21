---
title : "Chuyển đổi Read Replica thành instance độc lập"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

Bạn có thể sử dụng Read replica promotion làm sơ đồ khôi phục dữ liệu nếu primary DB instance fail trong trường hợp cấu hình Single-AZ instance . Cách tiếp cận này bổ sung cho việc sao chép đồng bộ, tự động phát hiện lỗi và chuyển đổi dự phòng.

Trong trường hợp xảy ra lỗi, hãy làm như sau:
- Chuyển đổi read replica.
- Chuyển lưu lương của database đến promoted DB instance.
- Tạo một read replica thay thế với DB instance được chuyển đổi làm primary db.

1. Truy cập giao diện Amazon RDS [console](https://console.aws.amazon.com/rds/home#databases) , chọn **rdspg-fcj-labs-read** instance của bạn.
2. Chúng ta cần kiểm tra xem Replica Lag có gần bằng 0 hay không để giảm thiểu việc mất dữ liệu do các giao dịch bị thiếu từ primary instance.
![promote](/images/5/2/1.png)

3. Khi chúng ta chắc chắn rằng độ trễ là tối thiểu, chúng ta quay lại RDS console, chọn replica instance và bắt đầu chuyển đổi: trong menu **Actions**, chọn **Promote**:
![promote](/images/5/2/2.png)

4. Trong trang **Settings**, bạn có thể để các cài đặt mặc định như ***Enable automated backups*** và nhấp vào **Promote Read Replica**.
![promote](/images/5/2/3.png)

Quá trình chuyển đổi có thể mất vài phút hoặc lâu hơn để hoàn thành, tùy thuộc vào kích thước của read replica. Replica instance sẽ chuyển sang trạng thái **Modifying** trong thời gian đó.

Sau đó, bản sao sẽ trạng thái **available** và được hiển thị dưới dạng instance độc lập.
![promote](/images/5/2/4.png)


#### (Không bắt buộc) AWS CLI

Ngoài ra, bạn có thể chuyển đổi read replica bằng AWS CLI như dưới đây:
{{%expand "Code" %}}
Lệnh sau tạo read replica:

```
AWSREGION=`aws configure get region`

aws rds promote-read-replica \
	--db-instance-identifier rdspg-fcj-labs-read \
	--backup-retention-period 1 \
	--region $AWSREGION

```
{{% /expand%}}
