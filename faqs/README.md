<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css">


# Frequently ask questions
<span style="font-size:12px;">4 minutes read time | For Development Team</span>

## How to backup MSSQL cloud database?

In AWS, specifically when using Amazon RDS (Relational Database Service), backups are typically managed automatically. However, you still have the option to handle backups manually, which is necessary in certain industries where backups need to be retained for periods longer than the default 35-day maximum of automated backups. 

For instance, the customers who are using the Accounting & Final Accounts Module often require datasets from specific dates, such as Financial Year End or Month End, for audit purposes. This requirement is not related to **disaster recovery**, and restoring the production database to a specific date is not possible. Therefore, in such scenarios, manual backups are essential.

As you can see in the image below from the Amazon User Guide (docs.aws.amazon.com/AmazonRDS/latest/UserGuide), MSSQL database backup files are stored in S3 buckets for future reference.

<div style="background-color:#D9EDF7; border-left:0px solid #31708F; padding:0.5em; margin-bottom:1em; border-radius: 2px;">
  <i class="fas fa-info-circle" style="colour : #3d95ba"></i> <strong>NOTE</strong>
  <p>Kindly note that there is a <b>cost</b> for S3 storage.</p>
</div>

![awsBackupImage](/images/aws_bacups.png)

***<span style="font-size:10px;">Image by docs.aws.amazon.com/AmazonRDS/latest/UserGuide</span>***

In order to backup MSSQL backups files (.bak) to S3 bucket, please follow the steps below.

* **Step 01** :  Login to Microsoft SQL Server Management Studio

![SQLServerMngStd](/images/SQLServerMngStd.png)

<div style="background-color:#D9EDF7; border-left:0px solid #31708F; padding:0.5em; margin-bottom:1em; border-radius: 2px;">
  <i class="fas fa-info-circle" style="colour : #3d95ba"></i> <strong>NOTE</strong>
  <p>Kindly refer <a href="https://www.1password.com/"><b>1Password</b></a> account for server names and login credentials.</p>
</div>


* **Step 02** :  Select the database server and start a new query window
* **Step 03** :  Execute the command below

## Exporting SQL Server databases using native backup

```SQL
exec msdb.dbo.rds_backup_database
	@source_db_name='database_name',
	@s3_arn_to_backup_to='arn:aws:s3:::bucket_name/file_name.extension',
	[@kms_master_key_arn='arn:aws:kms:region:account-id:key/key-id'],	
	[@overwrite_s3_backup_file=0|1],
	[@type='DIFFERENTIAL|FULL'],
	[@number_of_files=n];
```

#### Example
``` SQL
    exec msdb.dbo.rds_backup_database 
    @source_db_name='prod_db_customer01', 
    @s3_arn_to_backup_to='arn:aws:s3:::compSingapore/prod_db_customer01_2024_Apr_24.bak',
    @overwrite_S3_backup_file=1;
```

While exporting, you can check the status using this command
``` SQL
    exec msdb.dbo.rds_task_status
```

## Importing SQL Server databases backup file using native backup
In order to import MSSQL backups files (.bak) from S3 bucket to new database, please follow the command given below.

#### Example

``` SQL
    exec msdb.dbo.rds_restore_database
    @restore_db_name='prod_db_customer01_audit',
    @s3_arn_to_restore_from='arn:aws:s3:::compSingapore/prod_db_customer01_2024_Apr_24.bak'
```

While importing, you can check the status using this command
``` SQL
    exec msdb.dbo.rds_task_status
```