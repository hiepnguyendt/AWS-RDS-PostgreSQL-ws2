---
title : "CloudWatch Logs & Alerts"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---
#### Viewing CloudWatch Logs
When you created your database in the first lab you selected to publish logs to CloudWatch. Both the database and upgrade logs can be found in a CloudWatch Log Group associated with your database.

{{% notice note %}}
Amazon CloudWatch allows us to consolidate logs from various AWS services into a single location to help draw correlations between services, like the application plane and Amazon RDS database.
{{% /notice %}}

Open [CloudWatch Logs](https://console.aws.amazon.com/cloudwatch/home#logs:)  enter ``/aws/rds`` into the filter box and hit enter. Select the group assoicated with your database rds-pg-labs and take a look at the log stream.

![cloudwatch_log](/images/3/3-1/1.png)

#### Creating an RDS Database Alarm
1. Return to the [Database List](https://console.aws.amazon.com/rds/home#databases:)  Click on **rdspg-fcj-labs** database to go to the details page. The Database detail page has a number of tabs you can look at.

2. Now go to the **Log & events** tab and click on the **Create alarm** button.

![cloudwatch_log](/images/3/3-1/2.png)

3. Create a new alarm as shown below. 
- Send notifications : choose **Yes**
- Send notifications to : choose **New email or SMS topic**
- Topic name : fill yourname topic **AvgCPU-rdspg-fcj-labs** 
- With these recipients: fill your email address 
- Metric : choose **Average** of **CPUUtilization**
- Threshold : choose **>= 15 percent**
- Evaluation period : default setting
- Name of alarm : default

![cloudwatch_log](/images/3/3-1/3.png)

4. Now go to your email client for the email address you supplied for the notification. You should receive a confirmation email within a minute or two. Confirm your subscription by clicking the link in the email.

![cloudwatch_log](/images/3/3-1/4.png)

{{% notice note %}}
To receive the alarm email that will be generated later in the lab, you will need to confirm your subscription. You should receive a confirmation email from Amazon SNS within 90 seconds or so. Click on the link to confirm your subscription. We will be triggering this alarm in a future section
{{% /notice %}}

5. Finally, go to [AWS SNS Subscriptions](https://ap-southeast-1.console.aws.amazon.com/sns/v3/home?region=ap-southeast-1#/subscriptions) to check subscriptions status

![cloudwatch_log](/images/3/3-1/5.png)