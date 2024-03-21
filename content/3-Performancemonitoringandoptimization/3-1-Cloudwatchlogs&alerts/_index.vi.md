---
title : "CloudWatch Logs & Alerts"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---
#### Xem CloudWatch Logs
Khi tạo cơ sở dữ liệu trong bài thực hành đầu tiên, bạn đã chọn xuất bản nhật ký lên CloudWatch. Bạn có thể tìm thấy cả cơ sở dữ liệu và nhật ký nâng cấp trong Nhóm nhật ký CloudWatch được liên kết với cơ sở dữ liệu của bạn.

{{% notice note %}}
Amazon CloudWatch cho phép chúng ta tổng hợp nhật ký từ nhiều dịch vụ AWS khác nhau vào một vị trí duy nhất để giúp rút ra mối tương quan giữa các dịch vụ, như nền ứng dụng và cơ sở dữ liệu Amazon RDS.
{{% /notice %}}

Truy cập giao diện của [CloudWatch Logs](https://console.aws.amazon.com/cloudwatch/home#logs:) nhập ``/aws/rds`` vào trong filter box và nhấn **enter**. Chọn nhóm được liên kết với cơ sở dữ liệu ***rds-pg-labs*** của bạn và xem luồng nhật ký.
    ![cloudwatch_log](/images/3/3-1/1.png)

#### Tạo cảnh báo cơ sở dữ liệu RDS
1. Quay lại giao diên [Database List](https://console.aws.amazon.com/rds/home#databases:) Nhấp vào cơ sở dữ liệu **rdspg-fcj-labs** để chuyển đến trang chi tiết.

2. Bây giờ hãy chuyển đến tab **Log & events** và nhấp vào nút ****Create alarm**.
    ![cloudwatch_log](/images/3/3-1/2.png)

3. Tạo một cảnh báo mới như hình dưới đây.
- Send notifications : chọn **Yes**
- Send notifications to : chọn **New email or SMS topic**
- Topic name : đặt tên: **AvgCPU-rdspg-fcj-labs** 
- With these recipients: nhập địa chỉ mail của bạn 
- Metric : chọn **Average** of **CPUUtilization**
- Threshold : chọn **>= 15 percent**
- Evaluation period : default setting
- Name of alarm : default
    ![cloudwatch_log](/images/3/3-1/3.png)

4. Bây giờ hãy truy cập vào email của bạn đã cũng cấp ở bước trên. Bạn sẽ nhận được email xác nhận trong vòng một hoặc hai phút. Xác nhận đăng ký của bạn bằng cách nhấp vào liên kết trong email.
    ![cloudwatch_log](/images/3/3-1/4.png)

{{% notice note %}}
Để nhận được email cảnh báo, bạn cần phải xác nhận đăng ký của mình. Bạn sẽ nhận được email xác nhận từ Amazon SNS trong vòng 90 giây hoặc lâu hơn. Nhấp vào liên kết để xác nhận đăng ký của bạn. Chúng ta sẽ kích hoạt cảnh báo này trong phần sau.
{{% /notice %}}

5. Cuối cùng, hãy truy cập [Đăng ký AWS SNS](https://ap-southeast-1.console.aws.amazon.com/sns/v3/home?region=ap-southeast-1#/subscriptions) để kiểm tra trạng thái đăng ký.
    ![cloudwatch_log](/images/3/3-1/5.png)