---
title : "Create Read-replica to provide read scalability"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

**Amazon RDS Read Replicas** provide enhanced performance and durability for RDS instances. They make it easy to elastically scale out beyond the capacity constraints of a single DB instance for read-heavy database workloads. You can create from one to 5 replicas of a given source DB Instance and serve high-volume application read traffic from multiple copies of your data, thereby increasing aggregate read throughput. Read replicas can also be promoted when needed to become standalone DB instances. Find more features of RDS Read-replicas [here](https://aws.amazon.com/rds/features/read-replicas/).

![create](/images/5/rdspg.png)

**Amazon RDS creates a second DB instance** using a snapshot of the source DB instance. It will uses the engines native asynchronous replication to update the read replica whenever there is a change to the source DB instance. The read replica operates as a DB instance that allows only read-only connections; applications can connect to a read replica just as they would to any DB instance.

1. Find the RDS PostgreSQL instance **rdspg-fcj-labs** under the **Databases** menu and click on the radio button in front of the DB Instance. Click on the **Actions** drop down on the right side and choose **Create Read Replica.**

![create](/images/5/1/1.png)

2. For **Settings**, you must provide a DB Instance Identifier (i.e. the name for this instance). Something like "rdspg-fcj-labs-read" works.

![create](/images/5/1/2.png)

3. For the **Instance Configurations** and **AWS Region** sections, leave the default values. **Uncheck** the **Enable storage autoscaling** option.

{{% notice note %}}
Notice that you can optionally size the Read Replica use a different instance tier than your primary instance. In your case, the Destination Region will likely be different than what you see in the screenshot and that is OK.
{{% /notice %}}

![create](/images/5/1/3.png)

4. For **Connectivity**, leave the defaults.

{{% notice note %}}
Leave the Network Security settings at their default.
{{% /notice %}}

5. For **Availability**, select Multi-AZ DB instance.

![create](/images/5/1/4.png)

6. For simplicity, we’ll leave the rest at their defaults. Scroll to the bottom and click **Create read replica**.

![create](/images/5/1/7.png)

*It will take about 10 minutes for your read replica to be created. Your primary database continues to be available as this happens.*

7. Once the Read Replica status is **Available** on the **Databases** page (you may need to use the refresh icon to see the latest **Status**), click on the new read replica instance.

![create](/images/5/1/8.png)

8. On the detail page for the read replica, first notice the endpoint. The read replica has a different endpoint than your primary instance

![create](/images/5/1/9.png)

9. Next, scroll down to view the replication details. You can see that our primary instance is **replicating** to our read replica instance.

![create](/images/5/1/10.png)

You can then connect to the read replica using its endpoint and the same username and password as the source instance. For example, connect and execute the following query to check the replication lag between the source and the replica:

```
SELECT extract(epoch from now() - pg_last_xact_replay_timestamp()) AS replica_lag;

```

![create](/images/5/1/11.png)


RDS PostgreSQL also allows you to promote a read replica to be a standalone instance. We will do that in the next Lab.

**Congratulations**: In this lab, you added a read replica instance to your configuration to provide additional read scalability to your application.


#### (OPTIONAL) AWS CLI
Alternatively you can create the read replica instance using the AWS CLI as shown below:

{{%expand "Code" %}}
The following command creates the read replica:

```
AWSREGION=`aws configure get region`

aws rds create-db-instance-read-replica \
	--db-instance-identifier rdspg-fcj-labs-read \
	--source-db-instance-identifier rdspg-fcj-labs \
	--db-instance-class db.t3.medium \
	--region $AWSREGION
```
{{% /expand%}}
