---
title : "Create some DB Activity"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

#### Create some DB Activity
- Using **MobaXterm** to connect to your app server which you created at **workshop 1**

- Then run the following command to generate some activity on your RDS instance.

    ```
    pgbench -i --fillfactor=90 --scale=500 --host=rdspg-fcj-labs.cssuddr073hp.us-east-1.rds.amazonaws.com --username masteruser pglab
    ```
    *This will create a new database called pglab on the PostgreSQL server at rdspg-fcj-labs.cssuddr073hp.us-east-1.rds.amazonaws.com and populate it with 500 times the size of the default test data set. The test data will be inserted using a fill factor of 90%, which means that each page of the database will be filled to 90% capacity before moving on to the next page.*

- Once the **pgbench** command begins you can move on to the next session.

    ![genate_activities](/images/3/3-2/1.png)