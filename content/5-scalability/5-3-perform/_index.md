---
title : "Perform vertical scaling"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

1. Open the [Amazon RDS console](https://console.aws.amazon.com/rds/home).

2. Choose the **rdspg-fcj-labs** instance and click **Modify**.

3. On the **Modify DB Instance** page - choose the appropriate **DB instance class or DB instance size:**. Then click **Continue**

4. On **Summary of modifications** notice the changed istance type and choose **Apply immediately** to apply the changes and click on **Modify DB instance**.

5. The instance will start the DB instance class upgrade process, that could take 10-15 minutes, after which you should see in the RDS Console the updated instance class:

#### (OPTIONAL AWS CLI)
Alternatively you can scale up the instance using the AWS CLI as shown below:
{{%expand "Code" %}}
The following command modifies the instance class of the instance.

```
AWSREGION=`aws configure get region`

aws rds modify-db-instance \
	--db-instance-identifier rds-pg-labs \
	--db-instance-class db.t3.large \
	--apply-immediately \
	--region $AWSREGION



```
{{% /expand%}}
