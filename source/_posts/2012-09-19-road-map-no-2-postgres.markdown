---
layout: post
title: "road map no. 2 - postgres"
date: 2012-09-19 17:39
comments: true
categories: 
---

Straight from Thoughtbot's map:

Read these sections of the [Postgres manual](http://www.postgresql.org/docs/9.0/static/index.html)


####Basics
- ~~ 2.5. Querying a Table ~~
- ~~ 2.6. Joins Between Tables ~~
- ~~ 2.7. Aggregate Functions ~~
- ~~ 3.5. Window Functions ~~

####SQL Syntax  
- ~~4.1. Lexical Structure~~
- ~~4.2. Value Expressions~~

####Data Definition  
- 5.1. Table Basics
- 5.2. Default Values
- 5.3. Constraints
- 5.5. Modifying Tables

####Data Manipulation  
- 6.1. Inserting Data
- 6.2. Updating Data
- 6.3. Deleting Data

####Queries  
- 7.1. overview
- 7.2. table expressions
- 7.3. select lists
- 7.4. combining queries
- 7.5. sorting rows
- 7.6. limit and offset
- 7.7. values lists
- 7.8. WITH Queries (Common Table Expressions)

####Functions and Operators  
- 9.1. Logical Operators
- 9.2. Comparison Operators
- 9.7.1. LIKE
- 9.20.Subquery Expressions

####Indexes  
- 11.1. Introduction
- 11.5. Combining Multiple Indexes

I. SQL Commands  
{% codeblock sql-commands lang:sql %}

ALTER TABLE
CREATE INDEX [ CONCURRENTLY ]
CREATE TABLE
DELETE
INSERT
SELECT
UPDATE

{% endcodeblock %}

Read the following sections of the SQL Cookbook:  

Appendix A: Window Function Refresher
Any recipes that interest you