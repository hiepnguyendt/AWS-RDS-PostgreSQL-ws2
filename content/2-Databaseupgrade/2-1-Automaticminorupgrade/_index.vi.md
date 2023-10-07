---
title : "Automatic minor upgrade"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

When you are creating the db instance you will find a checkbox that will enable automatic minor version upgrades like the following image.

![auto_minor](/images/2/2-1/1.png)

We can find out if the option is enabled by running the following command:

```
aws rds describe-db-instances 
       --db-instance-identifier rdspg-fcj-labs 
      --region $AWSREGION 
      --query 'DBInstances[*].AutoMinorVersionUpgrade'
```

![auto_minor](/images/2/2-1/2.png)

Or by going to the **RDS console**, clicking on the db instance and select the **Maintenance & backups** tab.

![auto_minor](/images/2/2-1/3.png)

In this case, you have not enabled **Auto minor version upgrade** in the first create database.\
Now, you can enable by click **Modify** button, then scroll down and select **Enabled Auto minor version upgrade**

![auto_minor](/images/2/2-1/4.png)

A PostgreSQL DB instance is automatically upgraded during your maintenance window if the following criteria are met:

- The DB instance has the Auto minor version upgrade option enabled.
- The DB instance is running a minor DB engine version that is less than the current automatic upgrade minor version.