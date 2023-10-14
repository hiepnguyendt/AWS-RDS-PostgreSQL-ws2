---
title : "Scalability"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

**Scalability** is the ability of a system to handle increasing amounts of data and traffic without impacting performance. Amazon RDS PostgreSQL offers a variety of scalability options, including:

- Vertical scaling: Increasing the size of your DB instance class can provide more CPU, memory, and I/O capacity.
- Horizontal scaling: Adding read replicas to your DB instance can distribute read traffic across multiple instances and improve performance.
- Aurora: Amazon RDS Aurora is a fully managed, MySQL- and PostgreSQL-compatible relational database that is built for the cloud. Aurora offers high scalability and performance, and it is also highly available.

**In this lab**, you will add a read replica instance to your configuration to provide additional read scalability to your application. And simulate read-replica failover scenario.

#### This lab contains following tasks:
1. [Create Read-replica to provide read scalability](5-1-create/)
2. [Promote Read Replica into standalone instance](5-2-promote/)
3. [Perform vertical scaling](5-3-perform/)
4. [Migrating to a Multi-AZ DB cluster using an inbound read replica](5-4-migrating/)
5. [Multi-AZ DB Cluster outbound read replica](5-5-multiazdbcluster/)
