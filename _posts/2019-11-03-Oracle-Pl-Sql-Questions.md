---
layout: post
title: Oracle Sql and Pl/Sql concepts and Q&A
date: 2019-11-03 00:00:00 +0000
description: Oracle Sql and Pl/Sql concepts with question and answer for interview preperation
img: # Add image post (optional)
tags: [Oracle, SQL, PL/SQL]
---
# Oracle Sql and Pl/Sql concepts and Q&A
I will try to add all the important topics in Q&A format based on my experience. Feel free to update me in case of any observation or you want to add anything. This post is specific to technical concepts.

##### 1. SQL Query processing    
Order of SQL query processing in Oracle database.  
  * FROM 
  * CONNECT BY 
  * WHERE 
  * GROUP BY 
  * HAVING 
  * SELECT 
  * ORDER BY
  
##### 2. How does database processes sql statement
  * SQL parsing (Includes syntax check, semantic check and shared pool check)
  * SQL optimization
  * SQL row source generation
  * Execution

##### 3. SQL Parsing
SQL parsing involves separating the pieces of a SQL statement into a data structure that other routines can process. During the parsing database performs following checks.  
  * Syntax check
    check each SQL statement for syntactic validity.    
  * Semantic check
    A semantic check determines whether a statement is meaningful, for example, whether the objects and columns in the statement exist.
  * Shared pool check
    Database performs a shared pool check to determine whether it can skip resource-intensive steps of statement processing.When a user submits a SQL statement, the database searches the shared SQL area to see if an existing parsed statement has the same hash value. Based on result of hash check, it follows below.
    * Hard Parse
    * Soft Parse

##### 4. What is hard parsing
Based on shared pool check, if Oracle Database cannot reuse existing code, then it must build a new executable version of the application code.  
This operation is known as a hard parse, or a library cache miss.  
During the hard parse, the database accesses the library cache and data dictionary cache numerous times to check the data dictionary.   When the database accesses these areas, it uses a serialization device called a latch on required objects so that their definition does not change. Latch contention increases statement execution time and decreases concurrency.  
