---
layout: post
title: Oracle Database Administration Topics
date: 2019-11-01 00:00:00 +0000
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

Happy learning.
