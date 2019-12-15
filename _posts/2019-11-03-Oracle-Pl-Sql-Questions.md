---
layout: post
title: Oracle Sql and Pl/Sql topics as Q&A
date: 2019-11-03 00:00:00 +0000
description: Oracle Sql and Pl/Sql concepts with question and answer for interview preperation
img: # Add image post (optional)
tags: [Oracle, SQL, PL/SQL]
---
Important topics in Q&A format is based on my experience. Feel free to update me in case of any observation or you want to add anything. This post is specific to technical concepts.

##### 1. What is SQL query execution order?    
Order of SQL query processing in Oracle database.  
  * FROM 
  * CONNECT BY 
  * WHERE 
  * GROUP BY 
  * HAVING 
  * SELECT 
  * ORDER BY
  
##### 2. How does database processes sql statement?
  * SQL parsing (Includes syntax check, semantic check and shared pool check)
  * SQL optimization
  * SQL row source generation
  * Execution

##### 3. Define SQL parsing.
SQL parsing involves separating the pieces of a SQL statement into a data structure that other routines can process. During the parsing database performs following checks.  
  * Syntax check
    check each SQL statement for syntactic validity.    
  * Semantic check
    A semantic check determines whether a statement is meaningful, for example, whether the objects and columns in the statement exist.
  * Shared pool check
    Database performs a shared pool check to determine whether it can skip resource-intensive steps of statement processing.When a user submits a SQL statement, the database searches the shared SQL area to see if an existing parsed statement has the same hash value. Based on result of hash check, it follows below.
    * Hard Parse
    * Soft Parse

##### 4. What is hard parse?
Based on shared pool check, if Oracle Database cannot reuse existing code, then it must build a new executable version of the application code. This operation is known as a **hard parse**, or a **library cache miss**. During the hard parse, the database accesses the library cache and data dictionary cache numerous times to check the data dictionary. When the database accesses these areas, it uses a serialization device called a **latch** on required objects so that their definition does not change. Latch contention *increases statement execution time* and decreases concurrency.  

##### 5. What is soft parse?
A soft parse is any parse that is not a hard parse. If the submitted statement is the same as a reusable SQL statement in the shared pool, then Oracle Database reuses the existing code. This reuse of code is also called a **library cache hit**.  

##### 6. What is latch?
A latch is an internal Oracle mechanism used to protect data structures in the SGA from simultaneous access. Atomic hardware instructions like TEST-AND-SET are used to implement latches. Latches are more restrictive than locks in that they are always exclusive. Latches are never queued, but will spin or sleep until it obtains a resource or times out. Latches are important for performance tuning.
In another word, laches are a type of locks.

##### 7. What is lock?
Locks are mechanisms that prevent destructive interaction between transactions accessing the same resource. The resources can be either:
  * user objects, such as tables and rows,
  * or system objects not visible to users, such as shared data structures in memory and data dictionary rows.
