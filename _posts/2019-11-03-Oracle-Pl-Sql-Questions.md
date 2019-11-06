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

##### 4. What is hard parse
Based on shared pool check, if Oracle Database cannot reuse existing code, then it must build a new executable version of the application code. This operation is known as a **hard parse**, or a **library cache miss**. During the hard parse, the database accesses the library cache and data dictionary cache numerous times to check the data dictionary. When the database accesses these areas, it uses a serialization device called a **latch** on required objects so that their definition does not change. Latch contention *increases statement execution time* and decreases concurrency.  

##### 5. What is soft parse
A soft parse is any parse that is not a hard parse. If the submitted statement is the same as a reusable SQL statement in the shared pool, then Oracle Database reuses the existing code. This reuse of code is also called a **library cache hit**.  

##### 6. What is latch
A latch is an internal Oracle mechanism used to protect data structures in the SGA from simultaneous access. Atomic hardware instructions like TEST-AND-SET are used to implement latches. Latches are more restrictive than locks in that they are always exclusive. Latches are never queued, but will spin or sleep until it obtains a resource or times out. Latches are important for performance tuning.
In another word, laches are a type of locks.

##### 7. What is lock
Locks are mechanisms that prevent destructive interaction between transactions accessing the same resource. The resources can be either:
    * user objects, such as tables and rows,
    * or system objects not visible to users, such as shared data structures in memory and data dictionary rows.
Oracle Database automatically obtains and manages necessary locks when executing SQL statements. For more details on lock refer oracle docs link on [Data Concurrency and Consistency](https://docs.oracle.com/database/121/CNCPT/consist.htm).

##### 8. Difference between lock and latch
Following are the differences between these two:  
    * Latches are short term in length of operation and locks are long duration in restricting access to Oracle data structures.
    * Latches are lightweight serialization and locks are the heavy duty long running serialization mechanism.
    * Locks permit shared and concurrent access while latches allow access to only a single process at a time and prevent other processes within Oracle from accessing that process while a latch is held by the process.
    * Latches affect only data structures within the Oracle SGA, whereas locks apply to Oracle transactions.
    * Latch can be requested in only two modes: no-wait and willing-to-wait, while locks have six different request modes: null, row share, row exclusive, share row exclusive, share, or exclusive.
    * Latches are visible only to the local instance in memory as opposed to locks which maintain information within the Oracle database and are visible to all of the instances that have access to the database.
   
