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
  * Semantic check
  * Shared pool check
