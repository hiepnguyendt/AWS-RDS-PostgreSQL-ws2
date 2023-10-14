---
title : "Modify Custom Parameter Group"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 6.2. </b> "
---


To modify a parameter group in Amazon RDS, you can use the AWS Console, the AWS CLI, or the AWS SDK.

Using the AWS Console:

1. Go to the Amazon RDS [console](https://console.aws.amazon.com/rds/home#databases).
2. In the navigation pane, choose **Parameter groups**.
3. Choose the parameter group that you want to modify.
4. Under **Actions**, choose **Edit**.

![modify](/images/6/2/1.png)

5. In the filter parameters search box, look for ``log_connections`` parameter.

![modify](/images/6/2/2.png)

6. Click on the drop down in the **Values** column (if needed you can check the box to the left to activate the Values drop down).

![modify](/images/6/2/3.png)

7. Change the value to 1 and click **Save changes**.
8. Now clear the search box and look for another parameter by entering ``log_min_duration_statement``.

![modify](/images/6/2/4.png)

9. In the **Values** box, enter ``4000``.

![modify](/images/6/2/5.png)

10. Click **Save changes**. 

{{% notice note %}}
Note if you get an error message like “ Error saving: This parameter group cannot be modified because it is currently being applied to DB Instance” please wait a few minutes and retry your save action.
{{% /notice %}}

11. Click the checkbox against the new parameter group and go to Parameter group **actions** drop down (on top right), choose **Compare**.

![modify](/images/6/2/6.png)

12. Verify the changes from the default parameter group and then click **Close**.

![modify](/images/6/2/7.png)

#### (OPTIONAL) AWS CLI

Alternatively you can modify a custom parameter group using the AWS CLI as shown below:

{{%expand "Code" %}}
The following command modifies a custom parameter group:

```

AWSREGION=`aws configure get region`

aws rds modify-db-parameter-group \
	--db-parameter-group-name custom-pg \
	--parameters "ParameterName='log_connections',ParameterValue=1,ApplyMethod=immediate"    "ParameterName='log_min_duration_statement',ParameterValue=4000,ApplyMethod=immediate" \
	--region $AWSREGION
```
{{% /expand%}}