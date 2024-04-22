#  COMPULIN ERP
<span style="font-size:12px;">1 minutes read time | For Development Team</span>

# Frequently ask questions

## How to backup an AWS RDS-MSSQL database manually ?

In order to backup MSSQL dump files to S3 bucket, you can use below command

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