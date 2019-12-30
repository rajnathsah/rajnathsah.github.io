---
layout: post
title: Oracle Database Administration Topics
date: 2019-12-29 00:00:00 +0000
description: Useful points related to Oracle Database Administration
img: # Add image post (optional)
tags: [Oracle, Database, DBA] # add tag
---
# Useful points related to Oracle Database Administration

1. How to take export back of oracle database schema?
Schema can be exported using expdp command.
```sql
expdp system/manager@orcl schemas=SCOTT directory=DATA_PUMP_DIR dumpfile=SCOTT.dmp logfile=expdpSCOTT.log
```
For detailed step, Please refer [export backup](https://github.com/rajnathsah/Oracle-Scripts-and-Notes/blob/master/dbascript/backup.md).

2. How to take database backup using RMAN?
Login to rman and run backup command.
```sql
$rman
RMAN> CONNECT TARGET /
RMAN> BACKUP AS BACKUPSET DATABASE
```
For more detailed step, Please refer [rman backup](https://github.com/rajnathsah/Oracle-Scripts-and-Notes/blob/master/dbascript/rman_backup_recovery.md)

3. How to restore database from rman backup?
Run restore command from rman prompt
```sql
RMAN> RESTORE DATABASE;
```
For more detailed step, Please refer [rman recovery](https://github.com/rajnathsah/Oracle-Scripts-and-Notes/blob/master/dbascript/rman_backup_recovery.md#rman-recovery)

Happy learning.
