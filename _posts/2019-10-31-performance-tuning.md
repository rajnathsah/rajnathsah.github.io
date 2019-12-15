---
layout: post
title: "Performance Tuning!"
date: 2019-10-31 00:00:00 +0000
description: Oracle sql and pl/sql performance tuning tips and techniques. # Add post description (optional)
img:  # Add image post (optional)
tags: [Performance Tuning, Query Tuning]
---
# Introduction
* Performance tuning is a set of approach to optimize the database operations.
* Tuning is done at many level, depending upon the nature of issue.
* Most common tuning issues are because of poorly written codes and in case of oracle database it is sql or pl/sql.
* Other factors could be database design or configuration or management.

# Start?
Performance tuning can be done after indetifying the area which has issues. Oracle provides multiple tools to figure out the problematic areas.  
* Explain Plan
* Tkprof
* AWR Report
* ASH Report
* ADDM Report

Above tools helps in identifying the area which has issues. It is good to have some idea about the tools. Once we have the area which is causing issue, then next comes the question of what to do get the required timing.  

# What next?
* Understanding of requirement is first, once you know the requirement, find out the approach to get the requirement done. This will help you to have an idea for getting the problem solved. 
* Look at the code piece which is causing issues. Scan and see if it is following the good coding practice. Code which is written based on standard pratice, makes it more readable and can be easily debug.
* Look at the joins used in query, pay attention to **outer joins**.
* Look at the columns used for data filter, run the explain plan and see the table access method.
* Look at the amount of data returned by table filter and based on that verify indexes, in case indexes are not used for fetching data, find the reason. Oracle optimizer decides on the usage of indexes based on multiple factor like table statistics, database settings and amount of data fetched based on filter.
* In a complex query, look at the subquery part, if it is getting repeated multiple times then find a ways to avoid it.
* In case query usage function then look the function behaviour and logic.

For more details, Please refer the github repo [link](https://github.com/rajnathsah/PerformanceTuning_Oracle). 