Oracle Database automatically obtains and manages necessary locks when executing SQL statements. For more details on lock refer oracle docs link on [Data Concurrency and Consistency](https://docs.oracle.com/database/121/CNCPT/consist.htm).

##### 8. Difference between lock and latch?  
Following are the differences between these two:  
  * Latches are short term in length of operation and locks are long duration in restricting access to Oracle data structures.
  * Latches are lightweight serialization and locks are the heavy duty long running serialization mechanism.
  * Locks permit shared and concurrent access while latches allow access to only a single process at a time and prevent other processes within Oracle from accessing that process while a latch is held by the process.
  * Latches affect only data structures within the Oracle SGA, whereas locks apply to Oracle transactions.
  * Latch can be requested in only two modes: no-wait and willing-to-wait, while locks have six different request modes: null, row share, row exclusive, share row exclusive, share, or exclusive.
  * Latches are visible only to the local instance in memory as opposed to locks which maintain information within the Oracle database and are visible to all of the instances that have access to the database.
   
##### 9. What is cursor?
A cursor is a pointer which points towards a pre allocated memory location in the PGA. The memory location to which it points is known as context area, oracle associates every SELECT statement with a cursor to hold the query information in this context area.  
There are two types of cursors:-    
  * Implicit cursors    
  * Explicit cursors
For more details, refer [cursor](https://github.com/rajnathsah/Oracle-Scripts-and-Notes/blob/master/Notes/Cursor.md) document of my github repository.

##### 10. What is Ref Cursor?
  * A Ref cursor is basically a data type. 
  * A variable created based on such a data type is generally called a cursor variable. 
  * A cursor variable can be associated with different queries at run-time. 
  * The primary advantage of using cursor variables is their capability to pass result sets between sub programs (like stored procedures, functions, packages etc.).
  * Refer [example](https://github.com/rajnathsah/Oracle-Scripts-and-Notes/blob/master/Notes/ref_cursor.md) for more details.
  
##### 11. Difference between decode and case statement?
DECODE and CASE statements (In oracle) provides a conditional construct, of this form
```sql
if A = n1 
 then A1
else if A = n2 
 then A2
else X
```
* Oracle database prior to 8.1.6 only had DECODE function.
* CASE was introduced in Oracle 8.1.6 as a standard, more meaningful and more powerful function.
* Everything DECODE can do, CASE can. There is a lot else CASE can do though, which DECODE cannot.
* DECODE performs an equality check only. CASE is capable of other logical comparisons such as < > etc.  
For detailed comparison, please refer [Decode & Case](https://github.com/rajnathsah/Oracle-Scripts-and-Notes/blob/master/Notes/Decode%20and%20Case.md).

##### 12. What is deterministic function?
A function is called deterministic function, if function always return the same output for multiple invocation with same given input arguments. For more details with example, please refer [deterministic function](https://github.com/rajnathsah/Oracle-Scripts-and-Notes/blob/master/Notes/Deterministic%20function.md).

##### 13. What is Global temporary table?
A temporary table is a table that holds data only for the duration of a session or transaction. GTT (Global Temporary Table) is used for storing temporary data which is needed or reffered only during a transaction and session. For more details about syntax and usage, Please refer [global temporary](https://github.com/rajnathsah/Oracle-Scripts-and-Notes/blob/master/Notes/Global%20Temporary%20Table.md) document.

##### 14. How to execute sql query and procedure from shell script?
* Command to execute sql script and print value
```shell
val=`sqlplus -s <db username>/<db password>@<service name> <<EOF
set heading off
set pagesize 0
select sysdate from dual;
exit
EOF`
echo "System date - $val"
```
* Command to execute procedure and print log
```shell
val=`echo "execute  proc_hello_world;" | sqlplus <db username>/<db password>@<service name> | tail -5 | head -n1`
echo "$val"
```

##### 15. What is materialized view and how it helps in query optimization?
Materialized view in Oracle database is an object that contains the results of a query. They are local copies of data located remotely or aggregate data of queries.  
```sql
CREATE MATERIALIZED VIEW view-name
BUILD [IMMEDIATE | DEFERRED]
REFRESH [FAST | COMPLETE | FORCE ]
ON [COMMIT | DEMAND ]
[[ENABLE | DISABLE] QUERY REWRITE]
AS
SELECT ...;
```
* IMMEDIATE: View is populated immediately.
* DEFERRED: View is populated on first requested refresh.
* FAST: Fast refresh is attempted, in case materialized view logs are not present against source table in advance, fast refresh will fail.
* COMPLETE: View is truncated and re-popolated completely using associated query.
* FORCE: Fast refresh of view is attempted, if not possible then complete refresh is performed.
* COMMIT: Refresh is triggered by commiting data change on source table.
* DEMAND: Refresh is triggered by manual or scheduled request.
* QUERY REWRITE: It is a hint for optimizer if materialized view should be considered for query re-write operations.
* Populating materialized view add loads on the server involved, it should be noted while using it.
* Materialized view logs help in fast refresh but increases DML time on source table.
* Missing regular refresh increases the size of log table.
* Verify QUERY_REWRITE_INTEGRITY and QUERY_REWRITE_ENABLED parameter to confirm before using QUERY REWRITE option.

##### 16. How to process large set of data using forall without fail?
Using SAVE EXCEPTIONS option, all exception raised during forall processing can be saved. For more details with example, Please refer [bulk data processing](https://github.com/rajnathsah/PerformanceTuning_Oracle/blob/master/Bulk%20Data%20Processing.md) article.

##### 17. What is hints in oracle and how to use it?
Hints let us make decisions which are made by the oracle optimizer. Syntax to enable hints is below.  
```sql
/*+ hint(view_name.table_in_view) */
```
* Note: Hints should be used with caution in production system.

##### 18. What is index?
An index is a method of allowing faster retrieval of records. An index creates an entry for each value that appears in the indexed columns. By default, Oracle creates B-tree indexes. For more details, please refer [index](https://github.com/rajnathsah/PerformanceTuning_Oracle/blob/master/Index.md) article.

##### 19. What is partitioning?
Method to break a very large table and/or its associated indexes into smaller and manageable pieces. In another words partitioning allows tables, indexes, and index-organized tables to be subdivided into smaller pieces, enabling these database objects to be managed and accessed at a smaller level. For more details, please refer [partitioning](https://github.com/rajnathsah/PerformanceTuning_Oracle/blob/master/Partitioning.md) article.
