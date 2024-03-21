---
title : "Point in Time Restore"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 4.5. </b> "
---


#### Recover your database to a point in time

Sometimes you will want to restore the database to a particular point in the past, just prior to running a data conversion script that encountered errors, or to refresh your stage environment to a state before an upgrade script was run. This is called a **Point In Time Recovery** or **PITR**.

{{% notice info %}}
*Amazon RDS for PostgreSQL allows restore to any point in time during your backup retention period. This is possible through the use of automated backups in combination with transaction logs, which are uploaded to S3 every 5 minutes.*
{{% /notice %}}

So far in this section of the lab we have used the **AWS Management Console** for our tasks. You also can administer your **RDS PostgreSQL** instance from the command line using the **AWS CLI**. To demostrate this we will use the command-line interface for this particular lab.

{{% notice info %}}
*Amazon RDS keeps track of the latest restorable time for your database. We will lookup this information using the AWS-CLI. To run these commands we will use the Cloud9 enviornment you setup in the prerequisties section. If you haven't done this, return here and complete this step.*
{{% /notice %}}

1. Using your EC2 instances, lookup the latest restore time for your database.

    ```
    aws rds describe-db-instances \
    --db-instance-identifier rdspg-fcj-labs \
    --region $AWSREGION \
    --query 'DBInstances[0].LatestRestorableTime' \
    --output text
    ```

2. Sample output below shows a latest restore time of Octorber 6, 2023 at 14:04 UTC

    ```
    2023-10-6T12:04:19Z
    ```

    ![pitr](/images/4/4-5/1.png)

3. Using the **AWS-CLI** we can use the following command to restore the database to the latest restorable time we looked up in the prior step. Be sure to update the time.

    ```
    aws rds restore-db-instance-to-point-in-time \
    --source-db-instance-identifier rdspg-fcj-labs \
    --target-db-instance-identifier rdspg-fcj-labs-restore-latest \
    --restore-time 2023-10-6T12:04:19Z
    ```

4. You will see output similar to:
{{%expand "Output" %}}
```
"DBInstance": {
        "PubliclyAccessible": true,
        "MasterUsername": "masteruser",
        "MonitoringInterval": 0,
        "LicenseModel": "postgresql-license",
        "VpcSecurityGroups": [
            {
                "Status": "active",
                "VpcSecurityGroupId": "sg-040ccaea808501fad"
            }
        ],
        "CopyTagsToSnapshot": false,
        "OptionGroupMemberships": [
            {
                "Status": "pending-apply",
                "OptionGroupName": "default:postgres-15"
            }
        ],
        "PendingModifiedValues": {},
        "Engine": "postgres",
        "MultiAZ": false,
        "DBSecurityGroups": [],
        "DBParameterGroups": [
            {
                "DBParameterGroupName": "default.postgres15",
                "ParameterApplyStatus": "in-sync"
            }
        ],
        "PerformanceInsightsEnabled": false,
        "AutoMinorVersionUpgrade": true,
        "PreferredBackupWindow": "22:30-23:00",
        "DBSubnetGroup": {
            "Subnets": [
                {
                    "SubnetStatus": "Active",
                    "SubnetIdentifier": "subnet-0b17d2e34c1123500",
                    "SubnetOutpost": {},
                    "SubnetAvailabilityZone": {
                        "Name": "us-east-1e"
                    }
                },
                {
                    "SubnetStatus": "Active",
                    "SubnetIdentifier": "subnet-0cf079d239ac29e77",
                    "SubnetOutpost": {},
                    "SubnetAvailabilityZone": {
                        "Name": "us-east-1b"
                    }
                },
                {
                    "SubnetStatus": "Active",
                    "SubnetIdentifier": "subnet-0bee38e5233fa9f09",
                    "SubnetOutpost": {},
                    "SubnetAvailabilityZone": {
                        "Name": "us-east-1f"
                    }
                },
                {
                    "SubnetStatus": "Active",
                    "SubnetIdentifier": "subnet-06327d061c2b4587b",
                    "SubnetOutpost": {},
                    "SubnetAvailabilityZone": {
                        "Name": "us-east-1a"
                    }
                },
                {
                    "SubnetStatus": "Active",
                    "SubnetIdentifier": "subnet-0cd302225e2bf84ca",
                    "SubnetOutpost": {},
                    "SubnetAvailabilityZone": {
                        "Name": "us-east-1d"
                    }
                },
                {
                    "SubnetStatus": "Active",
                    "SubnetIdentifier": "subnet-0b05e234776e32206",
                    "SubnetOutpost": {},
                    "SubnetAvailabilityZone": {
                        "Name": "us-east-1c"
                    }
                }
            ],
            "DBSubnetGroupName": "default",
            "VpcId": "vpc-05136ed012ff6cc5d",
            "DBSubnetGroupDescription": "default",
            "SubnetGroupStatus": "Complete"
        },
        "ReadReplicaDBInstanceIdentifiers": [],
        "AllocatedStorage": 100,
        "DBInstanceArn": "arn:aws:rds:us-east-1:478371912360:db:rdspg-fcj-labs-restore-latest",
        "BackupRetentionPeriod": 3,
        "DBName": "pglab",
        "PreferredMaintenanceWindow": "thu:08:05-thu:08:35",
        "DBInstanceStatus": "creating",
        "IAMDatabaseAuthenticationEnabled": false,
        "EngineVersion": "15.3",
        "MaxAllocatedStorage": 1000,
        "DeletionProtection": false,
        "DomainMemberships": [],
        "StorageType": "io1",
        "DbiResourceId": "db-YZM6TLDJKUBUM3MFXZIGZ5AGGM",
        "CACertificateIdentifier": "rds-ca-2019",
        "Iops": 1000,
        "StorageEncrypted": true,
        "AssociatedRoles": [],
        "KmsKeyId": "arn:aws:kms:us-east-1:478371912360:key/dd420e91-002c-49bc-89d8-709cecce145a",
        "DBInstanceClass": "db.t3.medium",
        "DbInstancePort": 0,
        "DBInstanceIdentifier": "rdspg-fcj-labs-restore-latest"
    }

```

{{% /expand%}}

5. Now, let's return to the RDS Console to check the restoring database. If you look at the details, note all the database specifications (e.g. DB Instance Class, Security Group) match the original database.
![pitr](/images/4/4-5/2.png)

6. Now we will use the [RDS Console](https://console.aws.amazon.com/rds/home#databases:)  to restore our database to 30 minutes prior. Select the ``rdspg-fcj-labs`` database, choose **Actions**, and select **Restore to point in time.**
![pitr](/images/4/4-5/3.png)

7. On the **Launch DB Instance** page, choose a custom and select a time 30 minutes prior. Enter a new DB instance identifier (e.g. ``rdspg-fcj-labs-earlier-restore``), leave the remaining information at default values and click on **Restore to point in time**.
![pitr](/images/4/4-5/4.png)
![pitr](/images/4/4-5/5.png)

8. As the restore begins you will be take back to the list of databases and should see your new instance being created.
![pitr](/images/4/4-5/6.png)