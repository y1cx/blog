---
title: SQL
date: 2022-04-06 14:46:26
categories: Exercises
tags: SQL
description: SQL Exercises from LeetCode.
---

### SQL: Structured Query Language

Data Base Software -> Database -> Schema -> Table -> Data

#### Schema

Schema is a collection of tables, which can be split and grouped according to logic, and some table details meta settings can be set on this layer, like a blueprint. But in some database software, such as MySQL, Schema and Database are integrated into one.

Create Schema: Create a schema and give it a name, set the character encoding with the most common character 4-Byte UTF-8 Unicode Encoding and the derivative that can use emoji-related symbols, example: 

```sql
CREATE SCHEMA `new_schema` 
DEFAULT CHARACTER SET utf8mb4 
COLLATE utf8mb4_unicode_ci;
```

Show all schema:

```sql
SELECT * FROM INFORMATION_SCHEMA.SCHEMATA;
```

#### Table

Table record the metadata of each column, including the data type, default value, comments, etc.

Create Table: An example here creates a new table with the new schema, id is the column name and INT is the data type, NOT NULL is column attribute, with a comment. The metadata of the table is also declared, the primary key of the table is id.

```sql
CREATE SCHEMA `new_schema` 
DEFAULT CHARACTER SET utf8mb4 
COLLATE utf8mb4_unicode_ci;

CREATE TABLE `new_schema`.`new_table` (
  `id` INT NOT NULL COMMENT 'This is a primary index',
  PRIMARY KEY (`id`)
);
```

Read Table:

```sql
SHOW FULL COLUMNS FROM `new_schema`.`new_table`;
```

Delete Table:

```sql
DROP TABLE `new_schema`.`new_table`;
```

Clean Table:

```sql
TRUNCATE `new_schema`.`new_table`;
```

#### Column

```sql
CREATE SCHEMA `new_schema` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

CREATE TABLE `new_schema`.`users` (
  `id` INT NOT NULL AUTO_INCREMENT COMMENT 'This is the primary index',
  `name` VARCHAR(45) NOT NULL DEFAULT 'N/A',
  PRIMARY KEY (`id`)
);
```

```sql
ALTER TABLE `new_schema`.`users`
ADD COLUMN `age` INT NULL AFTER `name`;

ALTER TABLE `new_schema`.`users`
CHANGE COLUMN `id` `id` INT(11) NOT NULL AUTO_INCREMENT,
CHANGE COLUMN `name` `user_name` VARCHAR(45) NOT NULL DEFAULT 'No Name';

SHOW FULL COLUMNS FROM `new_schema`.`users`;
```

#### CASE Statement

#### Window Functions

https://mode.com/sql-tutorial/sql-window-functions/#advanced-windowing-techniques 

https://mode.com/blog/most-popular-window-functions-and-how-to-use-them/?utm_medium=referral&utm_source=mode-site&utm_campaign=sql-tutorial

```sql
SELECT start_terminal,
    duration_seconds,
    SUM(duration_seconds) OVER
        (PARTITION BY start_terminal ORDER BY start_time)
        AS running_total,
    COUNT(duration_seconds) OVER
        (PARTITION BY start_terminal ORDER BY start_time)
        AS running_count,
    AVG(duration_seconds) OVER
        (PARTITION BY start_terminal ORDER BY start_time)
        AS running_avg
FROM tutorial.dc_bikeshare_q1_2012
WHERE start_time < '2012-01-08'
```

#### RANK

```sql
SELECT start_terminal,
    start_time,
    duration_seconds,
    ROW_NUMBER() OVER (PARTITION BY start_terminal
        ORDER BY start_time)
        AS row_number
FROM tutorial.dc_bikeshare_q1_2012
WHERE start_time < '2012-01-08'

SELECT start_terminal,
    duration_seconds,
    RANK() OVER (PARTITION BY start_terminal
        ORDER BY start_time)
        AS rank
FROM tutorial.dc_bikeshare_q1_2012
WHERE start_time < '2012-01-08'
```

#### NLITE

```sql
SELECT start_terminal,
    duration_seconds,
    NTILE(4) OVER
        (PARTITION BY start_terminal ORDER BY duration_seconds)
        AS quartile,
    NTILE(5) OVER
        (PARTITION BY start_terminal ORDER BY duration_seconds)
        AS quintile,
    NTILE(100) OVER
        (PARTITION BY start_terminal ORDER BY duration_seconds)
        AS percentile
FROM tutorial.dc_bikeshare_q1_2012
WHERE start_time < '2012-01-08'
ORDER BY start_terminal, duration_seconds
```

#### LAG and LEAD

 Note: As you can see, the LAG() function allowed us to calculate the difference between each day's sales amount and the previous day's sales amount for each region. The NULL value in the sales_diff column indicates that there was no previous value to subtract from the current value.
 
```sql
SELECT
  date,
  region,
  sales_amount,
  sales_amount - LAG(sales_amount, 1) OVER (PARTITION BY region ORDER BY date) AS sales_diff
FROM
  sales;
```

### Exercises

175. Combine Two Tables

```sql
SELECT Person.firstName, Person.lastName, Address.city, Address.state 
FROM Person 
LEFT OUTER JOIN Address 
ON Person.personId = Address.personId;
```

176. Second Highest Salary

1st Method: Use functionIFNULL(, NULL) ro report null if there is no second highest salary. Use DISTINCT to show equal salary only once.

```sql
SELECT IFNULL(
(SELECT DISTINCT salary 
FROM Employee
ORDER BY salary DESC
LIMIT 1,1), NULL) AS SecondHighestSalary;
```

