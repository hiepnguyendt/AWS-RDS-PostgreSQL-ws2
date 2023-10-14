---
title : "High Availability"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 7. </b> "
---


***High availability (HA) for Amazon RDS PostgreSQL can be achieved using a variety of methods, including:***

- Multi-AZ deployments: Multi-AZ deployments create a standby replica of the database in a different Availability Zone (AZ) than the primary database. If the primary database fails, the standby replica can be promoted to the primary database, minimizing downtime.
- Read replicas: Read replicas are copies of the database that can be used to offload read traffic from the primary database. This can improve the performance and scalability of the database. Read replicas can also be used to create a disaster recovery plan in case the primary database fails.
- Database clusters: Database clusters are a group of database instances that work together as a single logical unit. Clustered databases can be used to achieve high availability and scalability by distributing the workload across multiple instances.

**Multi-AZ deployments** are the most common way to achieve high availability for Amazon RDS PostgreSQL. Multi-AZ deployments are easy to set up and manage, and they provide a high level of availability.

**Read replicas** can also be used to improve the availability of Amazon RDS PostgreSQL databases. Read replicas can be used to offload read traffic from the primary database, which can improve the performance and scalability of the database. Read replicas can also be used to create a disaster recovery plan in case the primary database fails.

**Database clusters** are the most advanced option for achieving high availability and scalability for Amazon RDS PostgreSQL. Database clusters are more complex to set up and manage than multi-AZ deployments, but they can provide a higher level of availability and scalability.

**In this lab**, you will setup the high-availability Multi-AZ configuration for your RDS PostgreSQL database instance. Then you will simulate a failover event to see how it performs. Finally, you scale-up your RDS instance for additional write capacity.

#### This lab contains following tasks:

1. [Setup high availability for RDS PostgreSQL (Multi-AZ)](7-1-setup/)
2. [Connect to the Multi-AZ endpoint](7-2-connect/)
3. [Perform failover to verify high availability](7-3-perform/)


