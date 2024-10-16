# Database Design

## Introduction
In this lab you will execute some SQL operations, practice using Joins and use Explain.

Estimated Lab Time: 20 minutes

### Objectives
In this lab, you will:
-Work with SQL 
-Practice some CRUD operations
-Work with Joins and EXPLAIN

> **Note:** 
  -Server: mysql1

## Task 1: Work with SQL

1. Connect to your <span style="color:red">mysql-advanced</span> with admin user
    ```
    <span style="color:green">shell></span> <copy>mysqlsh admin@mysql1:3307</copy>
    ```

2. Create a new table poi
     ```
    <span style="color:blue">mysql></span> <copy>use world;</copy>
    ```
     ```
    <span style="color:blue">mysql></span> <copy>SHOW TABLES;</copy>
    ```
     ```
    <span style="color:blue">mysql></span> <copy>CREATE TABLE if not exists poi (x Int, y INT, z INT);</copy>
    ```

3. We forgot to add a primary key! Let's add a new column for id used for large integer values
     ```
    <span style="color:blue">mysql></span> <copy>alter table poi add id bigint;</copy>
    ```
    ```
    <span style="color:blue">mysql></span> <copy>ALTER TABLE poi ADD PRIMARY KEY (id);</copy>
    ```

4. Create a copy of your city table
    ```
    <span style="color:blue">mysql></span> <copy>create table city_part as select * from city;</copy>
    ```

5. How many records does it contain?
    ```
    <span style="color:blue">mysql></span> <copy>SELECT count(*) FROM city_part;</copy>
    ```

6. How many records city table contain?
    ```
    <span style="color:blue">mysql></span> <copy>SELECT count(*) FROM city;</copy>
    ```

7. Verify the difference of the two table creation (there is a big one!)
    ```
    <span style="color:blue">mysql></span> <copy>show create table city\G</copy>
    ```
    ```
    <span style="color:blue">mysql></span> <copy>show create table city_part\G</copy>
    ```

8. Create an index on new table
    ```
    <span style="color:blue">mysql></span> <copy>CREATE INDEX myidindex ON city_part (ID);</copy>
    ```

9. Check table statistics. What is the Cardinality (=unique records) of primary key?
    ```
    <span style="color:blue">mysql></span> <copy>SELECT * FROM INFORMATION_SCHEMA.STATISTICS WHERE table_name ='city' and table_schema='world'\G</copy>
    ```

10. Delete some columns (Population and CountryCode)
    ```
    <span style="color:blue">mysql></span> <copy>ALTER TABLE city_part DROP COLUMN Population;</copy>
    ```
    ```
    <span style="color:blue">mysql></span> <copy>ALTER TABLE city_part DROP COLUMN CountryCode;</copy>
    ```
    ```
    <span style="color:blue">mysql></span> <copy>SHOW CREATE TABLE city_part;</copy>
    ```

11. Optimize the table
    ```
    <span style="color:blue">mysql></span> <copy>OPTIMIZE TABLE city_part;</copy>
    ```
    > **Note:** warning is expected: https://dev.mysql.com/doc/refman/8.0/en/optimize-table.html

12. Update table statistics

    ```
    <span style="color:blue">mysql></span> <copy>ANALYZE TABLE city_part;</copy>
    ```

## Task 2: Work with Joins

1. Join (Inner Join): An inner join returns rows where there is a match between the joined tables based on the specified condition.

    - List cities along with the countries they belong to
    
    ```
    <span style="color:blue">mysql></span> <copy>SELECT city.Name AS City, country.Name AS Country FROM city INNER JOIN country ON city.CountryCode = country.Code;</copy>
    ```

2. Left Join (Outer Join): A left join returns all rows from the left table and the matched rows from the right table. If there's no match, NULL values are included for the right table's columns.

    - List all countries along with their cities.
    
    ```
    <span style="color:blue">mysql></span> <copy>SELECT country.Name AS Country, city.Name AS City  FROM country LEFT JOIN city ON country.Code = city.CountryCode order by city.Name;</copy>
    ```

3. Left Semijoin (Outer Join): A left join returns all rows from the left table and the matched rows from the right table. If there's no match, NULL values are included for the right table's columns.

    - List all countries along with their official languages.
    
    ```
    <span style="color:blue">mysql></span> <copy>SELECT country.Name AS Country, countrylanguage.Language AS Language FROM country LEFT JOIN countrylanguage ON country.Code = countrylanguage.CountryCode AND countrylanguage.IsOfficial = 'T';</copy>
    ```

4. Right Join (Outer Join): A right join is similar to a left join but returns all rows from the right table and the matched rows from the left table.
    - List all official languages along with the countries that speak them.

    ```
    <span style="color:blue">mysql></span> <copy>SELECT countrylanguage.Language AS Language, country.Name AS Country FROM countrylanguage RIGHT JOIN country ON countrylanguage.CountryCode = country.Code AND countrylanguage.IsOfficial = 'T';</copy>
    ```

## Task 3: Work with Joins

1. Use the EXPLAIN command to show how MySQL will process this statement

    ```
    <span style="color:blue">mysql></span> <copy>EXPLAIN SELECT countrylanguage.Language AS Language, country.Name AS Country FROM countrylanguage RIGHT JOIN country ON countrylanguage.CountryCode = country.Code AND countrylanguage.IsOfficial = 'T';</copy>
    ```

2.  You can now exit from MySQL Shell
    ```
    <span style="color:blue">mysql></span> <copy>\q</copy>
    ```

## Acknowledgements

- **Author** - Marco Carlessi, Principal Sales Consultant
- **Contributors** -  Perside Foster, Principal Sales Consultant, Selena Sánchez, MySQL Solutions Engineer
- **Last Updated By/Date** - Perside Foster, Partner Solutions Engineer, November 2024
