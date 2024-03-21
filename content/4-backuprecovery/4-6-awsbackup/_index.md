---
title : "AWS Backup"
date :  "`r Sys.Date()`" 
weight : 6
chapter : false
pre : " <b> 4.6. </b> "
---

[AWS Backup](https://aws.amazon.com/backup/)  is a fully managed backup service that makes it easy to centralize and automate the backing up of data across AWS services. With AWS Backup, you can create backup policies called **backup plans**. You can use these plans to define your backup requirements, such as how frequently to back up your data and how long to retain those backups.

**AWS Backup** lets you apply **backup plans** to your AWS resources by **simply tagging them**. AWS Backup will automatically backs up your AWS resources according to the backup plan that you defined.

{{% notice note %}}
*RDS/PostgreSQL will automatically backup your database and retain those backups for the length of your retention period, up to 35 days. Backups preformed via **AWS Backup** are considered manual snapshots, and will persist until deleted.*
{{% /notice %}}

#### Creating a Service Role for AWS Backup
In order for **AWS Backup** to preform operations on your behalf we need to assign it a service role.
1. From the [IAM Console](https://console.aws.amazon.com/iamv2/home#/roles)  select **Create role**
    ![aws backups](/images/4/4-6/1.png)

2. Select **AWS Service** for the trusted entity type and use the use cases dropdown to find and select **AWS Backup**, select the **AWS Backup** radio button, then click **Next**. 
![aws backups](/images/4/4-6/2.png)

3. In the add permissions step, use the filter by entering **AWSBackupServiceRole** and select the checkboxes for: **AWSBackupServiceRolePolicyForBackup** and **AWSBackupServiceRolePolicyForRestore**, then click **Next**.
![aws backups](/images/4/4-6/3.png)

4. Give the role a name, ``rdspg-AWSBackupServiceRole``, review the details then click **Create Role**.
![aws backups](/images/4/4-6/4.png)
![aws backups](/images/4/4-6/5.png)
![aws backups](/images/4/4-6/6.png)
![aws backups](/images/4/4-6/7.png)

#### On-Demand Backup from AWS Backup

Begin in the [AWS Backup Console](https://console.aws.amazon.com/backup/home) . 
1. First create a **Backup Vault**, which is a logical container used to organize your backups. Click on **Backup Vaults** from the left-hand menu, then select **Create Backup vault**.
![aws backups](/images/4/4-6/8.png)

2. Give the vault a name, ``rdspg-backup-vault`` and click **Create Backup vault**.
![aws backups](/images/4/4-6/9.png)

3. With your vault created you can now create an on-demand backup. Choose **Protected resources** from the left-hand menu, then click **Create on-demand backup**.
![aws backups](/images/4/4-6/10.png)

4. Complete the dialog by selecting RDS and your resource type then choosing the **rdspg-fcj-labs** database. Select the **backup vault** you just created. Select choose an **IAM role**, and select your **rdspg-AWSBackupServiceRole** from the dropdown and finally hit **Create on-demand backup**.
![aws backups](/images/4/4-6/11.png)

5. You will see the backup in your backup jobs list. This is the same as the manual snapshot, but your backup is organized into a backup vault.
![aws backups](/images/4/4-6/12.png)


#### Setup a Backup Plan in AWS Backup

{{% notice note %}}
*Selection of resources from a backup plan can be done using either resource tags or direct references.*
{{% /notice %}}

Now setup a backup plan using resource tags. Using **AWS Backup** this way will ensure that newly created resources that are properly tagged with be backed up via **AWS Backup**

1. Begin by adding resource tags to your database from the [RDS Console](https://console.aws.amazon.com/rds/home#databases:) , click the **rdspg-fcj-labs** instance, then select the tags tab and choose **Add**.

![aws backups](/images/4/4-6/13.png)

2. Add ``Environment`` for ``production`` and ``ResourceType`` for ``rdspg-fcj`` tag to your **rdspg-fcj-labs** database.

3. From the [AWS Backup Console](https://console.aws.amazon.com/backup/home)  select **Backup plans** from the left-hand menu, then choose **Create backup plan**.
![aws backups](/images/4/4-6/14.png)

4. Select **Build new plan**, enter Backup plan name ``rdspg-backup-plan``, enter Backup rule name ``DailyBackups``, select Backup vault **rdspg-backup-vault**, leaving everything else at the default, then click **Create plan**.
![aws backups](/images/4/4-6/15.png)
![aws backups](/images/4/4-6/16.png)

5. Complete the dialog by entering the resource assignment name, ``rdspg-bp-resource-selection``, choose **IAM role** and select the IAM role created earlier **rdspg-AWSBackupServiceRole**. 
![aws backups](/images/4/4-6/17.png)
6. Finally add the **Environment** and **ResoureType** tags as shown in the screengrab.
![aws backups](/images/4/4-6/18.png)

**Congratulations!** Now that you have created a backup plan based on tags, any databases you create in future with these tags will be automatically backed up with this plan.



