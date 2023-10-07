---
title : "Modify Backups"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 4.2.</b> "
---

#### Adjusting your backup window & backup retention

Next we will change the backup window to 22:30 (UTC) and set the backup retention to 3 days.

In the upper right of the database details screen choose the **Modify** button

![Modify Backups](/images/4/4-2/0.png)

On the **Mondify DB Instance** page, scroll down to the backup section which is under "**Additional Configuraiton**" section. Using the pull down menu change the backup retention period to 3 days. The maximium retention period for automated backups is 35 days. *Manual snapshots can be retained indefinetly*

Update the backup window to occur at 22:30 (UTC), so plan accordingly

![Modify Backups](/images/4/4-2/1.png)

Once you've made the appropriate changes click the continue button. Confirm the modifications on the next screen and choose the *Apply immediately* option. Click **Modify DB Instance**

![Modify Backups](/images/4/4-2/2.png)

As the changes are being applied you'll be taken back to the database instance details page. Note: the exact start time of the backup will be chosen at random within the 30 minute window.

#### (OPTIONAL) AWS CLI

Alternatively you can modify the instance's backup window using the **AWS CLI** as shown below:

{{%expand "AWS CLI" %}}

```
AWSREGION=`aws configure get region`

aws rds modify-db-instance \
	--db-instance-identifier rds-pg-labs \
	--preferred-backup-window 22:00-22:30 \
	--apply-immediately \
	--region $AWSREGION

```

{{% /expand%}}
