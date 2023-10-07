---
title : "Performance monitoring"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

{{% notice note %}}
This chapter assumes you have already created an RDS PostgreSQL instance in the first worshop.
{{% /notice %}}

Monitoring the health and preformance of your database is an important task. In this lab, you will configure an automated alert for one of your database performance metrics. Then you will run a generated workload against your Postgres database. From there you will view the performance metrics in the RDS Console and analyze the metrics using the RDS Performance Insights tool.

{{% notice info %}}
Amazon RDS provides CloudWatch metrics for your database instances at no additional charge. You can use the RDS Management Console to view key operational metrics, including compute/memory/storage capacity utilization, I/O activity, and instance connections. Amazon RDS also provides Enhanced Monitoring, which provides access to over 50 metrics, including CPU, memory, file system, and disk I/O; and Performance Insights, an easy-to-use tool that helps you quickly detect performance problems.
{{% /notice %}}

#### This lab contains following tasks:
1. [CloudWatch Logs & Alerts](3-1-Cloudwatchlogs&alerts/)
2. [Create some DB Activity](3-2-createsomedbactivity/)
3. [Reviewing Performance](3-3-reviewingperformance/)
4. [Database Load Test](3-4-databaseloadtest/)
5. [Make your Load Test run faster](3-5-makeyourloadtestrunfaster/)


