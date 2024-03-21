---
title : "Setup high availability for RDS PostgreSQL (Multi-AZ)"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 7.1. </b> "
---

#### Verify Multi-AZ settings for RDS PostgreSQL

Open the Amazon RDS [console](https://console.aws.amazon.com/rds/home#databases). Scroll to the right of the databases settings in the list for your **rdspg-fcj-labs** database to review its Multi-AZ configuration.

![HA](/images/7/1/1.png)

*We could confirm that Multi-AZ was not configured, so we need to setup our database for high availability in the next section.*

#### Setup high availability for RDS PostgreSQL (Multi-AZ)

Amazon RDS Multi-AZ deployments provide enhanced availability and durability for RDS database (DB) instances, making them a natural fit for production database workloads. When you provision a Multi-AZ DB Instance, Amazon RDS automatically creates a primary DB Instance and synchronously replicates the data to a standby instance in a different Availability Zone (AZ). Each AZ runs on its own physically distinct, independent infrastructure, and is engineered to be highly reliable. In case of an infrastructure failure, Amazon RDS performs an automatic failover to the standby (or to a read replica in the case of Amazon Aurora), so that you can resume database operations as soon as the failover is complete. Since the endpoint for your DB Instance remains the same after a failover, your application can resume database operation without the need for manual administrative intervention. More details can be seen [here](https://aws.amazon.com/blogs/database/amazon-rds-under-the-hood-multi-az/).

1. To configure RDS PostgreSQL instance for High availability we need to modify the instance. Select **rdspg-fcj-labs** instance and press the **Modify** button on top.

2. In the **Modify DB Instance: rdspg-fcj-labs** page, scroll down to the **Availability & durability** section and make sure to select **Create a standby instance (recommended for production usage)**.
![HA](/images/7/1/2.png)

3. Scroll to the bottom and click on **Continue**. On the next window you will notice that the attribute **Multi-AZ** deployment is changed to **Yes** in the New Value column. In the Scheduling of modifications section select **Apply immediately** and click **Modify DB Instance** at the bottom.
![HA](/images/7/1/3.png)

***Now the instance will change to **Modifying** status and we need to wait until this operation completes - this usually takes ~5-10 mins.***

4. When status become **Available** again, click on the **rdspg-fcj-labs** instance identifier to review its settings.
![HA](/images/7/1/4.png)

***So we configured our RDS PostgreSQL instance for high availability and in the next section we will test this capability.***


#### (OPTIONAL) AWS CLI

Alternatively you can convert the instance to Multi-AZ using the AWS CLI as shown below:
{{%expand "Code" %}}
The following command converts the instance to Multi-AZ.

```

AWSREGION=`aws configure get region`

aws rds modify-db-instance \
	--db-instance-identifier rdspg-fcj-labs \
	--multi-az \
	--apply-immediately \
	--region $AWSREGION


```
{{% /expand%}}