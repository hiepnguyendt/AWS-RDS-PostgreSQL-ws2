---
title : "Create Custom Parameter Group"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 6.1. </b> "
---

1. Go to the Amazon RDS [console](https://console.aws.amazon.com/rds/home#databases).
2. In the navigation pane, choose **Parameter groups**.
![pg](/images/6/1/1.png)

3. Choose **Create parameter group**.
![pg](/images/6/1/2.png)

4. In the **Parameter group family list**, select the **database engine** and **version** for your parameter group.
5. In the **Type**, select DB Parameter Group.
6. In the **Group name**, enter a name for your parameter group.
7. In the **Description**, enter a description for your parameter group.
8. Choose Create.
![pg](/images/6/1/3.png)

9. To verify, in the RDS menu, click on **Parameter Groups** either on the left hand pane or on the main RDS screen. 
![pg](/images/6/1/4.png)

#### (OPTIONAL) AWS CLI

Alternatively you can create a custom parameter group using the AWS CLI as shown below:

{{%expand "Code" %}}
The following command creates a custom parameter group:

```

AWSREGION=`aws configure get region`

aws rds create-db-parameter-group \
	--db-parameter-group-name custom-pg \
	--db-parameter-group-family postgres15 \
	--description "custom-postgres parameter group" \
	--region $AWSREGION



```
{{% /expand%}}