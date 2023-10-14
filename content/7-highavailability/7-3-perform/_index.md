---
title : "Perform failover to verify high availability"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 7.3 </b> "
---

**RDS PostgreSQL** does provide customers an option to simulate the AZ failure and High Availability by offering the option to reboot the PostgreSQL Instance with the failover option. This option will initiate AZ level failover for the instance - the instance in the secondary AZ will become primary, and the instance in the primary will become the new secondary.

1. Open the Amazon RDS [console](https://console.aws.amazon.com/rds/home#databases)  and choose your **rdspg-fcj-labs** instance. Click on the **Actions** drop down on the right side and choose **Reboot**.

![HA](/images/7/3/1.png)


2. Make sure to check the **Reboot With Failover** option, and click **Reboot**.

![HA](/images/7/3/2.png)

3. Next, click on the **Logs & Events** tab to check the status of the failover. Observe the change in the events as the failover goes through various steps.

![HA](/images/7/3/3.png)

4. Now, go back to the EC2 instance terminal window, where the while loop command is still running. You will notice the change in the ***IP address*** as shown below. You will also notice a brief notice of interruption right around the time of failover. Afer that the database instance becomes available again and activity against the database completes successfully.

![HA](/images/7/3/4.png)


Also note that RDS endpoint has started resolving to a new IP address (from another AZ)\
***(Note: In your lab the IP will be different).***

{{% notice note %}}
You will also see the status and AZ in the Amazon RDS service console update as the failover happens but there may be a short delay before the console status and AZ are refreshed.
{{% /notice %}}


#### (OPTIONAL) AWS CLI

Alternatively you can reboot the instance with failover using the AWS CLI as shown below:

{{%expand "Code" %}}
The following command reboots the instance with failover.

```

AWSREGION=`aws configure get region`

aws rds reboot-db-instance \
	--db-instance-identifier rdspg-fcj-labs \
	--region $AWSREGION \
	--force-failover



```
{{% /expand%}}