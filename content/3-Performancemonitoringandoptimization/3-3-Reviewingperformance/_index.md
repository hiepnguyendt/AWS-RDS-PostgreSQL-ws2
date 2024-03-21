---
title : "Reviewing Performance"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

Now that we have a workload running against our AWS RDS PostgreSQL database, we can take a look at the metrics and dashboards available in CloudWatch and dive deeper with Performance Insights.

1. In the [RDS Console(https://console.aws.amazon.com/rds/home#databases)] , navigate to the Database instance details page for your database. To view CloudWatch metrics, click on the **Monitoring** tab. Explore various charts.
    ![reviewing](/images/3/3-3/1.png)

2. You can click on the chart area of an individual chart to bring the chart full screen and get access to different chart customizations.
    ![reviewing](/images/3/3-3/2.png)
3. To view Enhanced monitoring metrics, click on the Monitoring dropdown on the right side of the screen and pick Enhanced monitoring and explore various charts.
    ![reviewing](/images/3/3-3/3.png)
4. Now look for the **Performance Insights** link in the RDS console. Right-click on the link and open up Performance Insights in a new browser tab.
    ![reviewing](/images/3/3-3/4.png)
5. Select your database instance and you will notice the load being generated on your RDS instance
    ![reviewing](/images/3/3-3/5.png)
    ![reviewing](/images/3/3-3/5.1.png)
6. Explore different waits in Performance Insights
    ![reviewing](/images/3/3-3/6.png)
Next we will put a heavy load on the database and revisit these dashboards.

