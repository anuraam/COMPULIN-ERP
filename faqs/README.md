<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css">


# Frequently ask questions
<span style="font-size:12px;">1 minutes read time | For Development Team</span>

## How to backup MSSQL database manually ( in AWS RDS - Amazon Relaional Data Services)?

![awsBackupImage](/images/aws_bacups.png)



***<span style="font-size:10px;">Image by docs.aws.amazon.com/AmazonRDS/latest/UserGuide</span>***


In order to backup MSSQL backups files (.bak) to S3 bucket, please follow below steps.

* Step 01 :  Login to Microsoft SQL Server Management Studio

![SQLServerMngStd](/images/SQLServerMngStd.png)


<div style="background-color:#D9EDF7; border-left:6px solid #31708F; padding:0.5em; margin-bottom:1em;">
  <i class="fas fa-info-circle" style="colour : #3d95ba"></i> <strong>NOTE</strong>
  <p>Kindly refer 1Password account for server names and login credentials.</p>
</div>


* Step 02 :  Select the database server and start a new query windows
* Step 03 :  
#### Exporting SQL Server databases using native backup

```SQL
exec msdb.dbo.rds_backup_database
	@source_db_name='database_name',
	@s3_arn_to_backup_to='arn:aws:s3:::bucket_name/file_name.extension',
	[@kms_master_key_arn='arn:aws:kms:region:account-id:key/key-id'],	
	[@overwrite_s3_backup_file=0|1],
	[@type='DIFFERENTIAL|FULL'],
	[@number_of_files=n];
```

### Example
``` SQL
    exec msdb.dbo.rds_backup_database 
    @source_db_name='prod_db_customer01', 
    @s3_arn_to_backup_to='arn:aws:s3:::Singapore/prod_db_customer01_2024_Apr_24.bak',
    @overwrite_S3_backup_file=1;
```

While exporting, you can check the status using this command
``` SQL
    exec msdb.dbo.rds_task_status
```

#### Importing SQL Server databases backup file using native backup
### Example

``` SQL
    exec msdb.dbo.rds_restore_database
    @restore_db_name='Inventory_DEMO',
    @s3_arn_to_restore_from='arn:aws:s3:::compulindbsingapre/Inventory_JC30JAN2020.bak'
```
