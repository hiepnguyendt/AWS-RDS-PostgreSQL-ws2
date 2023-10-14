---
title : "Parameter Groups"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---


**Parameter groups** are a collection of engine configuration values that you set for your Amazon Relational Database Service (RDS) database instance. They can be used to configure a wide range of database settings, such as memory allocation, logging level, and connection pooling.

**Parameter groups** are useful for a variety of purposes, including:

- Consistency: Parameter groups can help you to ensure that all of your database instances are configured in the same way. This can help to reduce the risk of errors and make it easier to troubleshoot problems.
- Performance tuning: Parameter groups can be used to tune your database for specific workloads. For example, you can increase the memory allocation for a database that is running heavy read workloads.
- Security: Parameter groups can be used to implement security best practices. For example, you can disable certain features that are not needed for your application.

Here are some tips for using parameter groups effectively:

- Use the default parameter groups as a starting point, but modify them to meet the specific needs of your application.
- Create custom parameter groups for different types of database instances, such as production instances and development instances.
- Test any changes to parameter groups in a staging environment before deploying them to production.
- Monitor your database instances to ensure that the parameter groups are configured correctly.

#### This lab contains the following tasks
1. [Create Custom Parameter Group](6-1-create/)
2. [Modify Custom Parameter Group](6-2-modify/)
3. [Apply Custom Parameter Group](6-3-apply/)
