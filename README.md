<p align="center">
<a href="https://cdn.pixabay.com/photo/2013/09/18/12/13/sqlite-183454_640.png"><img src="https://cdn.pixabay.com/photo/2013/09/18/12/13/sqlite-183454_640.png" title="tux" /></a>
</p>

# Exploring-SQL-Basic-Navigation-and-Filtering
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

Next we construct our `WHERE` clause with the following: `department = 'Marketing'` tells the query we want only the Marketing department in our results, and `AND office LIKE 'East%';` queries results that only return offices located in the East office of the organization, regardless of office number. 

---
## Change Directory Permissions

In Linux, it's actually the `x` (execute) permission that allows a given owner the ability to see or access a directory. Let's say that we discover that our `drafts` directory has incorrect permissions enabled. Company policy states that only our user account (`researcher2`) has permission to access this directory. Our permissions tell us that currently, `groups` are also able to access this directory:

<a href="https://imgur.com/2PAKxHe"><img src="https://i.imgur.com/2PAKxHe.jpg" title="LC4.1" /></a>

In order to correct this, we have to do two things. First, we have to make sure that we are currently in the parent directory to `drafts`, in this case the `projects` directory. (Use `cd` to navigate to `projects` if needed.)

From there we can run `chmod g-x drafts` to remove `group` access to the drafts directory:

<a href="https://imgur.com/erNn46N"><img src="https://i.imgur.com/erNn46N.jpg" title="LC4.2" /></a>

And we can now confirm that `researcher2` is the only user allowed to access the `drafts` directory now, in compliance with company policy. 

---
## Summary

In completing this lab we have learned how to identify ourselves on a linux command line, navigate to a target directory and confirm our location, and list all of the files and directories we want to see, including hidden files. We've configured the permissions on these files and directories in accordance with company policy and reduced our attack surface in doing so. Only those owners who require access to these files now have access, and unnecessary permissions have been revoked where needed. In essense, we've strengthened one of the essential parts of the CIA Triad in our organization, namely Confidentiality. This is why regular audits are important; we must confirm that we aren't leaving holes in our attack surface to both internal and external threats. The Principle of Least Privilege requires that we only give permissions where absolutely necessary for employees to do their jobs, and of course to keep all unauthorized connections from outside the organzation from accessing our data. 

---
## Commands Used in this Lab:
* `whoami` = tells you what user you are currently operating as in the system
* `ls` = list all items in the working directory, argument `-l` shows permissions, argument `-la` shows permissions and hidden files. 
* `pwd` = print the working directory
* `cd` = change directory
* `chmod` = (change mode) = changes file/directory permissions for users, groups, and others




