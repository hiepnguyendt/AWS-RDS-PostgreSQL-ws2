---
title : "Kiểm tra khả năng chịu tải nhanh hơn"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 3.5 </b> "
---

- Trong bài tập này, chúng tôi sẽ chỉ cho bạn cách làm cho khối lượng giao dịch của bạn nhanh hơn. Nhanh hơn có nghĩa là chúng tôi muốn hoàn thành nhiều giao dịch hơn trong cùng một khoảng thời gian. Để làm như vậy, chúng ta sẽ cần xác định các điểm nghẽn và khắc phục chúng.

- Trước tiên, hãy xác định số lượng giao dịch mỗi giây mà chúng ta đã thực hiện. Quay về với giao diện terminal của **MobaXterm** và xem số lượng giao dịch mỗi giây (**tps transactions per second**) khi sử dụng **pgbench** để kiểm tra khả năng với 200 client.
    ![load test faster](/images/3/3-5/1.png)

- Trong hai dòng cuối cùng của ảnh chụp màn hình ở trên, chúng ta thấy rằng khối lượng giao dịch của chúng ta có thể thực hiện khoảng 578 giao dịch mỗi giây (tps) trên db.t3.medium instance. Nếu môi trường lab của bạn sử dụng loại phiên bản khác, chẳng hạn như db.t3.large, bạn có thể thấy một giá trị khác.

- Bây giờ, hãy phân tích hiệu suất của chúng ta. Nói chung, bạn có thể xem xét hiệu suất của cơ sở dữ liệu ở mức độ vi mô và vĩ mô. Ở cấp độ vi mô, bạn muốn sẽ xem xét các câu lệnh SQL và xác định xem có bất kỳ câu lệnh SQL nào có thể được điều chỉnh hay không. Performance Insights là một công cụ tốt cho loại phân tích cấp vi mô này.

- Ở cấp độ vĩ mô, bạn muốn xem xét các số liệu về hiệu năng cho toàn bộ hệ thống. Bạn làm điều này để xem liệu có bất kỳ tài nguyên quan trọng nào (chẳng hạn như CPU, Bộ nhớ hoặc IO) đang bị quá tải và trở thành nút thắt cổ chai hay không. Các monitoring metrics trong RDS console (cũng có sẵn trong AWS CloudWatch) là công cụ tốt cho việc này.

- Hãy bắt đầu bằng cách bắt đầu ở cấp độ vi mô và xem xét khối lượng công việc của chúng ta trong Performance Insights. Chúng ta có thể nhận thấy rằng sự kiện chờ hàng đầu của chúng ta là **WALWriteLock** và **DataFileRead**. Chúng tôi đã xác định đây là sự kiện chờ đợi hàng đầu vì chúng tôi có thể thấy trực quan rằng nó chiếm nhiều diện tích nhất trong biểu đồ. Nói cách khác, nó có số phiên chờ đợi sự kiện này nhiều nhất (trung bình 100 phiên cơ sở dữ liệu đang chờ sự kiện đó ). Chúng ta cũng có thể xác định rằng Câu lệnh SQL được liên kết với **WALWriteLock** là **END**, trong PostgreSQL là câu lệnh closes/commits.

    *Counter metrics*
        ![data load test](/images/3/3-4/3.png)

    *Database load*
        ![data load test](/images/3/3-4/4.png)

    *Top SQL*
        ![data load test](/images/3/3-4/5.png)

- Có thể bạn chưa quen với sự kiện chờ **WALWriteLock**. WAL là **Write Ahead Log**, còn được gọi là nhật ký giao dịch. Các tệp này chứa bản ghi liên tục về COMMITs và COMMITs không thể quay lại máy khách cho đến khi thông tin WAL được ghi an toàn vào đĩa.

- Trong trường hợp của chúng ta, có vẻ như một số lượng lớn sessions đang sao lưu trên WALWriteLock và các phiên này đang chờ đến khi giao dịch của họ kết thúc (commits). Nói cách khác, hệ thống dường như không thể ghi các commits vào disk đủ nhanh.

- Bây giờ chúng ta hãy nhìn vào cấp độ vĩ mô. Để làm như vậy, hãy quay lại RDS console và chuyển đến **Monitoring** tab cho cơ sở dữ liệu của bạn. Điều này sẽ cho chúng ta thấy một số, số liệu trên toàn hệ thống. Đây là một ví dụ về những gì bạn sẽ thấy:
    ![load test faster](/images/3/3-5/3.png)

- Trong biểu đồ trên, hãy lưu ý rằng **CPUUtilization** không cao mặc dù chúng tôi có 200 kết nối đồng thời trên 2 vCPU (db.t3.medium). Vì vậy, chúng ta có thể nói rằng có vẻ như CPU không phải là điểm nghẽn cổ chai.

- Tiếp theo hãy xem xét một số số liệu I/O. Trong biểu đồ trên, hãy lưu ý rằng các waiting I/O requests của DiskQueueDepth đã trở nên lớn hơn. Ngoài ra, hãy lưu ý rằng IOPS ghi của chúng ta gần bằng 1000. Nếu bạn nhớ từ khi tạo cơ sở dữ liệu, chúng tôi đã quyết định sử dụng bộ lưu trữ Provisioned IOPS (io1) với 1000 IOPS. Vì vậy, có vẻ như chúng tôi có thể sắp đạt đến giới hạn hiện tại của dung lượng lưu trữ được phân bổ. Để xác nhận, bạn cũng có thể xem qua các biểu đồ và tìm biểu đồ Write Latency:

    ![load test faster](/images/3/3-5/4.png)

