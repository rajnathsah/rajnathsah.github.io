---
layout: post
title: Oracle Pl/Sql concepts and Q&A
date: 2019-11-03 00:00:00 +0000
description: Oracle Pl/Sql concepts and question and answer for interview preperation
img: # Add image post (optional)
tags: [Oracle, SQL, PL/SQL]
---
# Oracle Pl/Sql concepts and Q&A

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
    * SQL parsing involves separating the pieces of a SQL statement into a data structure that other routines can process. During the parsing database performs following checks.  
        * Syntax check
        * Semantic check
        * Shared pool check
