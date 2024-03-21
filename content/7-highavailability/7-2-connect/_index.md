---
title : "Connect to the Multi-AZ endpoint"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 7.2. </b> "
---



**RDS PostgreSQ**L does provide customers an option to simulate the AZ failure and High Availability by offering the option to reboot the Postgres Instance with the failover option. This option will initiate AZ level failover for the Instance, the instance on the secondary AZ will become primary, and the instance on the primary will become the new secondary.


1. In the terminal window EC2 instance, enter and run these commands to showcase connection to the database at 10-second intervals:
```
while true;
do
psql -h rdspg-fcj-labs.cssuddr073hp.us-east-1.rds.amazonaws.com -U masteruser pglab; 
echo -e "\n\n"
sleep 10
done

```

Observe the output below. This output shows you the current IP address of the RDS PostgreSQL primary instance. In the next task, we will cause a failover and observe the change in the IP address as the primary instance changes. For now, leave this window open and let the command loop run. Proceed to the next task.
![HA](/images/7/2/1.png)

#### Configure Event subscription for Failover event

To be notified about Failover events (or other ones) we create RDS Event subscription for this lab.

1. Open the Amazon RDS [console](https://console.aws.amazon.com/rds/home#event-subscriptions:)  and choose **Event Subscriptions** in the left pane.

2. Click **Create event subscription**.	
![HA](/images/7/2/2.png)
3. We use an email subscription in this example - to get notifications over the email you provide:	
![HA](/images/7/2/3.png)

And choose **rdspg-fcj-labs** instance from the list or use the All instances option, and select the **Failover event** type:	
![HA](/images/7/2/4.png)

{{% notice note %}}
Before you can start receiving the event notifications via email you will need to verify your email address. You should receive a verificaiton email shortly after creating the subscription.
![HA](/images/7/2/5.png)
{{% /notice %}}


#### (OPTIONAL) AWS CLI

Alternatively you can create an Event Subscription using the AWS CLI as shown below:

{{%expand "Code" %}}
The following code snippet creates an SNS Topic and an RDS Event Subscription with the same.

```

AWSREGION=`aws configure get region`

# Create an SNS Topic for the Event Subscription

SNSTOPICARN=$(aws sns create-topic \
    --name rdspg-fcj-email \
    --region $AWSREGION \
    --output text)

# Subscribe the given E-mail ID to the SNS Topic

aws sns subscribe \
	--topic-arn $SNSTOPICARN \
	--protocol email \
	--no-return-subscription-arn \
	--notification-endpoint fcj@gmail.com \
	--region $AWSREGION

# Create the RDS Event Subscription

aws rds create-event-subscription \
	--subscription-name subscr-rdspg-fcj-labs \
	--sns-topic-arn $SNSTOPICARN \
	--source-type db-instance \
	--event-categories failover \
	--source-ids rdspg-fcj-labs \
	--region $AWSREGION \
	--enabled

```
{{% /expand%}}
