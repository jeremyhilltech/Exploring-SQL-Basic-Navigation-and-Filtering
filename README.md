<p align="center">
<a href="https://cdn.pixabay.com/photo/2013/09/18/12/13/sqlite-183454_640.png"><img src="https://cdn.pixabay.com/photo/2013/09/18/12/13/sqlite-183454_640.png" title="tux" /></a>
</p>

# Using SQLite For Basic Navigation And Filtering Of SQL Databases
This lab shows navigation within SQL databases using basic commands such as `SELECT`, `FROM`, `WHERE`, `AND`, `OR`, `NOT`, and more to retrieve information using a SQlite3 command line. 

## Context:

This lab is constructed from a learning module in the Google Cybersecurity Professional course. This lab is not intended to be instruction for how to use SQL, but rather a demonstration of basic navigation, queries, and filtering. Database access using a SQLite3 command line was provided via remote access in a browser, and therefore this lab is not easily recreated, as you will not have access to the reference databases in this lab. Note also that commands are written in all caps to help with ease of human reading, however it is not required that all caps be used in practice when querying with SQLite. 

***Work shown here is for demonstration and purposes only.***

When using SQlite, you can find instructions on all special commands by typing `.help` in the command line. 

## Scenario:
You are a security professional at a large organization. Part of your job is to investigate security issues to help keep the system secure. You recently discovered some potential security issues that involve login attempts and employee machines.

Your task is to examine the organization’s data in their employees and log_in_attempts tables. You’ll need to use SQL filters to retrieve records from different datasets and investigate the potential security issues.

---
## Retrieve After-hours Login Attempts

You recently discovered a potential security incident that occurred after business hours. To investigate this, you need to query the `log_in_attempts` table and review after hours login activity. 

We will use filters in SQL to create a query that identifies all failed login attempts that occurred after 18:00. (The time of the login attempt is found in the `login_time` column. The `success` column contains a value of 0 when a login attempt failed; you can use either a value of 0 or FALSE in your query to identify failed login attempts.)

<a href="https://i.imgur.com/z3yStKG.png"><img src="https://i.imgur.com/z3yStKG.png" title="SQL1.1" /></a>

Here we have used `SELECT *` to query the database for all available informtion. 

We then use `FROM` to direct the query to the correct table, in this case `log_in_attempts`. 

Next, we specify the parameters of our query with the `WHERE` command, and include where we want to pull the information from (`login_time`).

We know we're looking for the login attempts after 6PM, so we use the `>` operator, and then specify said time as a string to be searched. (`18:00`).

Lastly, we set a parameter to show only failed login attempts. It's important to note here that SQL uses boolean values here, meaning that numerical output of 1 = TRUE, and 2 = FALSE. This also means that since we are working with boolean values that we will ***not*** use `''` marks around the values to be searched, as boolean values are not strings, and will return an error if we attempt to search them that way. 

---
## Retrieve Login Attempts On Specific Dates

A suspicious event occurred on 2022-05-09. To investigate this event, you want to review all login attempts which occurred on this day and the day before. Use filters in SQL to create a query that identifies all login attempts that occurred on 2022-05-09 or 2022-05-08. (The date of the login attempt is found in the `login_date` column.)

<a href="https://i.imgur.com/KAjkpSF.png"><img src="https://i.imgur.com/KAjkpSF.png" title="SQL2.1" /></a>

Here we repeat our first steps with `SELECT` and `FROM`. 

Next we modify our `WHERE` command to return data from either of the included dates: `WHERE login_date = '2022-05-09' OR login_date = '2022-05-09';`. Of important note here is that the dates are searched as strings, and the return will bring the data of either day or both days, depending on what data is available. Also of important note is that the column must be specified again after the `OR` filter used, so `login_date` is included twice here. This returns our data successfully, with a total (not seen in this screenshot due to the data produced) of 75 rows of information. 

---
## Retrieve Login Attempts Made Outside Of Mexico

