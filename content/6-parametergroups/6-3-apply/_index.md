---
title : "Apply Custom Parameter Group"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 6.3 </b> "
---


We are now going to apply this custom parameter group to our primary and replica instances.

1. Click on the **Databases** menu on the left and select the radio button next to **rdspg-fcj-labs** and click on **Modify** at the top.
2. Scroll down to **Additional Configuration**->**Database options** and choose the custom parameter group that you previously created in your account (e.g. custom-pg) as shown below.

![modify](/images/6/3/1.png)

3. Scroll down to the bottom and click on **Continue**.
4. In the next section, choose **Apply Immediately** as shown below.

![modify](/images/6/3/2.png)

5. Select the radio button next to the DB identifier **rdspg-fcj-labs** as shown below and verify that the **Status** of the database is **Modifying**.

![modify](/images/6/3/3.png)

6. Certain parameter changes require a reboot of the RDS instance, hence you can verify if a reboot is required by selecting the primary instance **rdspg-fcj-labs** and under the **Configuration** tab, on the bottom left against the modified parameter group **(custom-pg)** you will see a **Pending reboot** written indicating a need for the instance to be rebooted in order for the changes to be applied. ( *The same can be verified for the replica instance too.* )

![modify](/images/6/3/4.png)

7. So, you will have to reboot instances for the Parameter group to become active  ( **In-sync** ).
8. By selecting the database **rdspg-fcj-labs**, and by going under the **Configuration** tab, you can now see after rebooting the instance, the parameter group dispalys to be **In Sync**.

![modify](/images/6/3/5.png)

#### (OPTIONAL) AWS CLI

Alternatively you can apply the custom parameter group using the AWS CLI as shown below:

{{%expand "Code" %}}
The following command applies the custom parameter group.

```

AWSREGION=`aws configure get region`

# Modify the instance and change the parameter group

aws rds modify-db-instance \
	--db-instance-identifier rdspg-fcj-labs \
	--db-parameter-group-name custom-pg \
	--apply-immediately \
	--region $AWSREGION

# Reboot the instance 

aws rds reboot-db-instance \
	--db-instance-identifier rdspg-fcj-labs \
	--region $AWSREGION

```
{{% /expand%}}


You can optionally verify the parameter changes from psql command line. To do so, connect to the instance :

```
psql -h rdspg-fcj-labs.cssuddr073hp.us-east-1.rds.amazonaws.com -U masteruser pglab

```

Then run the following commands

```
show log_connections;
show  log_min_duration_statement;

```

The ouput would look like below: 

![modify](/images/6/3/6.png)


**Congratulations**, you've made it to the end of the Parameter Groups lab.



