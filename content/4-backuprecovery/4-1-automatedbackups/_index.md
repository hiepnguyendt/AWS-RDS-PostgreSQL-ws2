---
title : "Automated Backups"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 4.1.</b> "
---
#### Reviewing your Instance's Automatic Backups

Let's start by looking at the backups of the database you have created. A full database backup is taken immediately following database creation.

Open the [Amazon RDS Console](https://console.aws.amazon.com/rds/home#databases:)
Click on **rdspg-fcj-labs** or the instance name you specified to reveal its details. Then select the **Maintenance & backups** tab.

![automated backups](/images/4/4-1/0.png)

Scroll down to the automated backup section and review the details. Note the backup window for the database and the latest possible restore time. Look at the available snapshots for the database. An automated snapshot from that initial database backup also appears.

![automated backups](/images/4/4-1/1.png)

#### (OPTIONAL) AWS CLI
Alternatively you can view the instance's backups using the AWS CLI as shown below:
{{%expand "AWS CLI" %}}
```
AWSREGION=`aws configure get region`

# List the automated backups for the instance

aws rds describe-db-instance-automated-backups \
	--db-instance-identifier rdspg-fcj-labs \
	--region $AWSREGION

# List the snapshots for the instance

aws rds describe-db-snapshots \
	--db-instance-identifier rdspg-fcj-labs \
	--region $AWSREGION

# Check the Latest Restorable Time (LRT) of the instance

aws rds describe-db-instances \
	--db-instance-identifier rdspg-fcj-labs \
	--query 'DBInstances[].LatestRestorableTime' \
	--region $AWSREGION \
	--output text


```

{{% /expand%}}
