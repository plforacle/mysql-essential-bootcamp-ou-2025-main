# MySQL Monitoring

## Introduction

MySQL provides several tools and features for monitoring and optimizing database performance, and the Slow Query Log, Performance Schema, and SYS Schema are essential components for database administrators and developers to gain insights into MySQL's performance. By using the Slow Query Log, Performance Schema, and SYS Schema together, you can effectively monitor and optimize MySQL database performance. These tools provide valuable information about slow queries, resource usage, and overall database health, helping you to identify and address performance bottlenecks.

In this lab you will work with all three tools

Estimated Lab Time: 40 minutes




### Objectives

In this lab, you will:

- Query Slow Query Log
- Query Performance Schema
- Query SYS Schema

 
## Task 1: Using Slow Query Log to monitor and analyze slow queries in MySQL

The Slow Query Log is a feature in MySQL that allows you to log queries that take longer than a specified threshold to execute. This is particularly useful for identifying and optimizing slow-running queries that may impact the overall performance of your database. The following steps help identify performance bottlenecks in your database by recording queries that take longer than a specified threshold to execute.

1. Display MySQL configuration variables related to slow query logging.

    ```bash
    <copy>show variables like '%slow%';</copy>
    ```

2. Enable the slow query log globally.

    ```bash
    <copy>SET GLOBAL slow_query_log='ON';</copy>
    ```

3. Open the slow query log file for viewing/editing in the vi editor.

    ```bash
    <copy>sudo vi /var/lib/mysql/oms1-slow.log</copy>
    ```

4. Analyze the slow query log file using mysqldumpslow, which summarizes slow queries, sorts them by execution time, and identifies patterns.

    ```bash
    <copy>sudo mysqldumpslow /var/lib/mysql/oms1-slow.log</copy>
    ```

## Task 2: Using MySQL Performance Schema for database monitoring and query analysis

The Performance Schema in MySQL is a powerful tool for monitoring and analyzing the internal execution of SQL statements and their impact on the database system. It provides detailed information about various aspects of MySQL's performance. It captures information about executed statements, table IO, memory usage, locking, and more. The following steps help identify performance bottlenecks, monitor query execution times, and analyze database operations.

1. Check if Performance Schema is enabled

    ```bash
    <copy>SHOW VARIABLES LIKE 'performance_schema';</copy>
    ```

2. Switch to the performance_schema database

    ```bash
    <copy>use performance_schema;</copy>
    ```

3. List all tables in the performance_schema

    ```bash
    <copy>show tables;</copy>
    ```

4. Show configuration of performance data collection components

    ```bash
    <copy>SELECT * FROM performance_schema.setup_consumers</copy>
    ```

5. Retrieve all user data (likely from a different schema)

    ```bash
    <copy>select * from users;</copy>
    ```

6. Query for specific statements containing 'janis' in a database:

    ```bash
    <copy>select * from events_statements_summary_by_digest where schema_name = 'mydatabase' and query_sample_text like '%janis%'\G;</copy>
    ```

7. Query analyzing statements with detailed performance metrics for queries containing 't1':

    ```bash
    <copy>SELECT
    event_id AS "Event Id",
    TRUNCATE(timer_wait / 10000000000, 3) AS "Query Time Millis",
    TRUNCATE(lock_time / 10000000000, 3) AS "Lock Time Millis",
    rows_sent AS "Result Set Size",
    rows_examined AS "Scanned Row Count",
    created_tmp_tables AS "Temp Table Count",
    select_scan AS "Table Scan Count",
    sort_scan AS "Table Sort Count",
    sort_rows AS "Sorted Row Count",
    Sql_text FROM  performance_schema.events_statements_history Where sql_text like '%t1%';</copy>
    ```

8. Query for examining execution stages of a specific query (with event_id = 9):

    ```bash
    <copy>SELECT 
    event_name AS "Stage Name", TRUNCATE(timer_wait / 10000000000, 3) AS "Stage Time Millis" FROM performance_schema.events_stages_history_long WHERE nesting_event_id = 9 ORDER BY timer_start</copy>
    ```

## Task 3: Using MySQL's Sys Schema to provide simplified views for performance analysis

The SYS Schema is a collection of views, procedures, and functions designed to simplify the process of monitoring and understanding MySQL performance. SYS Schema provides an easy-to-use interface for querying performance data, making it more accessible to DBAs and developers. The following steps help identify performance bottlenecks, track database usage patterns, and prioritize optimization efforts.

1. Switch to the sys schema database

    ```bash
    <copy>use sys;</copy>
    ```

2. First query identifies queries with full table scans containing **t1**. This helps find inefficient queries that might need optimization.

    ```bash
    <copy>select query, db, exec_count, total_latency, rows_sent, rows_examined, rows_sent_avg, rows_examined_avg from statements_with_full_table_scans where query like '%t1%';</copy>
    ```

3. Second query retrieves comprehensive table statistics. This provides detailed metrics on how tables are being used and their performance characteristics.:

    ```bash
    <copy>select table_schema, table_name, total_latency, rows_fetched, fetch_latency, rows_inserted, insert_latency, rows_updated, update_latency, rows_deleted, delete_latency, io_read_requests, io_read, io_read_latency from schema_table_statistics;</copy>
    ```

4. Third query is similar but omits the delete and I/O metrics:

    ```bash
    <copy>select table_schema, table_name, total_latency, rows_fetched, fetch_latency, rows_inserted, insert_latency, rows_updated, update_latency from schema_table_statistics;</copy>
    ```

## Task 4: Oracle Enterprise Manager for Monitoring MySQL Live Demo

![MYSQLEE](images/live-demo.png "live demo")


## Acknowledgements 

- **Author** - Marco Carlessi, Principal Sales Consultant
- **Contributors** -  Perside Foster, MySQL Solution Engineering, Selena Sánchez, MySQL Solutions Engineer
- **Last Updated By/Date** - Perside Foster, MySQL Solution Engineering, March 2025
