---
title : "Point in Time Restore"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 4.5. </b> "
---


#### Khôi phục cơ sở dữ liệu của bạn tại một thời điểm

Đôi khi, bạn sẽ muốn khôi phục cơ sở dữ liệu về một điểm cụ thể trong quá khứ, ngay trước khi chạy tập lệnh chuyển đổi dữ liệu gặp lỗi hoặc làm mới môi trường của bạn về trạng thái trước khi chạy tập lệnh nâng cấp. Đây được gọi là một **Point In Time Recovery** hay **PITR**.

{{% notice info %}}
*Amazon RDS for PostgreSQL cho phép khôi phục về bất kỳ thời điểm nào trong thời gian lưu giữ bản sao lưu của bạn. Điều này có thể thực hiện được thông qua việc sử dụng các bản sao lưu tự động kết hợp với nhật ký giao dịch, được tải lên S3 cứ sau 5 phút.*
{{% /notice %}}

Ở phần thực hành, chúng ta sẽ sử **AWS CLI** để thực hiện các thử nghiệm
{{% notice info %}}
*Amazon RDS theo dõi thời gian khôi phục mới nhất cho cơ sở dữ liệu của bạn. Chúng ta sẽ kiểm tra thông tin này bằng AWS-CLI. Để chạy các lệnh này, chúng tôi sẽ sử dụng môi trường Cloud9 mà bạn thiết lập trong phần điều kiện tiên quyết. Nếu bạn chưa làm điều này, hãy quay lại đây và hoàn thành bước này.*
{{% /notice %}}

1. Sử dụng EC2 instance, kiểm tra thời gian khôi phục mới nhất cho cơ sở dữ liệu của bạn.

    ```
    aws rds describe-db-instances \
    --db-instance-identifier rdspg-fcj-labs \
    --region $AWSREGION \
    --query 'DBInstances[0].LatestRestorableTime' \
    --output text
    ```

2. Output mẫu bên dưới hiển thị thời gian khôi phục mới nhất là ngày 6 tháng 10 năm 2023 lúc 14:04 UTC

    ```
    2023-10-6T12:04:19Z
    ```
    ![pitr](/images/4/4-5/1.png)

3. Bằng cách sử dụng **AWS-CLI**, chúng ta có thể sử dụng lệnh sau để khôi phục cơ sở dữ liệu về thời điểm có thể khôi phục mới nhất mà chúng ta đã tra cứu ở bước trước. Hãy chắc chắn để cập nhật thời gian.

    ```
    aws rds restore-db-instance-to-point-in-time \
    --source-db-instance-identifier rdspg-fcj-labs \
    --target-db-instance-identifier rdspg-fcj-labs-restore-latest \
    --restore-time 2023-10-6T12:04:19Z
    ```

4. Bạn sẽ thấy output tương tự như:
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

5. Bây giờ, hãy quay lại RDS Console để kiểm tra cơ sở dữ liệu đang khôi phục. Nếu bạn xem chi tiết, hãy lưu ý rằng tất cả các thông số cơ sở dữ liệu (ví dụ: DB Instance Class, Security Group) đều khớp với cơ sở dữ liệu gốc.
![pitr](/images/4/4-5/2.png)

6. Bây giờ, chúng ta sẽ sử dụng [RDS Console](https://console.aws.amazon.com/rds/home#databases:) để khôi phục cơ sở dữ liệu của chúng ta về 30 phút trước đó. Chọn cơ sở dữ liệu ``rdspg-fcj-labs``, chọn **Actions** và chọn **Restore to point in time.**
![pitr](/images/4/4-5/3.png)

7. Trên trang **Launch DB Instance**, chọn một tùy chỉnh và chọn thời gian trước 30 phút. Nhập một DB instance identifier mới (ví dụ: ``rdspg-fcj-labs-earlier-restore``), để thông tin còn lại ở giá trị mặc định và nhấp vào **Restore to point in time**.
![pitr](/images/4/4-5/4.png)
![pitr](/images/4/4-5/5.png)

8. Khi quá trình khôi phục bắt đầu, bạn sẽ được đưa trở lại danh sách cơ sở dữ liệu và sẽ thấy phiên bản mới của mình được tạo.
![pitr](/images/4/4-5/6.png)