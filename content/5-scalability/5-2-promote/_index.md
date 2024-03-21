---
title : "Promote Read Replica into standalone instance"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

You can use read replica promotion as a data recovery scheme if the source DB instance fails in case of Single-AZ instance configuration. This approach complements synchronous replication, automatic failure detection, and failover.

In the event of a failure, do the following:
- Promote the read replica.
- Direct database traffic to the promoted DB instance.
- Create a replacement read replica with the promoted DB instance as its source.

1. Open the Amazon RDS [console](https://console.aws.amazon.com/rds/home#databases) , choose your **rdspg-fcj-labs-read** instance.
2. We need to check that Replica Lag is close to zero to minimize the data loss of missing transactions from the primary instance.
![promote](/images/5/2/1.png)

3. When we are sure that lag is minimal, we return to the RDS Console, choose the replica instance and start the Promotion: in the **Actions** menu choose **Promote**:
![promote](/images/5/2/2.png)

4. In the **Settings** page, you can leave the defaults such as ***Enable automated backups*** and click on **Promote Read Replica**.
![promote](/images/5/2/3.png)

The promotion process can take several minutes or longer to complete, depending on the size of the read replica. The replica instance will get into the **Modifying** state during that time.

After that, the replica becomes available again and is shown as a standalone instance.
![promote](/images/5/2/4.png)


#### (OPTIONAL) AWS CLI

Alternatively you can promote the read replica using the AWS CLI as shown below:
{{%expand "Code" %}}
The following command creates the read replica:

```
AWSREGION=`aws configure get region`

aws rds promote-read-replica \
	--db-instance-identifier rdspg-fcj-labs-read \
	--backup-retention-period 1 \
	--region $AWSREGION

```
{{% /expand%}}
