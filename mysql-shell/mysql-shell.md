# MySQL Shell: the new client

## Introduction

In this lab you will download and start using MySQL Shell.

Estimated Lab Time: 10 minutes

### Objectives

In this lab, you will execute MySQL Shell and explore the following on the interface:

* Help
* Settings
* Test an extension: reporting
* Command mode

> **Note:**
- Server: mysql1
- To close the MySQL Shell you can the commands “\q” or “\exit”

## Task 1: Install MySQL Shell on mysql1

1. If not already connected, connect to mysql1 server through app-srv

    ```
    <span style="color:green">shell-app-srv$</span> <copy>ssh -i $HOME/sshkeys/id_rsa_mysql1 opc@mysql1</copy>
    ```

2. Our installation package is under /workshop/linux/ directory

    ```
    <span style="color:green">shell-mysql1></span> <copy>ls -l /workshop/linux/mysql-shell-commercial-8.*.rpm</copy>
    ```

3. Install Mysql Shell, a new client that can be used in devOps organizations (you’ll learn more about it during the course) 

    ```
    <span style="color:green">shell-mysql1></span> <copy>sudo yum install /workshop/linux/mysql-shell-commercial-8.*.rpm</copy>
    ```

4. Launch MySQL shell

    ```
    <span style="color:green">shell-mysql1></span> <copy>mysqlsh</copy>
    ```

5. Discover the command auto-completion feature, type \h and press TAB twice

    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="background-color:orange">SQL</span>><copy>\h</copy> [TAB][TAB]
    ```

6. Check the available options. Add the letter “e” to “\h” and press TAB again to see that the command will automatically complete for you. Press enter and explore the help menu

    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="background-color:orange">SQL</span>><copy>\he</copy> [TAB]
    ```

7. Activate the command history autosave in MySQL shell  
    MySQL Shell comes with the option of automatically saving the history of command disabled by default. Therefore we need to check and to activate it.
    * Show settings and look for history.autoSave

    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="background-color:orange">SQL</span>><copy>\option -l</copy>
    ```
    
    * Activate autosave history
    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="background-color:orange">SQL</span>><copy>\option --persist history.autoSave=1</copy>
    ```

8. Close and reopen the session and in the new one **uses the arrow up key** to verify that the data from previous session are available

    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="background-color:orange">SQL</span>><copy>\q</copy>
    ```
    ```
    <span style="color:green">shell-mysql1></span> <copy>mysqlsh</copy>
    ```

## Task 2: MySQL as a SQL Client

1. MySQL Shell can be used as SQL client.  
   Connect to the newly installed mysql-advanced instance. Enter the password when requested and press enter. 
    
    When the prompt asks to save the password choose No and press Enter.

    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="background-color:orange">SQL</span>><copy>\c admin@mysql1:3307</copy>
    ```
2. Now you can submit SQL commands

    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="color:white"><span style="background-color:orange">SQL</span></span>><copy>show databases;</copy>
    ```
    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="color:white"><span style="background-color:orange">SQL</span></span>><copy>SELECT * FROM world.city LIMIT 10;</copy>
    ```

3. MySQL Shell has also nice features. Here we see one of them: reporting.
   You can check the actual sessions with one of these commands
    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="color:white"><span style="background-color:orange">SQL</span></span>><copy>show processlist;</copy>
    ```
    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="color:white"><span style="background-color:orange">SQL</span></span>><copy>select * from performance_schema.processlist;</copy>
    ```

4. But continously check connection can be annoying, so we use now the command “query” that continuously repeat a query (default interval refresh time: 5 seconds).
    To exit from reporting press CTRL+C
    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="color:white"><span style="background-color:orange">SQL</span></span>><copy>\watch query --interval=2 select now(), processlist.* from performance_schema.processlist;</copy>
    ```

5. Switch to Python command mode, then Javascript command mode, check how prompt change
    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="color:white"><span style="background-color:orange">SQL</span></span>><copy>\py</copy>
    ```
    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="color:white"><span style="background-color:blue">Py</span></span>><copy>\js</copy>
    ```

6. Exit from MySQL Shell with “\q” or “\exit”
    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="background-color:yellow">JS</span>><copy>\q</copy>
    ```

7. The **mysqlsh** command is simmilar to the **mysql** command
    ```
    <span style="color:green">shell-mysql1></span> <copy>mysqlsh -uadmin -p -hmysql1 -P3307</copy>
    ```
    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="color:white"><span style="background-color:orange">SQL</span></span>><copy>SHOW DATABASES;</copy>
    ```
    ```
    <span style="color:blue">My</span><span style="color: orange">SQL </span><span style="color:white"><span style="background-color:orange">SQL</span></span>><copy>\exit</copy>
    ```

8. From now on, if you like, the labs may be completed with MySQL Shell instead of classic mysql client.

## Acknowledgements

- **Author** - Marco Carlessi, Principal Sales Consultant
- **Contributors** -  Perside Foster, Principal Sales Consultant, Selena Sánchez, MySQL Solutions Engineer
- **Last Updated By/Date** - Perside Foster, Partner Solutions Engineer, November 2024
