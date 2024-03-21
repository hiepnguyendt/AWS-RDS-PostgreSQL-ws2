---
title : "Creating a DB instance read replica from a Multi-AZ DB cluster"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

**Amazon RDS Read Replicas** provide enhanced performance and durability for RDS instances. They make it easy to elastically scale out beyond the capacity constraints of a single DB instance for read-heavy database workloads. You can create a DB instance read replica from a Multi-AZ DB cluster in order to scale beyond the compute or I/O capacity of the cluster for read-heavy database workloads. You can direct this excess read traffic to one or more DB instance read replicas. You can also use read replicas to migrate from a Multi-AZ DB cluster to a DB instance.

Please visit [Working with Multi-AZ DB cluster read replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_MultiAZDBCluster_ReadRepl.html) for additional details.

A DB instance read replica of a Multi-AZ DB cluster is different than the reader DB instances of the Multi-AZ DB cluster in the following ways:

- The reader DB instances act as automatic failover targets, while DB instance read replicas do not.
- Reader DB instances must acknowledge a change from the writer DB instance before the change can be committed. For DB instance read replicas, however, updates are asynchronously copied to the read replica without requiring acknowledgement.
- Reader DB instances always share the same instance class, storage type, and engine version as the writer DB instance of the Multi-AZ DB cluster. DB instance read replicas, however, don’t necessarily have to share the same configurations as the source cluster.
- You can promote a DB instance read replica to a standalone DB instance. You can’t promote a reader DB instance of a Multi-AZ DB cluster to a standalone instance.
- The reader endpoint only routes requests to the reader DB instances of the Multi-AZ DB cluster. It never routes requests to a DB instance read replica.

For more information, see [Overview of Multi-AZ DB clusters](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/multi-az-db-clusters-concepts.html#multi-az-db-clusters-concepts-overview).


To create a read replica, specify a Multi-AZ DB cluster as the replication source. One of the reader instances of the Multi-AZ DB cluster is always the source of replication, not the writer instance. This condition ensures that the replica is always in sync with the source cluster, even in cases of failover.

1. Find RDS PostgreSQL instance ***rdspg-fcj-labs-read-test*** under the **Databases** menu and click on the radio button in front of the DB Instance. Click on the **Actions** drop down on the right side and choose **Create Read Replica.**
![outbound](/images/5/4/1.png)

2. For **Settings**, you must provide a DB Instance Identifier (i.e. the name for this instance). 
![outbound](/images/5/4/2.png)

3. For the **Availability** section, select ***Multi-AZ DB instance*** deployment option.
![outbound](/images/5/4/3.png)

4. For simplicity, we’ll leave the rest at their defaults. Scroll to the bottom and click **Create read replica**.

After you click **Create read replica**, you’ll be taken back to the **Databases** page. Click the refresh icon to refresh the page and you should see the read replica being created.

*It will take about 10 minutes for your read replica to be created. Your primary database continues to be available as this happens*

5. Once the Read Replica status is **Available** on the **Databases** page (you may need to use the refresh icon to see the latest Status), click on the new read replica instance.
![outbound](/images/5/4/5.png)

6. On the detail page for the read replica, first notice the endpoint. The read replica has a different endpoint than your primary instance.

7. Next, scroll down to view the replication details. You can see that our primary cluster is replicating to our read replica instance.

You can then connect to the read replica using its endpoint and the same username and password as the source cluster. For example, connect and execute the following query to check the replication lag between the source and the replica:

```
SELECT extract(epoch from now() - pg_last_xact_replay_timestamp()) AS replica_lag;

```

**Congratulations**: In this lab, you added a Multi-AZ DB Instance read replica instance to your Multi-AZ DB Cluster configuration for scaling out read workload.

#### (OPTIONAL) AWS CLI

Alternatively you can promote the read replica using the AWS CLI as shown below:
{{%expand "Code" %}}
The following command creates the read replica:

```

aws rds create-db-instance-read-replica \
   --db-instance-identifier taz-maz-replica \
   --source-db-cluster-identifier rdspg-fcj-labs-read-test


```
{{% /expand%}}