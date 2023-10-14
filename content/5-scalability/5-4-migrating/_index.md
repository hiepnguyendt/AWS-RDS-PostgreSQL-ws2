---
title : "Migrating to a Multi-AZ DB cluster using a read replica"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

**Amazon RDS Read Replicas** provide enhanced performance and durability for RDS instances. They make it easy to elastically scale out beyond the capacity constraints of a single DB instance for read-heavy database workloads. You can create from one to 5 replicas of a given source DB Instance and serve high-volume application read traffic from multiple copies of your data, thereby increasing aggregate read throughput. Read replicas can also be promoted when needed to become standalone DB instances. Find more features of RDS Read-replicas [here](https://aws.amazon.com/rds/features/read-replicas/) .

To ***migrate a Single-AZ deployment or Multi-AZ DB instance deployment to a Multi-AZ DB cluster deployment with reduced downtime, you can create a Multi-AZ DB cluster read replica***. Please visit [Working with Multi-AZ DB cluster read replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_MultiAZDBCluster_ReadRepl.html)  for additional details.



Amazon RDS creates a second DB instance using a snapshot of the source DB instance. It will uses the engines' native asynchronous replication to update the read replica whenever there is a change to the source DB instance. The read replica operates as a DB instance that allows only read-only connections; applications can connect to a read replica just as they would to any DB instance.

To migrate a Single-AZ deployment or Multi-AZ DB instance deployment to a Multi-AZ DB cluster deployment with reduced downtime, you can create a Multi-AZ DB cluster read replica.

1. Find RDS PostgreSQL instance ***rdspg-fcj-labs*** under the **Databases** menu and click on the radio button in front of the DB Instance. Click on the Actions drop down on the right side and choose **Create Read Replica.**

![migrating](/images/5/3/1.png)

2. For **Settings**, you must provide a DB Instance Identifier (i.e. the name for this instance).

![migrating](/images/5/3/2.png)

3. For the **Availability** section, select ***Multi-AZ DB Cluster*** - new deployment option.

![migrating](/images/5/3/4.png)


4. For simplicity, we’ll leave the rest at their defaults. Scroll to the bottom and click **Create read replica**

After you click **Create read replica**, you’ll be taken back to the **Databases** page. Click the refresh icon to refresh the page and you should see the read replica being created.

![migrating](/images/5/3/5.png)

*It will take about 10 minutes for your read replica to be created. Your primary database continues to be available as this happens.*

5. Once the Read Replica status is **Available** on the **Databases** page ( you may need to use the refresh icon to see the latest Status ), click on the new read replica instance.

![migrating](/images/5/3/6.png)

6. On the detail page for the Multi-AZ DB replica cluster, first notice the endpoint(s). The replica has a different endpoint(s) than your primary instance.

![migrating](/images/5/3/7.png)

7. Next, scroll down to view the replication details. You can see that our primary instance is replicating to our read replica instance.

![migrating](/images/5/3/8.png)

You can then connect to the read replica using its endpoint and the same username and password as the source instance. For example, connect and execute the following query to check the replication lag between the source and the replica:

```
SELECT extract(epoch from now() - pg_last_xact_replay_timestamp()) AS replica_lag;

```

![migrating](/images/5/3/9.png)

***You can also promote this Multi-AZ read replica cluster to a stand-alone cluster. We will do that in the next step.***

8. Click on **Actions**, and select **Promote** to detach and promote the ***rdspg-fcj-labs-read-test*** Multi-AZ DB Cluster by selecting **Promote read replica**

![migrating](/images/5/3/10.png)

It will take about 1-2 minutes for your read replica to be detached and promoted as an independent RDS DB Cluster with Multi-AZ DB Cluster deployment option.

At this point, you have successfully migrated your RDS PostgreSQL instance with Multi-AZ DB Instance deployment mode to the RDS PostgreSQL Multi-AZ DB Cluster deployment option.

**Congratulations**: In this lab, you added a Multi-AZ DB Cluster read replica instance to your configuration to migrate your application/database from Multi-AZ DB Instance deployment option to the Multi-AZ DB Cluster deployment option.

#### (OPTIONAL) AWS CLI

Alternatively you can promote the read replica using the AWS CLI as shown below:

{{%expand "Code" %}}
The following command creates the read replica:

```
ARN=`aws rds describe-db-instances --db-instance-identifier rdspg-fcj-labs --query 'DBInstances[].DBInstanceArn' --output text --region $AWSREGION`

aws rds create-db-cluster \
        --db-cluster-identifier rdspg-fcj-labs-read-test \
        --engine postgres \
        --replication-source-identifier $ARN \
        --db-cluster-instance-class db.r5d.large \
        --storage-type io1 --iops 1000 \
        --region $AWSREGION 

aws rds promote-read-replica-db-cluster \
        --db-cluster-identifier rdspg-fcj-labs-read-test

```
{{% /expand%}}