- Trong biểu đồ WriteLatency, chúng ta có thể thấy rằng độ trễ ghi rất cao trong khối lượng công việc. Nó sẽ phù hợp với giả thuyết của chúng tôi rằng chúng tôi đang đạt đến giới hạn của bộ nhớ được định cấu hình (1000 IOPS), điều này khiến độ sâu hàng đợi và độ trễ ghi tăng lên, đồng thời khiến việc ghi vào WAL mất nhiều thời gian hơn. Ngày càng có nhiều phiên cơ sở dữ liệu phải chờ lâu hơn để WAL được ghi khi họ thực hiện giao dịch của mình.

- Vì vậy, có vẻ như chúng ta có thể làm cho khối lượng công việc giao dịch của mình chạy nhanh hơn nếu tăng IOPS. Tin tốt là bộ nhớ mà RDS sử dụng có tính linh hoạt. Bạn có thể tăng/giảm cấu hình IOPS trong khi cơ sở dữ liệu chạy. Bạn thậm chí có thể thay đổi Loại lưu trữ (từ io1 sang gp2 hoặc ngược lại) trong khi cơ sở dữ liệu chạy nếu bạn muốn.

- Hãy sửa đổi cơ sở dữ liệu của chúng ta và chỉ định 3500 IOPS (tăng từ giá trị hiện tại là 1000). Để làm như vậy, hãy nhấp vào nút **Modify** ở đầu màn hình. Trên màn hình sửa đổi, thay đổi trường **Provisioned IOPS** thành 3500.

    ![load test faster](/images/3/3-5/5.png)

- Sau đó kéo xuống cuối trang sửa đổi và nhấp vào **Continue**.

- Trên trang **Summary and Scheduling**, hãy nhớ chọn **Apply Immediately**. Sau đó nhấp vào **Modify DB Instance**.
    ![load test faster](/images/3/3-5/6.png)

- Tại thời điểm này, bạn hãy truy cập [Databases List](https://console.aws.amazon.com/rds/home#databases:). Điều thú vị ở trang **Databases List** là bạn có thể dễ dàng làm mới trang để có thể theo dõi những thay đổi trong **Status**.
    ![load test faster](/images/3/3-5/7.png)

- Điều bạn muốn làm là xem **Status** trong khi RDS thay đổi IOPS. Cơ sở dữ liệu sẽ vẫn mở trong khi điều này xảy ra. Tuy nhiên, chúng tôi muốn đợi thay đổi hoàn tất trước khi chạy lại khối lượng công việc của mình. Quá trình này sẽ mất 10-15 phút. Đầu tiên, bạn sẽ thấy trạng thái có nội dung Modifying, sau đó sẽ có nội dung **Storage Optimization** khi hệ thống lưu trữ phụ trợ tự tối ưu hóa để cung cấp cấu hình IOPS mới.

#### (Không bắt buộc) AWS CLI
- Ngoài ra, bạn có thể sửa đổi IOPS của db instance bằng AWS CLI như dưới đây:
    {{%expand "Command" %}}
    Lệnh sau sửa đổi IOPS của instance.
    ```
    aws rds modify-db-instance --db-instance-identifier rds-pg-labs --iops 3500 --allocated-storage 100 --apply-immediately --region < your region >
    ```
    {{% /expand%}}

- Sau khi trạng thái trở về Available (hoặc nếu bạn thiếu kiên nhẫn, hãy đợi ít nhất cho đến khi thông báo “Storage Optimization”), sau đó quay lại Cloud9 và chạy lại khối lượng công việc tương tự như trước:
    ```

    pgbench --host <your database endpoint>--username masteruser --protocol=prepared -P 30 --time=300 --client=200 --jobs=200 <your dabase name>

    ```

- Bây giờ bạn sẽ thấy benchmark chạy nhanh hơn. Ví dụ: bây giờ bạn sẽ thấy khoảng 1300 giao dịch mỗi giây (tăng từ 578 giao dịch mỗi giây trước đó).

    ![load test faster](/images/3/3-5/8.png)

- Vì vậy, bằng cách tăng dung lượng **IOPS** của bộ lưu trữ, bạn có thể làm cho việc kiểm tra chạy nhanh hơn.
{{% notice tip %}}
Bạn không bị giới hạn sử dụng số liệu RDS/CloudWatch và Performance Insight. Bạn có thể mở rộng các chỉ số CloudWatch mặc định bằng [số liệu tùy chỉnh](https://github.com/awslabs/amazon-aurora-postgres-monitoring). Bạn có thể sử dụng các công cụ PostgreSQL khác như Bảng điều khiển được tích hợp trong pgAdmin. Hoặc một số DBA PostgreSQL sử dụng công cụ pgBadger để phân tích hiệu suất cơ sở dữ liệu. Để tìm hiểu về cách sử dụng pgBadger với RDS PostgreSQL, hãy đọc [tài liệu này](https://aws.amazon.com/blogs/database/optimizing-and-tuning-queries-in-amazon-rds-postgresql-based-on-native-and-external-tools/) .
{{% /notice %}}
{{% notice note %}}
 Trong bài thực hành này, chúng tôi đang sử dụng instance type **db.t3.mdedium**. Vì vậy, chúng ta cũng có thể thấy một số lượng lớn các sự kiện chờ đợi cho **DataFileRead**. Sự kiện chờ **DataFileRead** có thể cho biết rằng loại phiên bản của bạn có thể không có đủ bộ nhớ để đưa tập dữ liệu đang hoạt động vào bộ đệm dùng chung của cơ sở dữ liệu. Vì vậy, bạn có thể cân nhắc chuyển sang loại phiên bản có nhiều bộ nhớ hơn như một cách khả thi để giải quyết tình trạng chờ đợi **DataFileRead**.
{{% /notice %}}
