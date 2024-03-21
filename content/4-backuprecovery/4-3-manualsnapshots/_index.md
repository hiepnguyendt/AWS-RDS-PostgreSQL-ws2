---
title : "Manual Snapshots"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 4.3. </b> "
---


#### Taking a manual backup of your database.

In addition to automated databsae backups, there are often times when you want to take an explicit backup of the database, just ahead of a major software release, or to refresh your staging enviornment. With AWS RDS these backups are called manual snapshots. RDS creates a storage volume snapshot of your DB instance, backing up the entire DB instance and not just individual databases. They are stored in Amazon S3 but they are not in a customer accessible bucket.

1. With your instance selected from the [list of databases](https://console.aws.amazon.com/rds/home#databases:) . Select **Actions** -> **Take Snapshot**
	![manual snapshot](/images/4/4-3/1.png)

2. On the **Take DB Snapshot** screen, enter a name for your snapshot (e.g. ``manual-snapshot-rdspg-fcj-labs``) and click **Take Snapshot**.
	![manual snapshot](/images/4/4-3/2.png)

3. As the snapshot is creating, you will be taken to the Snapshots page in the AWS RDS Console. Let's go to the [list of databases](https://console.aws.amazon.com/rds/home#databases:)  and take a look at the instance status. You should see its backing up.
	![manual snapshot](/images/4/4-3/3.png)

4. Once the database status returns to **available** return to the [list of snapshots](https://console.aws.amazon.com/rds/home#snapshots-list:) . Review all the snapshot details by scrolling to the right.
	![manual snapshot](/images/4/4-3/4.png)

#### (OPTIONAL) AWS CLI
Alternatively you can take a manual snapshot of the instance using the AWS CLI as shown below:

{{%expand "AWS CLI" %}}
The following command takes a manual snapshot of the instance.
```
AWSREGION=`aws configure get region`

aws rds create-db-snapshot \
	--db-instance-identifier rdspg-fcj-labs \
	--db-snapshot-identifier manual-snapshot-rdspg-fcj-labs \
	--region $AWSREGION


```

{{% /expand%}}