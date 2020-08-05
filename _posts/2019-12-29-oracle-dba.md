---
title: Oracle Database Administration Topics
date: 2019-12-29 00:00:00 +0000
description: Useful points related to Oracle Database Administration
img: # Add image post (optional)
classes: wide
tags: [Oracle, Database, DBA] # add tag
---
# Useful points related to Oracle Database Administration

1. How to take export back of oracle database schema?
Schema can be exported using expdp command.
```sql
expdp system/manager@orcl schemas=SCOTT directory=DATA_PUMP_DIR dumpfile=SCOTT.dmp logfile=expdpSCOTT.log
```
For detailed step, Please refer [export backup](https://github.com/rajnathsah/Oracle-Scripts-and-Notes/blob/master/dbascript/backup.md).

2. How to split export dump file size while taking export?  
By defining filesize parameter and multiple dumpfile name. Dump file name can be multiple filenames seperated by comma or it can have something like test_dump%u.dmp to auto-gererate file name. In case you are providing file name manually and export is exceeding it, then export utility prompts at command prompt for the next file name.
```sql
expdp system/manager@orcl schemas=SCOTT directory=DATA_PUMP_DIR dumpfile=test_dump%u.dmp filesize=20m logfile=expdpSCOTT.log
```

3. How to take database backup using RMAN?
Login to rman and run backup command.
```sql
$rman
RMAN> CONNECT TARGET /
RMAN> BACKUP AS BACKUPSET DATABASE
```
For more detailed step, Please refer [rman backup](https://github.com/rajnathsah/Oracle-Scripts-and-Notes/blob/master/dbascript/rman_backup_recovery.md)

4. How to restore database from rman backup?
Run restore command from rman prompt
```sql
RMAN> RESTORE DATABASE;
```
For more detailed step, Please refer [rman recovery](https://github.com/rajnathsah/Oracle-Scripts-and-Notes/blob/master/dbascript/rman_backup_recovery.md#rman-recovery)

5. What is Oracle Virtual Private Database?
Oracle Virtual Private Database (VPD) enables you to create security policies to control database access at the row and column level. Essentially, Oracle Virtual Private Database adds a dynamic WHERE clause to a SQL statement that is issued against the table, view, or synonym to which an Oracle Virtual Private Database security policy was applied.  



Happy learning.