2nd Method:

```sql
SELECT MAX(salary) AS SecondHighestSalary
From Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```

177. Nth Highest Salary



178. Rank Scores 

1st Method: Use DENSE_RANK()

```sql
SELECT score, 
DENSE_RANK() OVER (ORDER BY score DESC) AS rank
FROM Scores;
```

2nd Method:

```sql
SELECT score, (SELECT COUNT(DISTINCT score) 
FROM scores s2 
WHERE s2.score >= s1.score) AS rank
FROM scores s1
ORDER BY score DESC;
```

181. Employees Earning More Than Their Managers

```sql
SELECT e.name AS Employee
FROM Employee AS e
JOIN Employee AS m
ON e.managerId = m.id
WHERE e.salary > m.salary;
```

182. Duplicate Emails

Note: GROUP BY, HAVING COUNT()

```sql
SELECT email
FROM Person
GROUP BY email
HAVING COUNT(email) > 1;
```

183. Customers Who Never Order

Note: IS NULL

```sql
SELECT c.name AS Customers
FROM Customers AS c
LEFT JOIN Orders AS o
ON c.id = o.customerId
WHERE o.customerId IS NULL
```

184. Department Highest Salary

Note: There might be more than one person with the highest salary in one department.

1st Method: 3 JOIN

```sql
SELECT d.name AS Department, e.name AS Employee, e.salary AS Salary
FROM Employee As e
JOIN Department AS d
ON e.departmentId = d.id
JOIN
(SELECT e.departmentId, MAX(e.salary) AS Salary
FROM Employee As e
GROUP BY e.departmentId) AS n
ON e.departmentId = n.departmentId 
AND e.salary = n.salary;
```

2nd Method: IN

```sql
SELECT
    Department.name AS Department,
    Employee.name AS Employee,
    Employee.salary AS Salary
FROM
    Employee
JOIN
    Department 
ON Employee.DepartmentId = Department.Id
WHERE
    (Employee.DepartmentId , Salary) 
    IN
        (SELECT 
            DepartmentId, MAX(Salary)
        FROM
            Employee
        GROUP BY 
            DepartmentId
    );
```

3rd Method: DENSE_RANK()

```sql
SELECT t.Department, t.Employee, t.Salary
FROM
(SELECT d.name AS Department, e.name AS Employee, e.salary AS Salary,
DENSE_RANK() OVER(PARTITION BY d.id ORDER BY e.salary DESC) AS rank
FROM Employee AS e
JOIN Department AS d
ON e.departmentId = d.id) AS t
WHERE t.rank = 1;
```

185. Department Top Three Salaries

1st Method: DENSE_RANK()

```sql
SELECT t.Department, t.Employee, t.Salary
FROM
(SELECT d.name AS Department, e.name AS Employee, e.salary AS Salary,
DENSE_RANK() OVER(PARTITION BY d.id ORDER BY e.salary DESC) AS rank
FROM Employee AS e
JOIN Department AS d
ON e.departmentId = d.id) AS t
WHERE t.rank <= 3;
```

2nd Method: No more than 2 salaries bigger than itself in the department.

```sql
SELECT
    d.Name AS 'Department', e1.Name AS 'Employee', e1.Salary
FROM
    Employee e1
        JOIN
    Department d ON e1.DepartmentId = d.Id
WHERE
    3 > (SELECT
            COUNT(DISTINCT e2.Salary)
        FROM
            Employee e2
        WHERE
            e2.Salary > e1.Salary
                AND e1.DepartmentId = e2.DepartmentId
        )
;
```

196. Delete Duplicate Emails

1st Method:

```sql
DELETE FROM Person 
WHERE id NOT IN 
(SELECT * 
 FROM(
     SELECT MIN(id) 
     FROM Person 
     GROUP BY email)
 AS p);
 ```

 ```sql
 DELETE FROM Person 
WHERE id NOT IN 
(
    SELECT MIN(p.id) 
    FROM (SELECT * FROM Person) AS p
    Group by p.email
);
```

2nd Method: Find bigger id having same address.

```sql
DELETE p1 
FROM Person p1, Person p2
WHERE
    p1.email = p2.email 
    AND p1.id > p2.id
```

197. Rising Temperature

Note: DATEFIFF() to compare two date type values.

```sql
SELECT DISTINCT w1.id
FROM Weather w1 
JOIN Weather w2 
ON DATEDIFF(w1.recordDate, w2.recordDate) = 1
WHERE  w1.temperature > w2.temperature
```

1303. Find the Team Size

```sql
SELECT Employee.employee_id, Output.team_size FROM Employee
INNER JOIN 
(SELECT team_id, COUNT(*) AS team_size FROM Employee
GROUP BY team_id) Output
WHERE Employee.team_id = Output.team_id
```

Note: Using PARTITION BY

```sql
SELECT 
    employee_id,
    COUNT(team_id) OVER(PARTITION BY team_id) AS team_size
FROM Employee
```

#### Test

```sql
SELECT e.country, e.export, i.import FROM
    (SELECT c.country, SUM(t.value) AS export 
    FROM companies c
    JOIN trades t ON c.name = t.seller
    GROUP BY c.country, t.seller) e
JOIN 
    (SELECT c.country, SUM(t.value) AS import
    FROM companies c
    JOIN trades t ON c.name = t.buyer
    GROUP BY c.country, t.buyer) i
ON e.country = i.country
ORDER BY e.country ASC
```

#### PIVOT