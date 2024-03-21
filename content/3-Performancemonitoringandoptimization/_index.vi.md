---
title : "Giám sát hiệu suất"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---


Giám sát tình trạng và hiệu suất cơ sở dữ liệu của bạn là một nhiệm vụ quan trọng. Trong phần thực hành này, bạn sẽ đặt cấu hình tự động cảnh báo cho một trong các số liệu về hiệu năng của cơ sở dữ liệu. Sau đó, bạn sẽ chạy khối lượng công việc được tạo dựa trên cơ sở dữ liệu Postgres của mình. Từ đó, bạn sẽ xem số liệu về hiệu năng trong RDS console và phân tích số liệu bằng công cụ RDS Performance Insights.

{{% notice info %}}
Amazon RDS cung cấp số liệu **CloudWatch** cho db instance của bạn mà không mất thêm phí. Bạn có thể sử dụng **RDS Management Console** để xem các số liệu vận hành chính, bao gồm mức compute/memory/storage capacity utilization, I/O activity và instance connections. Amazon RDS cũng cung cấp tính năng **Enhanced Monitoring**, cung cấp quyền truy cập vào hơn 50 số liệu, bao gồm CPU, memory, file system, và disk I/O; và Performance Insights, một công cụ dễ sử dụng giúp bạn nhanh chóng phát hiện các vấn đề về hiệu năng.
{{% /notice %}}

#### Nội dung
1. [CloudWatch Logs và Alerts](3-1-Cloudwatchlogs&alerts/)
2. [Tạo một số hoạt động cho cơ sở dữ liệu ](3-2-createsomedbactivity/)
3. [Đánh giá hiệu năng](3-3-reviewingperformance/)
4. [Kiểm tra khả năng chịu tải của cơ sở dữ liệu](3-4-databaseloadtest/)
5. [Kiểm tra khả năng chịu tải nhanh hơn](3-5-makeyourloadtestrunfaster/)


