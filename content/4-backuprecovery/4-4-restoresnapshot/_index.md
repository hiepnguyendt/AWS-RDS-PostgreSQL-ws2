---
title : "Restore Snapshot"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 4.4. </b> "
---

#### Restoring from manual snapshot

Database backups are of very little value unless they can be used to restore the database. In this section we will take the manual snapshot just created and restore the PostgreSQL database.

Select the snapshot you created in the prior section from [the list](https://console.aws.amazon.com/rds/home#snapshots-list:)  and select **Actions**, then click **Restore Snapshot**

![Restore snapshot](/images/4/4-4/1.png)

{{% notice info %}}
*When restoring from a snapshot, a **NEW RDS database instance** is created, the original instance will continue to run normally*
{{% /notice %}}

Complete the Restore DB Instance page using the defaults except for the DB Instance Identifier where you can enter ``rdspg-fcj-labs-restore-manual-snapshot``, Select ``db.t3.meidum`` as instance type and then select the Restore DB Instance at the bottom of the form.

![Restore snapshot](/images/4/4-4/2.png)

As the restore initiates you are taken to the list of databases. You can monitor the status of the restoration and refresh the list until the restored database's status is **Available**.


#### (OPTIONAL) AWS CLI

Alternatively you can restore an instance from a manual snapshot using the AWS CLI as shown below:

{{%expand "AWS CLI" %}}
The following command takes a manual snapshot of the instance.
```
AWSREGION=`aws configure get region`

aws rds restore-db-instance-from-db-snapshot \
	--db-instance-identifier rdspg-fcj-labs-restore-manual-snapshot \
	--db-snapshot-identifier manual-snapshot-rdspg-fcj-labs \
	--db-instance-class db.t3.medium \
	--region $AWSREGION

```

{{% /expand%}}