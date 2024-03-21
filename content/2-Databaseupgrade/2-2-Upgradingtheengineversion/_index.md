---
title : "Upgrading the engine version"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

Minor version upgrades include only changes that are backward-compatible with existing applications.

{{% notice warning %}}
If your PostgreSQL DB instance is using Read Replicas, you must upgrade all of the read replicas before upgrading the source instance. You could follow the same instructions below, but apply them first to read replicas.
{{% /notice %}}

If your DB instance is in a Multi-AZ deployment, both the writer and standby replicas are upgraded. Your DB instance might not be available until the upgrade is complete.

Let's upgrade our database instance now.

1. In the navigation pane, choose **Databases**, and then choose the **DB instance** that you want to upgrade.

2. Choose **Modify**. The **Modify DB Instance** page appears.
    ![upgrade_engine](/images/2/2-2/1.png)

3. For **DB engine version**, choose the new version.
    ![upgrade_engine](/images/2/2-2/2.png)

4. Choose **Continue** and check the summary of modifications.
    ![upgrade_engine](/images/2/2-2/3.png)

5. To apply the changes immediately, choose **Apply immediately**. Choosing this option can cause an outage in some cases.

6. On the confirmation page, review your changes. If they are correct, choose **Modify DB Instance** to save your changes.
    ![upgrade_engine](/images/2/2-2/4.png)

You can see your instance being upgraded by going back to the RDS instances page.
    ![upgrade_engine](/images/2/2-2/5.png)

#### (OPTIONAL) AWS CLI
Alternatively you can upgrade the instance using the **AWS CLI** as shown below:
    {{%expand "AWS CLI" %}}
    The following command upgrades the instance to version 15.4.
    ```
    aws rds modify-db-instance --db-instance-identifier <your database name> --engine-version 15.4 --apply-immediately --region <your region>

    ```
    {{% /expand%}}