There’s been suspicious activity with login attempts, but the team has determined that this activity didn't originate in Mexico. Now, you need to investigate login attempts that occurred outside of Mexico. Use filters in SQL to create a query that identifies all login attempts that occurred outside of Mexico. (When referring to Mexico, the `country` column contains values of both `MEX` and `MEXICO`, and you need to use the `LIKE` keyword with `%` to make sure your query reflects this.)

<a href="https://i.imgur.com/9wiLoQj.png"><img src="https://i.imgur.com/9wiLoQj.png" title="SQL3.1" /></a>

Here we modify our `WHERE` statement again to exclude all entries originating in Mexico. The challenge here is that there are entries on the table for Mexico that use either `MEX` or `MEXICO` in reference to logins from Mexico. 

First we use the `NOT` command to exclude the listed data set, and we then call on the `country` dataset to be specifically to be excluded. We then use `LIKE 'MEX%'` to exclude ALL strings that include the letters 'MEX' together. The `LIKE` command is what initiates the search in the strings inside of the `country` column for anything matching the criteria we enter before the `%` symbol. This will exclude data from all entries for Mexico regardless of how they were entered in the database. 

---
## Retrieve Employees In Marketing

Your team wants to perform security updates on specific employee machines in the Marketing department. You’re responsible for getting information on these employee machines and will need to query the employees table. Use filters in SQL to create a query that identifies all employees in the Marketing department for all offices in the East building.

(The department of the employee is found in the `department` column, which contains values that include `Marketing`. The office is found in the `office` column. Some examples of values in this column are `East-170`, `East-320`, and `North-434`. You’ll need to use the `LIKE` keyword with `%` to filter for the East building.) 

<a href="https://i.imgur.com/wMDJl4c.png"><img src="https://i.imgur.com/wMDJl4c.png" title="SQL4.1" /></a>

As seen above, we now use `FROM` to access the `employees` table. 

Next we construct our `WHERE` clause with the following: `department = 'Marketing'` tells the query we want only the Marketing department in our results, and `AND office LIKE 'East%';` queries results that only return offices located in the East office of the organization, regardless of office number. `LIKE` is used to identify patterns. 

---
## Retrieve Employees in Finance or Sales

Your team now needs to perform a different security update on machines for employees in the Sales and Finance departments. Use filters in SQL to create a query that identifies all employees in the Sales or Finance departments. (The department of the employee is found in the department column, which contains values that include Sales and Finance.)

<a href="https://i.imgur.com/EFQHjSO.png"><img src="https://i.imgur.com/EFQHjSO.png" title="SQL5.1" /></a>

By now we're getting the hang of using these filters. 

Now we modify our `WHERE` clause to focus only on `Finance` or `Sales` using the `departments` column. -> `WHERE department = 'Finance' OR department = 'Sales';`

---
## Retrieve All Employees Not In IT

Your team needs to make one more update to employee machines. The employees who are in the Information Technology department already had this update, but employees in all other departments need it. Use filters in SQL to create a query which identifies all employees not in the IT department. (The department of the employee is found in the department column, which contains values that include Information Technology.)

<a href="https://i.imgur.com/BB31bBR.png"><img src="https://i.imgur.com/BB31bBR.png" title="SQL6.1" /></a>

By now we're getting the hang of using these filters. 

Now we modify our `WHERE` clause to exclude anyone in the IT department since their computers have already received the needed updates. `WHERE NOT department = 'Information Technology';` yields us all of the computers minus those in IT that we can push updates to. 

---
## Summary

In completing this lab we have learned how to query and filter results in SQL databases using the SQLite command line. We used commands such as `SELECT`, `FROM`, `WHERE`, `NOT`, `AND`, `LIKE`, `OR`, and `%`. These tools allowed us to search for both inclusive and exclusive results and patterns within our data, helping us easily search suspicious login attempts on our networks, and identify information needed for efficient and effective decision making and resource management within our organization. 

---





