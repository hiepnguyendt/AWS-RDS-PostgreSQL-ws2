---
title : "Validate the upgrade process"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

After a few minutes you will see that the database already finished the upgrade process and the status will be back to **Available**.

![validate_process](/images/2/2-3/1.png)

Click on the instance, then the **Configuration** tab and you will see that the database engine version went from **15.3** to **15.4**.

![validate_process](/images/2/2-3/2.png)

Or if you rather use the **CLI** command, type in the console:

``````````
aws rds describe-db-instances --db-instance-identifier < your database name > --region < your region > --query DBInstances[*].EngineVersion

``````````

You can see **engine version** after run command above:

![validate_process](/images/2/2-3/3.png)
