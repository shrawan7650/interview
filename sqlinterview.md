# 🎯 Complete SQL Interview Preparation Guide

## Table of Contents
1. [SQL Basics (Foundation Level)](#1-sql-basics-foundation-level)
2. [SQL Querying (Filtering + Sorting + Limiting)](#2-sql-querying-filtering--sorting--limiting)
3. [SQL Aggregation + Grouping](#3-sql-aggregation--grouping)
4. [Joins (Most Asked Section)](#4-joins-most-asked-section)
5. [Subqueries & Nested Queries](#5-subqueries--nested-queries)
6. [Set Operations](#6-set-operations)
7. [SQL Functions (String, Date, Conversion)](#7-sql-functions-string-date-conversion)
8. [Window Functions (Advanced)](#8-window-functions-advanced)
9. [Normalization & Design Concepts](#9-normalization--design-concepts)
10. [Indexes, Views & Performance Optimization](#10-indexes-views--performance-optimization)
11. [Transactions & Locking (Advanced Concepts)](#11-transactions--locking-advanced-concepts)
12. [Tricky / Real Interview Queries](#12-tricky--real-interview-queries)
13. [4-Day Practice Plan](#13-4-day-practice-plan)
14. [Company-Specific Patterns](#14-company-specific-patterns)

---

## 1️⃣ SQL BASICS (FOUNDATION LEVEL)

### 🧩 Key Concepts

#### What is SQL?
- **SQL (Structured Query Language)**: Standard language for managing relational databases
- **MySQL**: A specific relational database management system (RDBMS)
- **DBMS**: Database Management System - software for storing and retrieving data

#### Data Types
- **Numeric**: INT, BIGINT, DECIMAL, FLOAT
- **String**: CHAR, VARCHAR, TEXT
- **Date/Time**: DATE, TIME, DATETIME, TIMESTAMP
- **Boolean**: BOOLEAN (TRUE/FALSE)

#### Constraints
- **PRIMARY KEY**: Unique identifier for each record
- **FOREIGN KEY**: Links two tables together
- **UNIQUE**: Ensures all values are different
- **NOT NULL**: Column cannot have NULL values
- **CHECK**: Validates data based on condition
- **DEFAULT**: Sets default value for column

#### SQL Command Categories
- **DDL (Data Definition Language)**: CREATE, ALTER, DROP, TRUNCATE
- **DML (Data Manipulation Language)**: SELECT, INSERT, UPDATE, DELETE
- **DCL (Data Control Language)**: GRANT, REVOKE
- **TCL (Transaction Control Language)**: COMMIT, ROLLBACK, SAVEPOINT

### 💬 Interview Questions & Answers

**Q1: What is the difference between DELETE, TRUNCATE, and DROP?**

```sql
-- DELETE: Removes specific rows, can use WHERE, can be rolled back, slower
DELETE FROM employees WHERE department = 'Sales';

-- TRUNCATE: Removes all rows, faster, cannot use WHERE, cannot be rolled back
TRUNCATE TABLE employees;

-- DROP: Removes entire table structure and data permanently
DROP TABLE employees;
```

**Answer:**
- **DELETE**: DML command, removes rows based on condition, uses WHERE clause, can be rolled back, slower (logs each row)
- **TRUNCATE**: DDL command, removes all rows, faster, cannot use WHERE, cannot be rolled back, resets identity
- **DROP**: DDL command, removes entire table structure permanently

**Q2: What are the types of SQL constraints?**

**Answer:**
1. PRIMARY KEY - Unique identifier
2. FOREIGN KEY - Referential integrity
3. UNIQUE - No duplicate values
4. NOT NULL - Must have a value
5. CHECK - Custom validation rules
6. DEFAULT - Default value if none provided

**Q3: What is the difference between CHAR and VARCHAR?**

**Answer:**
- **CHAR(n)**: Fixed-length, pads with spaces, faster for fixed-size data
  - Example: CHAR(10) always uses 10 bytes
- **VARCHAR(n)**: Variable-length, no padding, saves space
  - Example: VARCHAR(10) uses 1-10 bytes based on actual data

**Q4: What is a primary key and can a table have multiple primary keys?**

**Answer:**
- A primary key uniquely identifies each record in a table
- A table can have only ONE primary key
- However, a primary key can consist of multiple columns (composite key)

```sql
-- Single column primary key
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

-- Composite primary key
CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id)
);
```

**Q5: How do you find all rows where a column value is NULL?**

```sql
-- CORRECT
SELECT * FROM employees WHERE manager_id IS NULL;

-- INCORRECT (will not work)
SELECT * FROM employees WHERE manager_id = NULL;
```

**Answer:** Use `IS NULL` operator, not `= NULL` because NULL represents unknown value.

**Q6: What is the difference between WHERE and HAVING?**

```sql
-- WHERE: Filters rows before grouping
SELECT department, COUNT(*)
FROM employees
WHERE salary > 50000
GROUP BY department;

-- HAVING: Filters groups after grouping
SELECT department, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

**Answer:**
- **WHERE**: Filters individual rows, used before GROUP BY, cannot use aggregate functions
- **HAVING**: Filters grouped results, used after GROUP BY, can use aggregate functions

---

## 2️⃣ SQL QUERYING (FILTERING + SORTING + LIMITING)

### 🧩 Key Concepts

#### Basic SELECT Structure
```sql
SELECT column1, column2
FROM table_name
WHERE condition
ORDER BY column1 ASC/DESC
LIMIT n;
```

#### Pattern Matching with LIKE
- `%` - Matches zero or more characters
- `_` - Matches exactly one character

#### Operators
- **IN**: Checks if value exists in a list
- **BETWEEN**: Checks if value is within a range
- **IS NULL / IS NOT NULL**: Checks for NULL values

### 💬 Interview Questions & Answers

**Q1: Write a query to get all employees whose name starts with 'A'.**

```sql
SELECT * FROM employees
WHERE name LIKE 'A%';

-- More examples:
-- Ends with 'A'
SELECT * FROM employees WHERE name LIKE '%A';

-- Contains 'A' anywhere
SELECT * FROM employees WHERE name LIKE '%A%';

-- Second character is 'A'
SELECT * FROM employees WHERE name LIKE '_A%';

-- Exactly 5 characters and ends with 'A'
SELECT * FROM employees WHERE name LIKE '____A';
```

**Q2: Write a query to get records where salary is between 30,000 and 50,000.**

```sql
-- Using BETWEEN (inclusive)
SELECT * FROM employees
WHERE salary BETWEEN 30000 AND 50000;

-- Equivalent using comparison operators
SELECT * FROM employees
WHERE salary >= 30000 AND salary <= 50000;
```

**Q3: How do you sort employees by salary descending and then by name ascending?**

```sql
SELECT * FROM employees
ORDER BY salary DESC, name ASC;
```

**Answer:** Use multiple columns in ORDER BY. First criterion takes priority, second is used when first values are equal.

**Q4: What does SELECT DISTINCT do?**

```sql
-- Without DISTINCT (shows duplicates)
SELECT department FROM employees;

-- With DISTINCT (removes duplicates)
SELECT DISTINCT department FROM employees;

-- DISTINCT with multiple columns
SELECT DISTINCT department, city FROM employees;
```

**Answer:** DISTINCT removes duplicate rows from result set. With multiple columns, it considers unique combinations.

**Q5: Difference between IS NULL and = NULL?**

```sql
-- CORRECT
SELECT * FROM employees WHERE manager_id IS NULL;

-- INCORRECT (returns no results)
SELECT * FROM employees WHERE manager_id = NULL;
```

**Answer:**
- `IS NULL` is the correct way to check for NULL values
- `= NULL` always returns FALSE because NULL represents unknown, and you cannot compare unknown values

---

## 3️⃣ SQL AGGREGATION + GROUPING

### 🧩 Key Concepts

#### Aggregate Functions
- **COUNT()**: Counts number of rows
- **SUM()**: Adds numeric values
- **AVG()**: Calculates average
- **MIN()**: Finds minimum value
- **MAX()**: Finds maximum value

#### GROUP BY
Groups rows with same values into summary rows

#### HAVING
Filters grouped results (works with aggregate functions)

### 💬 Interview Questions & Answers

**Q1: Difference between WHERE and HAVING?**

```sql
-- Scenario: Find departments with average salary > 60000,
-- but only consider employees earning > 40000

SELECT department, AVG(salary) as avg_salary
FROM employees
WHERE salary > 40000          -- Filters individual rows BEFORE grouping
GROUP BY department
HAVING AVG(salary) > 60000;   -- Filters grouped results AFTER grouping
```

**Answer:**
| Aspect | WHERE | HAVING |
|--------|-------|--------|
| Applied | Before GROUP BY | After GROUP BY |
| Filters | Individual rows | Grouped results |
| Aggregate Functions | Cannot use | Can use |
| Example | WHERE salary > 50000 | HAVING COUNT(*) > 5 |

**Q2: Write a query to find the total salary department-wise.**

```sql
SELECT
    department,
    SUM(salary) as total_salary
FROM employees
GROUP BY department;

-- With additional formatting
SELECT
    department,
    SUM(salary) as total_salary,
    COUNT(*) as employee_count,
    AVG(salary) as avg_salary
FROM employees
GROUP BY department
ORDER BY total_salary DESC;
```

**Q3: How to get the number of employees in each department having more than 5 employees?**

```sql
SELECT
    department,
    COUNT(*) as employee_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 5
ORDER BY employee_count DESC;
```

**Q4: Can we use alias in HAVING clause?**

```sql
-- NOT ALLOWED in most databases (MySQL allows it)
SELECT
    department,
    COUNT(*) as emp_count
FROM employees
GROUP BY department
HAVING emp_count > 5;  -- May cause error

-- CORRECT WAY (works everywhere)
SELECT
    department,
    COUNT(*) as emp_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;
```

**Answer:** Generally, you cannot use column aliases in HAVING clause. Use the actual aggregate function instead. (MySQL is an exception that allows this).

**Additional Practice Questions:**

```sql
-- Q5: Find department with highest average salary
SELECT
    department,
    AVG(salary) as avg_salary
FROM employees
GROUP BY department
ORDER BY avg_salary DESC
LIMIT 1;

-- Q6: Count employees by department and city
SELECT
    department,
    city,
    COUNT(*) as count
FROM employees
GROUP BY department, city;

-- Q7: Find departments where minimum salary is above 40000
SELECT
    department,
    MIN(salary) as min_salary
FROM employees
GROUP BY department
HAVING MIN(salary) > 40000;
```

---

## 4️⃣ JOINS (MOST ASKED SECTION)

### 🧩 Key Concepts

#### Types of Joins

```
Visual Representation:

INNER JOIN          LEFT JOIN           RIGHT JOIN          FULL JOIN
   A ∩ B              A + (A ∩ B)         B + (A ∩ B)         A + B
   [===]              [=====]             [=====]             [=======]

CROSS JOIN          SELF JOIN
A × B               Table joined with itself
Every combination
```

### 💬 Interview Questions & Answers

**Q1: Difference between INNER JOIN and LEFT JOIN?**

```sql
-- Sample Tables:
-- employees: id, name, department_id
-- departments: id, name

-- INNER JOIN: Returns only matching rows from both tables
SELECT e.name, d.name as department
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;

-- LEFT JOIN: Returns all from left table, matching from right (NULL if no match)
SELECT e.name, d.name as department
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;
```

**Answer:**

| Aspect | INNER JOIN | LEFT JOIN |
|--------|-----------|-----------|
| Returns | Only matching rows | All rows from left + matches from right |
| Unmatched rows | Excluded | Included with NULL for right table |
| Use case | Need data in both tables | Need all data from main table |

**Example Data:**

```
employees:           departments:
id  name  dept_id    id  name
1   John  1          1   IT
2   Jane  2          2   HR
3   Bob   NULL       3   Sales

INNER JOIN Result:   LEFT JOIN Result:
John  IT             John  IT
Jane  HR             Jane  HR
                     Bob   NULL
```

**Q2: Write a query to get employee names and their department names.**

```sql
SELECT
    e.name as employee_name,
    d.name as department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;

-- With additional details
SELECT
    e.id,
    e.name as employee_name,
    e.salary,
    d.name as department_name,
    d.location
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id
ORDER BY e.name;
```

**Q3: What is a SELF JOIN? Give an example.**

```sql
-- Scenario: Find each employee and their manager (manager is also an employee)
-- Table: employees (id, name, manager_id)

SELECT
    e.name as employee_name,
    m.name as manager_name
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;

-- Example Data:
-- id  name     manager_id
-- 1   CEO      NULL
-- 2   John     1
-- 3   Jane     1
-- 4   Bob      2

-- Result:
-- employee_name  manager_name
-- CEO            NULL
-- John           CEO
-- Jane           CEO
-- Bob            John
```

**Answer:** SELF JOIN joins a table with itself. Useful for hierarchical data like employee-manager relationships, finding duplicates, or comparing rows within same table.

**Q4: How do you find unmatched records between two tables?**

```sql
-- Method 1: Using LEFT JOIN
SELECT a.*
FROM table_a a
LEFT JOIN table_b b ON a.id = b.id
WHERE b.id IS NULL;

-- Method 2: Using NOT IN
SELECT *
FROM table_a
WHERE id NOT IN (SELECT id FROM table_b);

-- Method 3: Using NOT EXISTS
SELECT *
FROM table_a a
WHERE NOT EXISTS (
    SELECT 1 FROM table_b b WHERE b.id = a.id
);
```

**Q5: What's the difference between JOIN and UNION?**

```sql
-- JOIN: Combines columns from multiple tables (horizontal merging)
SELECT e.name, d.name
FROM employees e
JOIN departments d ON e.department_id = d.id;

-- UNION: Combines rows from multiple queries (vertical merging)
SELECT name FROM employees
UNION
SELECT name FROM contractors;
```

**Answer:**

| Aspect | JOIN | UNION |
|--------|------|-------|
| Combines | Columns (horizontal) | Rows (vertical) |
| Tables | Can be different | Must have same structure |
| Result columns | From both tables | Same as first query |
| Duplicates | Keeps all | UNION removes, UNION ALL keeps |

**Additional Join Examples:**

```sql
-- RIGHT JOIN (rarely used, can be written as LEFT JOIN)
SELECT e.name, d.name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;

-- FULL OUTER JOIN (all rows from both tables)
SELECT e.name, d.name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;

-- CROSS JOIN (Cartesian product - every combination)
SELECT e.name, d.name
FROM employees e
CROSS JOIN departments d;

-- Multiple table joins
SELECT
    e.name,
    d.name as department,
    p.name as project,
    l.city as location
FROM employees e
JOIN departments d ON e.department_id = d.id
JOIN projects p ON e.project_id = p.id
JOIN locations l ON d.location_id = l.id;
```

---

## 5️⃣ SUBQUERIES & NESTED QUERIES

### 🧩 Key Concepts

#### Types of Subqueries
1. **Single-row subquery**: Returns one row
2. **Multiple-row subquery**: Returns multiple rows
3. **Correlated subquery**: References outer query
4. **Non-correlated subquery**: Independent of outer query

#### Subquery Locations
- **WHERE clause**: Filter based on subquery result
- **FROM clause**: Treat subquery as table (derived table)
- **SELECT clause**: Calculate values per row

### 💬 Interview Questions & Answers

**Q1: Write a query to find employees who earn more than the average salary.**

```sql
-- Method 1: Simple subquery
SELECT *
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Method 2: With comparison to average
SELECT
    name,
    salary,
    (SELECT AVG(salary) FROM employees) as avg_salary,
    salary - (SELECT AVG(salary) FROM employees) as difference
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

**Q2: Difference between correlated and non-correlated subquery?**

```sql
-- NON-CORRELATED: Subquery runs once, independent of outer query
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- CORRELATED: Subquery runs for each row of outer query
SELECT e1.name, e1.salary, e1.department_id
FROM employees e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e1.department_id  -- References outer query
);
```

**Answer:**

| Aspect | Non-Correlated | Correlated |
|--------|---------------|-----------|
| Dependency | Independent | Depends on outer query |
| Execution | Once | For each row of outer query |
| Performance | Faster | Slower |
| References | No outer query columns | References outer query |

**Q3: Can subqueries return multiple columns?**

```sql
-- YES - Multiple columns in FROM clause
SELECT *
FROM (
    SELECT
        department_id,
        AVG(salary) as avg_sal,
        COUNT(*) as emp_count
    FROM employees
    GROUP BY department_id
) as dept_stats
WHERE avg_sal > 60000;

-- YES - Multiple columns with IN (tuple comparison)
SELECT *
FROM employees
WHERE (department_id, salary) IN (
    SELECT department_id, MAX(salary)
    FROM employees
    GROUP BY department_id
);
```

**Answer:** Yes, subqueries can return multiple columns when used in FROM clause or with tuple comparison in WHERE clause.

**Q4: What happens if subquery returns more than one row?**

```sql
-- ERROR: Subquery returns more than one row
SELECT *
FROM employees
WHERE salary = (SELECT salary FROM employees WHERE department_id = 1);

-- CORRECT: Use IN for multiple rows
SELECT *
FROM employees
WHERE salary IN (SELECT salary FROM employees WHERE department_id = 1);

-- CORRECT: Use comparison with ANY/ALL
SELECT *
FROM employees
WHERE salary > ALL (SELECT salary FROM employees WHERE department_id = 1);
```

**Answer:** Using `=` with multiple-row subquery causes error. Use `IN`, `ANY`, `ALL`, or `EXISTS` instead.

**Additional Subquery Examples:**

```sql
-- Q5: Find employees earning more than anyone in Sales department
SELECT *
FROM employees
WHERE salary > ALL (
    SELECT salary
    FROM employees
    WHERE department = 'Sales'
);

-- Q6: Find departments with no employees
SELECT *
FROM departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.id
);

-- Q7: Subquery in SELECT clause
SELECT
    name,
    salary,
    (SELECT AVG(salary) FROM employees) as company_avg,
    (SELECT AVG(salary)
     FROM employees e2
     WHERE e2.department_id = e1.department_id) as dept_avg
FROM employees e1;

-- Q8: Subquery in FROM clause (derived table)
SELECT
    dept_name,
    total_salary
FROM (
    SELECT
        d.name as dept_name,
        SUM(e.salary) as total_salary
    FROM employees e
    JOIN departments d ON e.department_id = d.id
    GROUP BY d.name
) as dept_totals
WHERE total_salary > 200000;

-- Q9: Find second highest salary per department
SELECT department_id, MAX(salary) as second_highest
FROM employees e1
WHERE salary < (
    SELECT MAX(salary)
    FROM employees e2
    WHERE e2.department_id = e1.department_id
)
GROUP BY department_id;
```

---

## 6️⃣ SET OPERATIONS

### 🧩 Key Concepts

#### Set Operations
- **UNION**: Combines results and removes duplicates
- **UNION ALL**: Combines results and keeps duplicates
- **INTERSECT**: Returns only common rows
- **EXCEPT/MINUS**: Returns rows from first query not in second

#### Rules
- Same number of columns
- Compatible data types
- Column names from first query

### 💬 Interview Questions & Answers

**Q1: Difference between UNION and UNION ALL?**

```sql
-- Sample Tables:
-- table_a: id, name        table_b: id, name
--          1, John                 2, Jane
--          2, Jane                 3, Bob

-- UNION: Removes duplicates (slower)
SELECT name FROM table_a
UNION
SELECT name FROM table_b;
-- Result: John, Jane, Bob (3 rows)

-- UNION ALL: Keeps duplicates (faster)
SELECT name FROM table_a
UNION ALL
SELECT name FROM table_b;
-- Result: John, Jane, Jane, Bob (4 rows)
```

**Answer:**

| Aspect | UNION | UNION ALL |
|--------|-------|-----------|
| Duplicates | Removes | Keeps |
| Performance | Slower (needs to check duplicates) | Faster |
| Use case | Need unique results | Need all records |

**Q2: How to find records present in Table A but not in Table B?**

```sql
-- Method 1: EXCEPT (SQL Standard) / MINUS (Oracle)
SELECT id, name FROM table_a
EXCEPT
SELECT id, name FROM table_b;

-- Method 2: LEFT JOIN with NULL check
SELECT a.*
FROM table_a a
LEFT JOIN table_b b ON a.id = b.id
WHERE b.id IS NULL;

-- Method 3: NOT IN
SELECT *
FROM table_a
WHERE id NOT IN (SELECT id FROM table_b);

-- Method 4: NOT EXISTS
SELECT *
FROM table_a a
WHERE NOT EXISTS (
    SELECT 1 FROM table_b b WHERE b.id = a.id
);
```

**Additional Set Operation Examples:**

```sql
-- Q3: Find employees who are also managers
SELECT id, name FROM employees
INTERSECT
SELECT manager_id, name
FROM employees e
JOIN employees m ON e.manager_id = m.id;

-- Q4: Combine active and inactive users
SELECT user_id, name, 'active' as status FROM active_users
UNION ALL
SELECT user_id, name, 'inactive' as status FROM inactive_users;

-- Q5: Find common customers between two years
SELECT customer_id, name FROM sales_2023
INTERSECT
SELECT customer_id, name FROM sales_2024;

-- Q6: Complex UNION with aggregation
SELECT 'Q1' as quarter, SUM(amount) as total
FROM sales WHERE MONTH(date) BETWEEN 1 AND 3
UNION ALL
SELECT 'Q2', SUM(amount)
FROM sales WHERE MONTH(date) BETWEEN 4 AND 6
UNION ALL
SELECT 'Q3', SUM(amount)
FROM sales WHERE MONTH(date) BETWEEN 7 AND 9
UNION ALL
SELECT 'Q4', SUM(amount)
FROM sales WHERE MONTH(date) BETWEEN 10 AND 12;
```

---

## 7️⃣ SQL FUNCTIONS (STRING, DATE, CONVERSION)

### 🧩 Key Concepts

#### String Functions
- **UPPER/LOWER**: Case conversion
- **SUBSTRING/SUBSTR**: Extract part of string
- **LENGTH/LEN**: String length
- **CONCAT**: Join strings
- **TRIM/LTRIM/RTRIM**: Remove spaces
- **REPLACE**: Replace substring
- **LEFT/RIGHT**: Get characters from start/end

#### Date Functions
- **CURRENT_DATE/GETDATE/NOW**: Current date/time
- **DATEADD**: Add interval to date
- **DATEDIFF**: Difference between dates
- **EXTRACT/YEAR/MONTH/DAY**: Extract date parts
- **DATE_FORMAT/FORMAT**: Format dates

#### Conversion Functions
- **CAST**: Convert data type (SQL standard)
- **CONVERT**: Convert data type (SQL Server)

### 💬 Interview Questions & Answers

**Q1: Write a query to extract the first 3 characters of a name.**

```sql
-- SQL Standard
SELECT SUBSTRING(name, 1, 3) as short_name
FROM employees;

-- Alternative syntaxes
SELECT LEFT(name, 3) as short_name FROM employees;
SELECT SUBSTR(name, 1, 3) as short_name FROM employees;  -- PostgreSQL, MySQL

-- Complete example with multiple string operations
SELECT
    name,
    LEFT(name, 3) as first_3_chars,
    RIGHT(name, 3) as last_3_chars,
    UPPER(name) as uppercase,
    LOWER(name) as lowercase,
    LENGTH(name) as name_length,
    CONCAT(LEFT(name, 1), '.') as initial
FROM employees;
```

**Q2: How to find employees who joined in the current year?**

```sql
-- PostgreSQL
SELECT *
FROM employees
WHERE EXTRACT(YEAR FROM hire_date) = EXTRACT(YEAR FROM CURRENT_DATE);

-- MySQL
SELECT *
FROM employees
WHERE YEAR(hire_date) = YEAR(CURDATE());

-- SQL Server
SELECT *
FROM employees
WHERE YEAR(hire_date) = YEAR(GETDATE());

-- Generic approach
SELECT *
FROM employees
WHERE hire_date >= DATE_TRUNC('year', CURRENT_DATE);
```

**Q3: Difference between CAST and CONVERT?**

```sql
-- CAST (SQL Standard - works in all databases)
SELECT
    CAST(salary AS VARCHAR(20)) as salary_text,
    CAST('2024-01-15' AS DATE) as date_value,
    CAST(3.14159 AS INT) as rounded_value
FROM employees;

-- CONVERT (SQL Server specific - offers more formatting options)
SELECT
    CONVERT(VARCHAR(20), salary) as salary_text,
    CONVERT(DATE, '2024-01-15') as date_value,
    CONVERT(VARCHAR, hire_date, 101) as us_date_format  -- MM/DD/YYYY
FROM employees;
```

**Answer:**
- **CAST**: SQL standard, portable across databases, simpler syntax
- **CONVERT**: SQL Server specific, offers style parameter for date formatting
- Use CAST for portability, CONVERT for SQL Server date formatting

**Additional Function Examples:**

```sql
-- STRING FUNCTIONS

-- Q4: Get email username (before @)
SELECT
    email,
    SUBSTRING(email, 1, POSITION('@' IN email) - 1) as username
FROM users;

-- Q5: Replace and clean data
SELECT
    REPLACE(phone, '-', '') as clean_phone,
    TRIM(name) as clean_name,
    CONCAT(first_name, ' ', last_name) as full_name
FROM contacts;

-- Q6: Pattern matching and extraction
SELECT
    name,
    CASE
        WHEN name LIKE 'A%' THEN 'Starts with A'
        WHEN name LIKE '%son' THEN 'Ends with son'
        ELSE 'Other'
    END as name_category
FROM employees;

-- DATE FUNCTIONS

-- Q7: Calculate age
SELECT
    name,
    birth_date,
    DATEDIFF(YEAR, birth_date, GETDATE()) as age
FROM employees;

-- Q8: Date arithmetic
SELECT
    hire_date,
    hire_date + INTERVAL '1 year' as first_anniversary,  -- PostgreSQL
    DATEADD(MONTH, 3, hire_date) as probation_end,      -- SQL Server
    DATEDIFF(DAY, hire_date, CURRENT_DATE) as days_employed
FROM employees;

-- Q9: Format dates
SELECT
    order_date,
    DATE_FORMAT(order_date, '%Y-%m-%d') as iso_date,              -- MySQL
    DATE_FORMAT(order_date, '%M %d, %Y') as readable_date,        -- MySQL
    TO_CHAR(order_date, 'YYYY-MM-DD') as iso_date_pg,            -- PostgreSQL
    FORMAT(order_date, 'yyyy-MM-dd') as iso_date_sql_server      -- SQL Server
FROM orders;

-- Q10: Group by date parts
SELECT
    YEAR(order_date) as year,
    MONTH(order_date) as month,
    COUNT(*) as order_count,
    SUM(amount) as total_amount
FROM orders
GROUP BY YEAR(order_date), MONTH(order_date)
ORDER BY year, month;

-- CONVERSION FUNCTIONS

-- Q11: Multiple conversions
SELECT
    salary,
    CAST(salary AS VARCHAR) || ' USD' as formatted_salary,
    ROUND(CAST(salary AS DECIMAL(10,2)) / 12, 2) as monthly_salary,
    CAST(is_active AS INT) as active_flag
FROM employees;

-- Q12: Handle NULL conversions
SELECT
    name,
    COALESCE(CAST(bonus AS VARCHAR), 'No bonus') as bonus_text,
    CAST(COALESCE(commission, 0) AS DECIMAL(10,2)) as commission
FROM employees;
```

---

## 8️⃣ WINDOW FUNCTIONS (ADVANCED)

### 🧩 Key Concepts

#### Window Functions
Perform calculations across rows related to current row without grouping

#### Common Window Functions
- **ROW_NUMBER()**: Sequential number
- **RANK()**: Rank with gaps for ties
- **DENSE_RANK()**: Rank without gaps
- **NTILE(n)**: Divide into n buckets
- **LAG()**: Previous row value
- **LEAD()**: Next row value
- **FIRST_VALUE()**: First value in window
- **LAST_VALUE()**: Last value in window

#### Syntax
```sql
function() OVER (
    PARTITION BY column1      -- Optional: divide into groups
    ORDER BY column2          -- Required for ranking functions
    ROWS/RANGE specification  -- Optional: frame clause
)
```

### 💬 Interview Questions & Answers

**Q1: Difference between RANK() and DENSE_RANK()?**

```sql
-- Sample data: employees with salaries
-- name    salary
-- John    100000
-- Jane    100000
-- Bob     90000
-- Alice   90000
-- Mike    80000

SELECT
    name,
    salary,
    RANK() OVER (ORDER BY salary DESC) as rank,
    DENSE_RANK() OVER (ORDER BY salary DESC) as dense_rank,
    ROW_NUMBER() OVER (ORDER BY salary DESC) as row_num
FROM employees;

-- Result:
-- name    salary   rank  dense_rank  row_num
-- John    100000   1     1           1
-- Jane    100000   1     1           2
-- Bob     90000    3     2           3
-- Alice   90000    3     2           4
-- Mike    80000    5     3           5
```

**Answer:**

| Function | Behavior | Use Case |
|----------|----------|----------|
| RANK() | Skips numbers after ties (1,1,3) | Standard ranking |
| DENSE_RANK() | No gaps after ties (1,1,2) | Continuous ranking |
| ROW_NUMBER() | Always unique (1,2,3) | Sequential numbering |

**Q2: Write a query to find the 2nd highest salary using window function.**

```sql
-- Method 1: Using DENSE_RANK
SELECT name, salary
FROM (
    SELECT
        name,
        salary,
        DENSE_RANK() OVER (ORDER BY salary DESC) as rank
    FROM employees
) ranked
WHERE rank = 2;

-- Method 2: Using ROW_NUMBER (if we want unique salaries)
SELECT salary
FROM (
    SELECT
        DISTINCT salary,
        ROW_NUMBER() OVER (ORDER BY salary DESC) as row_num
    FROM employees
) ranked
WHERE row_num = 2;

-- Method 3: 2nd highest salary per department
SELECT department, name, salary
FROM (
    SELECT
        department,
        name,
        salary,
        DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) as rank
    FROM employees
) ranked
WHERE rank = 2;
```

**Q3: How does PARTITION BY work?**

```sql
-- Without PARTITION BY: Rank across entire table
SELECT
    name,
    department,
    salary,
    RANK() OVER (ORDER BY salary DESC) as overall_rank
FROM employees;

-- With PARTITION BY: Rank within each department
SELECT
    name,
    department,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) as dept_rank
FROM employees;

-- Multiple analytics per department
SELECT
    name,
    department,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) as dept_rank,
    AVG(salary) OVER (PARTITION BY department) as dept_avg_salary,
    COUNT(*) OVER (PARTITION BY department) as dept_emp_count,
    salary - AVG(salary) OVER (PARTITION BY department) as diff_from_avg
FROM employees;
```

**Answer:** PARTITION BY divides result set into groups. Window function applies separately to each group, like GROUP BY but without collapsing rows.

**Additional Window Function Examples:**

```sql
-- Q4: Running totals (cumulative sum)
SELECT
    order_date,
    amount,
    SUM(amount) OVER (ORDER BY order_date) as running_total,
    AVG(amount) OVER (ORDER BY order_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) as moving_avg_3
FROM orders;

-- Q5: Compare with previous and next values
SELECT
    name,
    salary,
    LAG(salary) OVER (ORDER BY salary) as prev_salary,
    LEAD(salary) OVER (ORDER BY salary) as next_salary,
    salary - LAG(salary) OVER (ORDER BY salary) as diff_from_prev
FROM employees;

-- Q6: Percentile and distribution
SELECT
    name,
    salary,
    NTILE(4) OVER (ORDER BY salary) as quartile,
    PERCENT_RANK() OVER (ORDER BY salary) as percent_rank,
    CUME_DIST() OVER (ORDER BY salary) as cumulative_distribution
FROM employees;

-- Q7: First and last values
SELECT
    name,
    department,
    salary,
    FIRST_VALUE(salary) OVER (PARTITION BY department ORDER BY salary DESC) as highest_in_dept,
    LAST_VALUE(salary) OVER (PARTITION BY department ORDER BY salary DESC
                             ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) as lowest_in_dept
FROM employees;

-- Q8: Top N per group
SELECT *
FROM (
    SELECT
        department,
        name,
        salary,
        ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) as rn
    FROM employees
) ranked
WHERE rn <= 3;  -- Top 3 salaries per department

-- Q9: Year-over-year comparison
SELECT
    YEAR(sale_date) as year,
    MONTH(sale_date) as month,
    SUM(amount) as monthly_sales,
    LAG(SUM(amount), 12) OVER (ORDER BY YEAR(sale_date), MONTH(sale_date)) as same_month_last_year,
    SUM(amount) - LAG(SUM(amount), 12) OVER (ORDER BY YEAR(sale_date), MONTH(sale_date)) as yoy_change
FROM sales
GROUP BY YEAR(sale_date), MONTH(sale_date);

-- Q10: Find gaps in sequences
SELECT
    id,
    LEAD(id) OVER (ORDER BY id) - id as gap_size
FROM table_name
WHERE LEAD(id) OVER (ORDER BY id) - id > 1;
```

---

## 9️⃣ NORMALIZATION & DESIGN CONCEPTS

### 🧩 Key Concepts

#### Normalization
Process of organizing data to reduce redundancy and improve data integrity

#### Normal Forms

**1NF (First Normal Form)**
- Atomic values (no repeating groups)
- Each column contains single value
- Each row is unique

**2NF (Second Normal Form)**
- Must be in 1NF
- No partial dependencies (non-key attributes depend on entire primary key)

**3NF (Third Normal Form)**
- Must be in 2NF
- No transitive dependencies (non-key attributes depend only on primary key)

**BCNF (Boyce-Codd Normal Form)**
- Stricter version of 3NF
- Every determinant is a candidate key

#### Types of Keys
- **Primary Key**: Unique identifier for table
- **Foreign Key**: References primary key in another table
- **Candidate Key**: Columns that could be primary key
- **Alternate Key**: Candidate keys not chosen as primary key
- **Composite Key**: Primary key made of multiple columns
- **Surrogate Key**: Artificial key (auto-increment ID)
- **Natural Key**: Real-world identifier (SSN, email)

### 💬 Interview Questions & Answers

**Q1: What is normalization and why is it needed?**

**Answer:** Normalization is the process of organizing database tables to minimize redundancy and dependency.

**Benefits:**
- Eliminates data redundancy (saves storage)
- Prevents update anomalies (inconsistent data)
- Improves data integrity
- Makes queries more efficient
- Easier maintenance

**Example of Unnormalized Data:**

```sql
-- BAD: Unnormalized (multiple phone numbers in one column)
CREATE TABLE customers (
    id INT,
    name VARCHAR(100),
    phones VARCHAR(200)  -- "555-1234, 555-5678, 555-9012"
);

-- GOOD: Normalized
CREATE TABLE customers (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE customer_phones (
    id INT PRIMARY KEY,
    customer_id INT,
    phone VARCHAR(20),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

**Q2: Difference between 2NF and 3NF?**

**2NF Example:**

```sql
-- Violates 2NF: teacher_name depends on teacher_id, not full composite key
CREATE TABLE course_enrollments (
    student_id INT,
    course_id INT,
    teacher_id INT,
    teacher_name VARCHAR(100),  -- Partial dependency problem
    grade CHAR(1),
    PRIMARY KEY (student_id, course_id)
);

-- Fixed to 2NF: Move teacher info to separate table
CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    grade CHAR(1),
    PRIMARY KEY (student_id, course_id)
);

CREATE TABLE courses (
    course_id INT PRIMARY KEY,
    teacher_id INT,
    teacher_name VARCHAR(100)
);
```

**3NF Example:**

```sql
-- Violates 3NF: city depends on zip_code (transitive dependency)
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    zip_code VARCHAR(10),
    city VARCHAR(100)  -- Depends on zip_code, not on id
);

-- Fixed to 3NF: Move zip/city relationship to separate table
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    zip_code VARCHAR(10),
    FOREIGN KEY (zip_code) REFERENCES zip_codes(code)
);

CREATE TABLE zip_codes (
    code VARCHAR(10) PRIMARY KEY,
    city VARCHAR(100)
);
```

**Answer:**

| Normal Form | Requirement | Example Issue |
|-------------|-------------|---------------|
| 2NF | No partial dependencies | Column depends on part of composite key |
| 3NF | No transitive dependencies | Column depends on non-key column |

**Q3: What is a surrogate key?**

```sql
-- Natural Key (real-world identifier)
CREATE TABLE employees (
    ssn VARCHAR(11) PRIMARY KEY,  -- Natural key
    name VARCHAR(100),
    email VARCHAR(100)
);

-- Surrogate Key (artificial identifier)
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,  -- Surrogate key
    ssn VARCHAR(11) UNIQUE,
    name VARCHAR(100),
    email VARCHAR(100)
);
```

**Answer:**

| Aspect | Natural Key | Surrogate Key |
|--------|-------------|---------------|
| Origin | Real-world data | System-generated |
| Example | SSN, Email | Auto-increment ID |
| Stability | Can change | Never changes |
| Meaning | Has business meaning | No business meaning |
| Best Practice | Use as UNIQUE constraint | Use as PRIMARY KEY |

**Additional Design Examples:**

```sql
-- Q4: Denormalization (intentional redundancy for performance)
-- OLTP (normalized)
CREATE TABLE orders (id INT, customer_id INT, total DECIMAL(10,2));
CREATE TABLE customers (id INT, name VARCHAR(100), email VARCHAR(100));

-- OLAP (denormalized for reporting)
CREATE TABLE orders_denormalized (
    order_id INT,
    customer_id INT,
    customer_name VARCHAR(100),  -- Redundant but faster for queries
    customer_email VARCHAR(100),  -- Redundant but faster for queries
    total DECIMAL(10,2)
);

-- Q5: Composite Key example
CREATE TABLE student_courses (
    student_id INT,
    course_id INT,
    enrollment_date DATE,
    grade CHAR(1),
    PRIMARY KEY (student_id, course_id),  -- Composite key
    FOREIGN KEY (student_id) REFERENCES students(id),
    FOREIGN KEY (course_id) REFERENCES courses(id)
);
```

---

## 🔟 INDEXES, VIEWS & PERFORMANCE OPTIMIZATION

### 🧩 Key Concepts

#### Indexes
Data structures that improve query performance by providing quick lookup

**Types:**
- **Clustered Index**: Sorts physical data, one per table
- **Non-Clustered Index**: Separate structure with pointers, multiple per table
- **Unique Index**: Ensures uniqueness
- **Composite Index**: Index on multiple columns

#### Views
Virtual tables based on SQL query

#### Performance Tips
- Use indexes on frequently queried columns
- Avoid SELECT *
- Use WHERE to filter early
- Limit result sets
- Analyze query execution plans

### 💬 Interview Questions & Answers

**Q1: What is an index and how does it improve performance?**

```sql
-- Without index: Full table scan (slow)
SELECT * FROM employees WHERE email = 'john@example.com';
-- Scans all 1,000,000 rows

-- Create index
CREATE INDEX idx_email ON employees(email);

-- With index: Direct lookup (fast)
SELECT * FROM employees WHERE email = 'john@example.com';
-- Uses index to find row immediately
```

**Answer:** Index is like a book's index - instead of reading every page, you look up the topic and jump to the right page. Database indexes work similarly by maintaining sorted lookup structures (typically B-trees) that allow quick data retrieval without scanning entire table.

**Performance Comparison:**
- Without index: O(n) - linear scan
- With index: O(log n) - tree search

**Q2: Difference between clustered and non-clustered index?**

```sql
-- Clustered Index: Determines physical order of data
-- Primary key automatically creates clustered index
CREATE TABLE employees (
    id INT PRIMARY KEY,  -- Clustered index
    name VARCHAR(100),
    email VARCHAR(100)
);

-- Non-Clustered Index: Separate structure with pointers
CREATE INDEX idx_name ON employees(name);        -- Non-clustered
CREATE INDEX idx_email ON employees(email);      -- Non-clustered

-- Composite index
CREATE INDEX idx_dept_salary ON employees(department_id, salary);
```

**Answer:**

| Aspect | Clustered Index | Non-Clustered Index |
|--------|----------------|---------------------|
| Per table | Only 1 | Multiple allowed |
| Data storage | Sorts actual data | Separate structure |
| Speed | Faster for range queries | Fast for exact lookups |
| Space | No extra space | Requires extra space |

**Visual Representation:**

```
Clustered Index:          Non-Clustered Index:
[Physical Data]           [Index]        [Physical Data]
Row 1 (ID=1)             ID=1 → Ptr     Row 3 (ID=3)
Row 2 (ID=2)             ID=2 → Ptr     Row 1 (ID=1)
Row 3 (ID=3)             ID=3 → Ptr     Row 2 (ID=2)
```

**Q3: What is a view and can we update data in a view?**

```sql
-- Create view
CREATE VIEW employee_summary AS
SELECT
    e.id,
    e.name,
    e.salary,
    d.name as department
FROM employees e
JOIN departments d ON e.department_id = d.id;

-- Query view like a table
SELECT * FROM employee_summary WHERE salary > 60000;

-- Simple view (updatable)
CREATE VIEW active_employees AS
SELECT id, name, email
FROM employees
WHERE status = 'active';

-- Update through view
UPDATE active_employees
SET email = 'new@example.com'
WHERE id = 1;

-- Complex view (NOT updatable - has JOIN, GROUP BY, etc.)
CREATE VIEW dept_stats AS
SELECT
    department_id,
    COUNT(*) as emp_count,
    AVG(salary) as avg_salary
FROM employees
GROUP BY department_id;
```

**Answer:** Views are virtual tables based on queries. Simple views (single table, no aggregations) are updatable. Complex views (joins, aggregations, DISTINCT, GROUP BY) are usually read-only.

**Benefits of Views:**
- Security (hide sensitive columns)
- Simplify complex queries
- Abstract database structure
- Reusable query logic

**Q4: Why is indexing sometimes bad for performance?**

```sql
-- Indexes slow down writes
INSERT INTO employees (name, email, salary, department_id)
VALUES ('John', 'john@example.com', 60000, 1);
-- Must update:
-- 1. Table data
-- 2. Index on email
-- 3. Index on department_id
-- 4. Index on salary (if exists)
-- Each index adds overhead!

-- Over-indexing hurts performance
CREATE INDEX idx1 ON employees(name);
CREATE INDEX idx2 ON employees(email);
CREATE INDEX idx3 ON employees(salary);
CREATE INDEX idx4 ON employees(department_id);
CREATE INDEX idx5 ON employees(hire_date);
-- Too many indexes!
```

**Answer:**

**Downsides of Indexes:**
- Slow down INSERT, UPDATE, DELETE (must update index)
- Consume disk space
- Too many indexes confuse query optimizer
- Maintenance overhead

**When NOT to index:**
- Small tables (full scan is fast enough)
- Columns frequently updated
- Columns with low cardinality (few unique values like boolean)
- Tables with more writes than reads

**Additional Performance Examples:**

```sql
-- Q5: Analyze query performance
EXPLAIN SELECT * FROM employees WHERE salary > 60000;
EXPLAIN ANALYZE SELECT * FROM employees WHERE salary > 60000;

-- Q6: Composite index usage (order matters!)
CREATE INDEX idx_dept_salary ON employees(department_id, salary);

-- Uses index (department_id is first)
SELECT * FROM employees WHERE department_id = 1 AND salary > 60000;

-- Uses index (department_id alone)
SELECT * FROM employees WHERE department_id = 1;

-- Does NOT use index efficiently (salary is second)
SELECT * FROM employees WHERE salary > 60000;

-- Q7: Covering index (includes all query columns)
CREATE INDEX idx_covering ON employees(department_id, name, salary);

-- No table lookup needed - all data in index
SELECT name, salary
FROM employees
WHERE department_id = 1;

-- Q8: Materialized view (PostgreSQL) - stores results
CREATE MATERIALIZED VIEW dept_stats AS
SELECT
    department_id,
    COUNT(*) as emp_count,
    AVG(salary) as avg_salary
FROM employees
GROUP BY department_id;

-- Refresh when data changes
REFRESH MATERIALIZED VIEW dept_stats;
```

---

## 1️⃣1️⃣ TRANSACTIONS & LOCKING (ADVANCED CONCEPTS)

### 🧩 Key Concepts

#### ACID Properties
- **Atomicity**: All or nothing
- **Consistency**: Database remains in valid state
- **Isolation**: Concurrent transactions don't interfere
- **Durability**: Committed changes are permanent

#### Transaction Commands
- **BEGIN/START TRANSACTION**: Start transaction
- **COMMIT**: Save changes permanently
- **ROLLBACK**: Undo changes
- **SAVEPOINT**: Create checkpoint within transaction

#### Isolation Levels
- **READ UNCOMMITTED**: Lowest isolation, dirty reads possible
- **READ COMMITTED**: Prevents dirty reads
- **REPEATABLE READ**: Prevents dirty and non-repeatable reads
- **SERIALIZABLE**: Highest isolation, no concurrency issues

### 💬 Interview Questions & Answers

**Q1: What are ACID properties?**

**Answer:**

**A - Atomicity**
All operations complete or none do (all-or-nothing)

```sql
BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 100 WHERE id = 1;
    UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
-- Both updates succeed or both fail
```

**C - Consistency**
Database moves from one valid state to another

```sql
-- Constraint ensures consistency
ALTER TABLE accounts ADD CONSTRAINT check_balance CHECK (balance >= 0);

BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
    -- Fails if balance goes negative
ROLLBACK;
```

**I - Isolation**
Concurrent transactions don't interfere with each other

```sql
-- Transaction 1
BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 100 WHERE id = 1;
    -- Transaction 2 cannot see this change yet
COMMIT;
```

**D - Durability**
Committed changes survive system failures

```sql
COMMIT;  -- Data is saved permanently, even if server crashes
```

**Q2: Difference between COMMIT and ROLLBACK?**

```sql
-- COMMIT: Save changes permanently
BEGIN TRANSACTION;
    INSERT INTO orders (customer_id, total) VALUES (1, 100);
    INSERT INTO order_items (order_id, product_id) VALUES (1, 10);
COMMIT;  -- Changes saved

-- ROLLBACK: Undo all changes
BEGIN TRANSACTION;
    DELETE FROM products WHERE id = 1;
    -- Oops, wrong product!
ROLLBACK;  -- Deletion undone

-- SAVEPOINT: Partial rollback
BEGIN TRANSACTION;
    INSERT INTO customers (name) VALUES ('John');
    SAVEPOINT sp1;
    INSERT INTO customers (name) VALUES ('Jane');
    SAVEPOINT sp2;
    INSERT INTO customers (name) VALUES ('Bob');
    ROLLBACK TO sp2;  -- Only Bob insertion undone
COMMIT;  -- John and Jane saved
```

**Answer:**

| Command | Effect | Use Case |
|---------|--------|----------|
| COMMIT | Makes changes permanent | Transaction successful |
| ROLLBACK | Undoes all changes | Error occurred, cancel everything |
| SAVEPOINT | Partial rollback point | Undo part of transaction |

**Q3: What is a deadlock and how to prevent it?**

**Example of Deadlock:**

```sql
-- Transaction 1
BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 100 WHERE id = 1;  -- Locks row 1
    -- Waits here for lock on row 2
    UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;

-- Transaction 2 (runs simultaneously)
BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 50 WHERE id = 2;   -- Locks row 2
    -- Waits here for lock on row 1
    UPDATE accounts SET balance = balance + 50 WHERE id = 1;
COMMIT;

-- DEADLOCK: Each transaction waits for the other's lock!
```

**Answer:** Deadlock occurs when two transactions wait for each other's locks, creating a circular dependency.

**Prevention Strategies:**

```sql
-- 1. Access resources in same order
BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 100 WHERE id = 1;  -- Always lock lower ID first
    UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;

-- 2. Use timeout
SET LOCK_TIMEOUT 5000;  -- Wait max 5 seconds

-- 3. Use lower isolation level
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

-- 4. Keep transactions short
BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 100 WHERE id = 1;
COMMIT;  -- Release lock immediately

-- 5. Use explicit locking order
SELECT * FROM accounts WHERE id IN (1, 2) ORDER BY id FOR UPDATE;
```

**Additional Transaction Examples:**

```sql
-- Q4: Transaction with error handling (PostgreSQL)
DO $$
BEGIN
    BEGIN
        INSERT INTO orders (customer_id, total) VALUES (1, 100);
        INSERT INTO order_items (order_id, product_id) VALUES (999, 10);  -- Error
    EXCEPTION WHEN OTHERS THEN
        ROLLBACK;
        RAISE NOTICE 'Error occurred, rolled back';
    END;
END $$;

-- Q5: Isolation levels demonstration
-- Session 1
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
BEGIN TRANSACTION;
    SELECT balance FROM accounts WHERE id = 1;  -- Returns 1000
    -- Session 2 updates balance to 500 here
    SELECT balance FROM accounts WHERE id = 1;  -- Returns 500 (non-repeatable read)
COMMIT;

-- Session 1 (with REPEATABLE READ)
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
BEGIN TRANSACTION;
    SELECT balance FROM accounts WHERE id = 1;  -- Returns 1000
    -- Session 2 updates balance to 500 here
    SELECT balance FROM accounts WHERE id = 1;  -- Still returns 1000 (consistent)
COMMIT;

-- Q6: Optimistic locking using version column
CREATE TABLE products (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    price DECIMAL(10,2),
    version INT DEFAULT 0
);

-- Update with version check
UPDATE products
SET price = 29.99, version = version + 1
WHERE id = 1 AND version = 5;  -- Only updates if version matches
```

---

## 1️⃣2️⃣ TRICKY / REAL INTERVIEW QUERIES

### 💬 Real Interview Problems

**Q1: Find the 2nd highest salary in employee table (multiple methods)**

```sql
-- Method 1: Subquery with MAX
SELECT MAX(salary) as second_highest
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);

-- Method 2: LIMIT with OFFSET
SELECT DISTINCT salary as second_highest
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;

-- Method 3: DENSE_RANK
SELECT salary as second_highest
FROM (
    SELECT
        salary,
        DENSE_RANK() OVER (ORDER BY salary DESC) as rank
    FROM employees
) ranked
WHERE rank = 2;

-- Method 4: Self JOIN
SELECT MAX(e1.salary) as second_highest
FROM employees e1
LEFT JOIN employees e2 ON e1.salary < e2.salary
GROUP BY e1.salary
ORDER BY e1.salary DESC
LIMIT 1;

-- Method 5: NOT IN
SELECT MAX(salary) as second_highest
FROM employees
WHERE salary NOT IN (SELECT MAX(salary) FROM employees);

-- Handle case when there's no 2nd highest (return NULL)
SELECT (
    SELECT DISTINCT salary
    FROM employees
    ORDER BY salary DESC
    LIMIT 1 OFFSET 1
) as second_highest;
```

**Q2: Find employees who earn more than their manager**

```sql
-- Using SELF JOIN
SELECT
    e.id,
    e.name as employee,
    e.salary as emp_salary,
    m.name as manager,
    m.salary as mgr_salary
FROM employees e
JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary;

-- With percentage difference
SELECT
    e.name as employee,
    e.salary as emp_salary,
    m.name as manager,
    m.salary as mgr_salary,
    ROUND((e.salary - m.salary) * 100.0 / m.salary, 2) as pct_more
FROM employees e
JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary
ORDER BY pct_more DESC;
```

**Q3: Find duplicate records in a table**

```sql
-- Method 1: Using GROUP BY and HAVING
SELECT
    email,
    COUNT(*) as duplicate_count
FROM employees
GROUP BY email
HAVING COUNT(*) > 1;

-- Method 2: Show all duplicate rows with details
SELECT e.*
FROM employees e
JOIN (
    SELECT email
    FROM employees
    GROUP BY email
    HAVING COUNT(*) > 1
) duplicates ON e.email = duplicates.email
ORDER BY e.email;

-- Method 3: Using window function
SELECT *
FROM (
    SELECT
        *,
        ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) as rn
    FROM employees
) t
WHERE rn > 1;

-- Method 4: Delete duplicates keeping one
DELETE FROM employees
WHERE id NOT IN (
    SELECT MIN(id)
    FROM employees
    GROUP BY email
);
```

**Q4: Find nth highest salary using subquery/window function**

```sql
-- Generic nth highest (n=3)
-- Method 1: Subquery
SELECT DISTINCT salary
FROM employees e1
WHERE (
    SELECT COUNT(DISTINCT salary)
    FROM employees e2
    WHERE e2.salary > e1.salary
) = 2;  -- n-1 for 3rd highest

-- Method 2: Window function (recommended)
SELECT salary
FROM (
    SELECT
        DISTINCT salary,
        DENSE_RANK() OVER (ORDER BY salary DESC) as rank
    FROM employees
) ranked
WHERE rank = 3;  -- Change to any n

-- Method 3: Using subquery with LIMIT
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 2;  -- n-1

-- Method 4: CTE for readability
WITH ranked_salaries AS (
    SELECT
        salary,
        DENSE_RANK() OVER (ORDER BY salary DESC) as rank
    FROM employees
)
SELECT salary as third_highest
FROM ranked_salaries
WHERE rank = 3;
```

**Q5: Write a query to find employees who joined on the same date**

```sql
-- Method 1: Self join
SELECT
    e1.name,
    e1.hire_date,
    e2.name as same_day_colleague
FROM employees e1
JOIN employees e2
    ON e1.hire_date = e2.hire_date
    AND e1.id < e2.id  -- Avoid duplicate pairs
ORDER BY e1.hire_date;

-- Method 2: Using GROUP BY
SELECT
    hire_date,
    COUNT(*) as employee_count,
    GROUP_CONCAT(name) as employees  -- String aggregate
FROM employees
GROUP BY hire_date
HAVING COUNT(*) > 1
ORDER BY hire_date;

-- Method 3: Window function
SELECT
    name,
    hire_date,
    COUNT(*) OVER (PARTITION BY hire_date) as same_day_count
FROM employees
WHERE COUNT(*) OVER (PARTITION BY hire_date) > 1
ORDER BY hire_date;
```

**Q6: Find departments with no employees**

```sql
-- Method 1: LEFT JOIN with NULL check
SELECT d.*
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
WHERE e.id IS NULL;

-- Method 2: NOT EXISTS
SELECT *
FROM departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.id
);

-- Method 3: NOT IN
SELECT *
FROM departments
WHERE id NOT IN (
    SELECT DISTINCT department_id
    FROM employees
    WHERE department_id IS NOT NULL
);
```

**Q7: Find consecutive numbers in a table**

```sql
-- Table: numbers (id, value)
-- Find numbers that appear at least 3 times consecutively

SELECT DISTINCT l1.value
FROM numbers l1
JOIN numbers l2 ON l1.id = l2.id - 1 AND l1.value = l2.value
JOIN numbers l3 ON l1.id = l3.id - 2 AND l1.value = l3.value;

-- Using window function (more elegant)
WITH grouped AS (
    SELECT
        value,
        id,
        id - ROW_NUMBER() OVER (PARTITION BY value ORDER BY id) as grp
    FROM numbers
)
SELECT DISTINCT value
FROM grouped
GROUP BY value, grp
HAVING COUNT(*) >= 3;
```

**Q8: Swap salary values**

```sql
-- Swap salaries between two specific employees
UPDATE employees
SET salary = CASE
    WHEN id = 1 THEN (SELECT salary FROM employees WHERE id = 2)
    WHEN id = 2 THEN (SELECT salary FROM employees WHERE id = 1)
    ELSE salary
END
WHERE id IN (1, 2);

-- Swap gender values (M <-> F)
UPDATE employees
SET gender = CASE
    WHEN gender = 'M' THEN 'F'
    WHEN gender = 'F' THEN 'M'
END;
```

**Q9: Calculate running total**

```sql
-- Method 1: Window function (best)
SELECT
    order_date,
    amount,
    SUM(amount) OVER (ORDER BY order_date) as running_total
FROM orders;

-- Method 2: Correlated subquery (slower)
SELECT
    o1.order_date,
    o1.amount,
    (SELECT SUM(o2.amount)
     FROM orders o2
     WHERE o2.order_date <= o1.order_date) as running_total
FROM orders o1
ORDER BY o1.order_date;
```

**Q10: Find customers who bought product A but not product B**

```sql
SELECT DISTINCT customer_id
FROM purchases
WHERE product_id = 'A'
  AND customer_id NOT IN (
      SELECT customer_id
      FROM purchases
      WHERE product_id = 'B'
  );

-- Alternative using EXISTS
SELECT DISTINCT customer_id
FROM purchases p1
WHERE p1.product_id = 'A'
  AND NOT EXISTS (
      SELECT 1
      FROM purchases p2
      WHERE p2.customer_id = p1.customer_id
        AND p2.product_id = 'B'
  );
```

**Q11: Find gaps in date sequences**

```sql
-- Find missing dates in a date range
WITH RECURSIVE date_range AS (
    SELECT MIN(order_date) as date FROM orders
    UNION ALL
    SELECT date + INTERVAL 1 DAY
    FROM date_range
    WHERE date < (SELECT MAX(order_date) FROM orders)
)
SELECT dr.date as missing_date
FROM date_range dr
LEFT JOIN orders o ON dr.date = o.order_date
WHERE o.order_date IS NULL;
```

**Q12: Pivot table (rows to columns)**

```sql
-- Before:
-- product  quarter  sales
-- A        Q1       100
-- A        Q2       150

-- After:
-- product  Q1   Q2   Q3   Q4

SELECT
    product,
    SUM(CASE WHEN quarter = 'Q1' THEN sales ELSE 0 END) as Q1,
    SUM(CASE WHEN quarter = 'Q2' THEN sales ELSE 0 END) as Q2,
    SUM(CASE WHEN quarter = 'Q3' THEN sales ELSE 0 END) as Q3,
    SUM(CASE WHEN quarter = 'Q4' THEN sales ELSE 0 END) as Q4
FROM sales
GROUP BY product;
```

---

## 1️⃣3️⃣ 4-DAY PRACTICE PLAN

### 📅 Day 1: Filtering & Sorting (15 Questions)

```sql
-- Q1: Get all employees whose salary is above 50000
SELECT * FROM employees WHERE salary > 50000;

-- Q2: Find employees whose name contains 'an'
SELECT * FROM employees WHERE name LIKE '%an%';

-- Q3: Get employees hired in 2023
SELECT * FROM employees WHERE YEAR(hire_date) = 2023;

-- Q4: List employees sorted by salary descending
SELECT * FROM employees ORDER BY salary DESC;

-- Q5: Get top 10 highest paid employees
SELECT * FROM employees ORDER BY salary DESC LIMIT 10;

-- Q6: Find employees in IT or HR department
SELECT * FROM employees WHERE department IN ('IT', 'HR');

-- Q7: Get employees with salary between 40000 and 60000
SELECT * FROM employees WHERE salary BETWEEN 40000 AND 60000;

-- Q8: Find employees whose email is NULL
SELECT * FROM employees WHERE email IS NULL;

-- Q9: Get distinct departments
SELECT DISTINCT department FROM employees;

-- Q10: Find employees not in Sales department
SELECT * FROM employees WHERE department != 'Sales';

-- Q11: Get employees hired after 2020 sorted by hire date
SELECT * FROM employees WHERE hire_date > '2020-01-01' ORDER BY hire_date;

-- Q12: Find employees with names starting with A or B
SELECT * FROM employees WHERE name LIKE 'A%' OR name LIKE 'B%';

-- Q13: Get first 5 employees alphabetically
SELECT * FROM employees ORDER BY name LIMIT 5;

-- Q14: Find employees with no manager (top-level)
SELECT * FROM employees WHERE manager_id IS NULL;

-- Q15: Get employees sorted by department, then salary descending
SELECT * FROM employees ORDER BY department, salary DESC;
```

### 📅 Day 2: Joins & Subqueries (15 Questions)

```sql
-- Q1: Get employee names with their department names
SELECT e.name, d.name as department
FROM employees e
JOIN departments d ON e.department_id = d.id;

-- Q2: Find all departments and their employee count
SELECT d.name, COUNT(e.id) as employee_count
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
GROUP BY d.id, d.name;

-- Q3: Get employees and their manager names
SELECT e.name as employee, m.name as manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;

-- Q4: Find employees earning more than average salary
SELECT * FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Q5: Get departments with more than 5 employees
SELECT d.name, COUNT(e.id) as count
FROM departments d
JOIN employees e ON d.id = e.department_id
GROUP BY d.id, d.name
HAVING COUNT(e.id) > 5;

-- Q6: Find employees who earn more than their department average
SELECT e.name, e.salary, e.department_id
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE department_id = e.department_id
);

-- Q7: List all projects with employee count
SELECT p.name, COUNT(ep.employee_id) as employee_count
FROM projects p
LEFT JOIN employee_projects ep ON p.id = ep.project_id
GROUP BY p.id, p.name;

-- Q8: Find employees not assigned to any project
SELECT e.*
FROM employees e
LEFT JOIN employee_projects ep ON e.id = ep.employee_id
WHERE ep.employee_id IS NULL;

-- Q9: Get highest paid employee per department
SELECT d.name, e.name, e.salary
FROM employees e
JOIN departments d ON e.department_id = d.id
WHERE e.salary = (
    SELECT MAX(salary)
    FROM employees
    WHERE department_id = e.department_id
);

-- Q10: Find departments with total salary > 200000
SELECT d.name, SUM(e.salary) as total_salary
FROM departments d
JOIN employees e ON d.id = e.department_id
GROUP BY d.id, d.name
HAVING SUM(e.salary) > 200000;

-- Q11: Get employees with same job title
SELECT e1.name, e2.name, e1.job_title
FROM employees e1
JOIN employees e2 ON e1.job_title = e2.job_title AND e1.id < e2.id;

-- Q12: Find employees in departments located in 'New York'
SELECT e.*
FROM employees e
WHERE department_id IN (
    SELECT id FROM departments WHERE location = 'New York'
);

-- Q13: Get all employee-project combinations
SELECT e.name, p.name
FROM employees e
CROSS JOIN projects p;

-- Q14: Find employees who work on more than 2 projects
SELECT e.name, COUNT(ep.project_id) as project_count
FROM employees e
JOIN employee_projects ep ON e.id = ep.employee_id
GROUP BY e.id, e.name
HAVING COUNT(ep.project_id) > 2;

-- Q15: List departments with no active projects
SELECT d.*
FROM departments d
WHERE NOT EXISTS (
    SELECT 1 FROM projects p
    WHERE p.department_id = d.id AND p.status = 'active'
);
```

### 📅 Day 3: Window Functions (10 Questions)

```sql
-- Q1: Rank employees by salary
SELECT
    name,
    salary,
    RANK() OVER (ORDER BY salary DESC) as rank
FROM employees;

-- Q2: Find 2nd highest salary per department
SELECT * FROM (
    SELECT
        name,
        department_id,
        salary,
        DENSE_RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) as rank
    FROM employees
) ranked
WHERE rank = 2;

-- Q3: Calculate running total of salaries
SELECT
    name,
    salary,
    SUM(salary) OVER (ORDER BY id) as running_total
FROM employees;

-- Q4: Find employees earning above their department average
SELECT
    name,
    salary,
    department_id,
    AVG(salary) OVER (PARTITION BY department_id) as dept_avg
FROM employees
WHERE salary > AVG(salary) OVER (PARTITION BY department_id);

-- Q5: Get previous and next employee salary
SELECT
    name,
    salary,
    LAG(salary) OVER (ORDER BY salary) as prev_salary,
    LEAD(salary) OVER (ORDER BY salary) as next_salary
FROM employees;

-- Q6: Calculate moving average (last 3 salaries)
SELECT
    name,
    salary,
    AVG(salary) OVER (ORDER BY id ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) as moving_avg
FROM employees;

-- Q7: Assign employees to quartiles by salary
SELECT
    name,
    salary,
    NTILE(4) OVER (ORDER BY salary) as quartile
FROM employees;

-- Q8: Get top 3 earners per department
SELECT * FROM (
    SELECT
        name,
        department_id,
        salary,
        ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) as rn
    FROM employees
) ranked
WHERE rn <= 3;

-- Q9: Calculate cumulative percentage
SELECT
    name,
    salary,
    SUM(salary) OVER (ORDER BY id) * 100.0 / SUM(salary) OVER () as cumulative_pct
FROM employees;

-- Q10: Find first and last hire in each department
SELECT DISTINCT
    department_id,
    FIRST_VALUE(name) OVER (PARTITION BY department_id ORDER BY hire_date) as first_hire,
    LAST_VALUE(name) OVER (PARTITION BY department_id ORDER BY hire_date
                           ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) as last_hire
FROM employees;
```

### 📅 Day 4: Real Interview SQL Challenges (10 Questions)

```sql
-- Q1: Find 2nd highest salary (handle ties)
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;

-- Q2: Delete duplicate rows keeping oldest
DELETE FROM employees
WHERE id NOT IN (
    SELECT MIN(id)
    FROM employees
    GROUP BY email
);

-- Q3: Find employees who earn more than their manager
SELECT e.name, e.salary, m.salary as manager_salary
FROM employees e
JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary;

-- Q4: Calculate department-wise salary percentile
SELECT
    name,
    department_id,
    salary,
    PERCENT_RANK() OVER (PARTITION BY department_id ORDER BY salary) as percentile
FROM employees;

-- Q5: Find longest consecutive working days
WITH daily_status AS (
    SELECT
        employee_id,
        work_date,
        work_date - ROW_NUMBER() OVER (PARTITION BY employee_id ORDER BY work_date) as grp
    FROM attendance
    WHERE status = 'present'
)
SELECT
    employee_id,
    MIN(work_date) as streak_start,
    MAX(work_date) as streak_end,
    COUNT(*) as consecutive_days
FROM daily_status
GROUP BY employee_id, grp
ORDER BY consecutive_days DESC;

-- Q6: Pivot sales data by quarter
SELECT
    product,
    SUM(CASE WHEN quarter = 'Q1' THEN sales ELSE 0 END) as Q1,
    SUM(CASE WHEN quarter = 'Q2' THEN sales ELSE 0 END) as Q2,
    SUM(CASE WHEN quarter = 'Q3' THEN sales ELSE 0 END) as Q3,
    SUM(CASE WHEN quarter = 'Q4' THEN sales ELSE 0 END) as Q4
FROM sales
GROUP BY product;

-- Q7: Find customers with consecutive orders
SELECT DISTINCT c1.customer_id
FROM orders c1
JOIN orders c2 ON c1.customer_id = c2.customer_id
    AND c1.order_date = c2.order_date - INTERVAL 1 DAY;

-- Q8: Calculate retention rate
SELECT
    DATE_TRUNC('month', first_purchase) as cohort_month,
    COUNT(DISTINCT customer_id) as cohort_size,
    COUNT(DISTINCT CASE WHEN months_since = 1 THEN customer_id END) * 100.0 /
        COUNT(DISTINCT customer_id) as month_1_retention
FROM (
    SELECT
        customer_id,
        MIN(purchase_date) as first_purchase,
        purchase_date,
        EXTRACT(MONTH FROM AGE(purchase_date, MIN(purchase_date) OVER (PARTITION BY customer_id))) as months_since
    FROM purchases
) cohorts
GROUP BY cohort_month;

-- Q9: Find products bought together
SELECT
    p1.product_id as product_a,
    p2.product_id as product_b,
    COUNT(DISTINCT p1.customer_id) as times_bought_together
FROM purchases p1
JOIN purchases p2 ON p1.customer_id = p2.customer_id
    AND p1.order_id = p2.order_id
    AND p1.product_id < p2.product_id
GROUP BY p1.product_id, p2.product_id
ORDER BY times_bought_together DESC;

-- Q10: Generate date series and find gaps
WITH RECURSIVE date_series AS (
    SELECT DATE '2024-01-01' as date
    UNION ALL
    SELECT date + INTERVAL '1 day'
    FROM date_series
    WHERE date < '2024-12-31'
)
SELECT ds.date as missing_date
FROM date_series ds
LEFT JOIN orders o ON ds.date = o.order_date
WHERE o.order_date IS NULL;
```

---

## 1️⃣4️⃣ COMPANY-SPECIFIC PATTERNS

### 🏢 TCS / Infosys / Wipro

**Focus Areas:**
- Basic joins and constraints
- DDL/DML commands
- GROUP BY and aggregations

**Sample Questions:**

```sql
-- Q1: Create table with constraints
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    salary DECIMAL(10,2) CHECK (salary > 0),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);

-- Q2: Simple joins with aggregation
SELECT
    d.name as department,
    COUNT(e.id) as employee_count,
    AVG(e.salary) as avg_salary
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
GROUP BY d.id, d.name;

-- Q3: Basic data manipulation
INSERT INTO employees (name, email, salary, department_id)
VALUES ('John Doe', 'john@example.com', 50000, 1);

UPDATE employees SET salary = salary * 1.1 WHERE department_id = 1;

DELETE FROM employees WHERE salary < 30000;
```

### 🏢 Accenture / Cognizant

**Focus Areas:**
- Subqueries
- Aggregate functions
- Complex filtering

**Sample Questions:**

```sql
-- Q1: Correlated subquery
SELECT
    name,
    salary,
    department_id
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE department_id = e.department_id
);

-- Q2: Multiple aggregations
SELECT
    department_id,
    COUNT(*) as emp_count,
    SUM(salary) as total_salary,
    AVG(salary) as avg_salary,
    MIN(salary) as min_salary,
    MAX(salary) as max_salary
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5;

-- Q3: Nested subqueries
SELECT name, salary
FROM employees
WHERE department_id IN (
    SELECT id
    FROM departments
    WHERE location IN (
        SELECT city
        FROM locations
        WHERE country = 'USA'
    )
);
```

### 🏢 Amazon / Flipkart / Paytm (Product-Based)

**Focus Areas:**
- Window functions
- Performance tuning
- Real-world data problems
- Complex business logic

**Sample Questions:**

```sql
-- Q1: Customer lifetime value
SELECT
    customer_id,
    COUNT(DISTINCT order_id) as total_orders,
    SUM(amount) as lifetime_value,
    AVG(amount) as avg_order_value,
    DATEDIFF(MAX(order_date), MIN(order_date)) as customer_lifetime_days
FROM orders
GROUP BY customer_id
HAVING COUNT(DISTINCT order_id) > 5
ORDER BY lifetime_value DESC;

-- Q2: Product recommendation (frequently bought together)
SELECT
    p1.product_id as product_a,
    p2.product_id as product_b,
    COUNT(*) as times_bought_together,
    COUNT(*) * 100.0 / (
        SELECT COUNT(DISTINCT order_id)
        FROM order_items
        WHERE product_id = p1.product_id
    ) as recommendation_score
FROM order_items p1
JOIN order_items p2 ON p1.order_id = p2.order_id AND p1.product_id < p2.product_id
GROUP BY p1.product_id, p2.product_id
HAVING COUNT(*) > 10
ORDER BY recommendation_score DESC;

-- Q3: User churn analysis
WITH user_activity AS (
    SELECT
        user_id,
        MAX(activity_date) as last_activity,
        COUNT(*) as total_activities,
        DATEDIFF(CURRENT_DATE, MAX(activity_date)) as days_since_last_activity
    FROM user_logs
    GROUP BY user_id
)
SELECT
    CASE
        WHEN days_since_last_activity <= 7 THEN 'Active'
        WHEN days_since_last_activity <= 30 THEN 'At Risk'
        ELSE 'Churned'
    END as user_status,
    COUNT(*) as user_count,
    AVG(total_activities) as avg_activities
FROM user_activity
GROUP BY
    CASE
        WHEN days_since_last_activity <= 7 THEN 'Active'
        WHEN days_since_last_activity <= 30 THEN 'At Risk'
        ELSE 'Churned'
    END;
```

### 🏢 Startups / Data Analyst Roles

**Focus Areas:**
- Joins with aggregations
- Case studies
- Business metrics
- Reporting queries

**Sample Questions:**

```sql
-- Q1: Monthly revenue trend
SELECT
    DATE_TRUNC('month', order_date) as month,
    COUNT(DISTINCT order_id) as total_orders,
    COUNT(DISTINCT customer_id) as unique_customers,
    SUM(amount) as revenue,
    AVG(amount) as avg_order_value,
    SUM(amount) - LAG(SUM(amount)) OVER (ORDER BY DATE_TRUNC('month', order_date)) as revenue_growth
FROM orders
GROUP BY DATE_TRUNC('month', order_date)
ORDER BY month;

-- Q2: Cohort analysis
SELECT
    DATE_TRUNC('month', registration_date) as cohort_month,
    COUNT(DISTINCT user_id) as cohort_size,
    COUNT(DISTINCT CASE WHEN DATE_TRUNC('month', activity_date) =
        DATE_TRUNC('month', registration_date) THEN user_id END) as month_0,
    COUNT(DISTINCT CASE WHEN DATE_TRUNC('month', activity_date) =
        DATE_TRUNC('month', registration_date) + INTERVAL '1 month' THEN user_id END) as month_1
FROM users u
LEFT JOIN activities a ON u.user_id = a.user_id
GROUP BY DATE_TRUNC('month', registration_date);

-- Q3: Product performance dashboard
SELECT
    p.name as product,
    p.category,
    COUNT(DISTINCT o.order_id) as orders,
    SUM(o.quantity) as units_sold,
    SUM(o.quantity * p.price) as revenue,
    AVG(r.rating) as avg_rating,
    COUNT(DISTINCT r.user_id) as review_count
FROM products p
LEFT JOIN order_items o ON p.id = o.product_id
LEFT JOIN reviews r ON p.id = r.product_id
GROUP BY p.id, p.name, p.category
ORDER BY revenue DESC;
```

---

## 🎯 Final Tips for Interview Success

### ✅ Do's
1. Always test your queries with sample data
2. Consider edge cases (NULL values, empty results)
3. Explain your thought process while writing queries
4. Use meaningful table and column aliases
5. Format SQL for readability
6. Understand execution order: FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
7. Know when to use indexes and their trade-offs
8. Practice writing queries without IDE assistance

### ❌ Don'ts
1. Don't use SELECT * in production (specify columns)
2. Don't create Cartesian products accidentally
3. Don't forget about NULL handling
4. Don't ignore data types in comparisons
5. Don't over-index tables
6. Don't write overly complex queries when simple ones work
7. Don't forget to consider query performance

### 🚀 Performance Optimization Checklist
- [ ] Use appropriate indexes
- [ ] Avoid SELECT *
- [ ] Use WHERE to filter early
- [ ] Use EXISTS instead of IN for large datasets
- [ ] Use LIMIT to restrict results
- [ ] Analyze query execution plans (EXPLAIN)
- [ ] Avoid functions on indexed columns in WHERE clause
- [ ] Use UNION ALL instead of UNION when duplicates are okay
- [ ] Denormalize for read-heavy workloads
- [ ] Partition large tables

### 📚 Quick Reference: SQL Execution Order

```
1. FROM & JOINs       -- Get tables and combine them
2. WHERE              -- Filter rows
3. GROUP BY           -- Group rows
4. HAVING             -- Filter groups
5. SELECT             -- Choose columns
6. DISTINCT           -- Remove duplicates
7. ORDER BY           -- Sort results
8. LIMIT/OFFSET       -- Restrict rows returned
```

### 🎓 Interview Strategy

**1. Understand the Question**
- Read carefully, identify key requirements
- Clarify ambiguities with interviewer

**2. Plan Your Approach**
- Think about table structure
- Identify necessary joins
- Consider edge cases

**3. Write the Query**
- Start simple, then optimize
- Use meaningful aliases
- Comment complex logic

**4. Test & Verify**
- Walk through with sample data
- Check for NULL handling
- Verify edge cases

**5. Optimize if Needed**
- Discuss indexes
- Consider performance
- Suggest alternatives

---

✅ How SQL Executes a Query (Basic → Advanced)
🔹 BASIC LEVEL (Execution Order of an SQL Query)

Many people think SQL runs in the order you write it — but SQL runs in a fixed internal order:

✅ SQL EXECUTION ORDER:
1. FROM
2. JOIN / ON
3. WHERE
4. GROUP BY
5. HAVING
6. SELECT
7. DISTINCT
8. ORDER BY
9. LIMIT / OFFSET

🔹 SIMPLE EXAMPLE:
SELECT name, salary
FROM employees
WHERE salary > 30000
ORDER BY salary DESC;

✅ Execution steps:

FROM → Load employees table

WHERE → Filter salary > 30000

SELECT → Get name, salary

ORDER BY → Sort high → low

💡 That’s why you can't use column alias in WHERE — because SELECT runs later.

🔹 INTERMEDIATE LEVEL (JOIN Execution)

Example:

SELECT e.name, d.department_name
FROM employees e
JOIN department d
ON e.dept_id = d.id
WHERE d.location = 'Delhi';

✅ Execution:

Load employees table

Load department table

Join using ON

Filter Delhi rows

Select columns

🔹 AGGREGATION & GROUPING
SELECT dept_id, COUNT(*) AS total
FROM employees
WHERE salary > 25000
GROUP BY dept_id
HAVING COUNT(*) > 3;

✅ Execution:

FROM → table

WHERE → filter rows

GROUP BY → make groups

HAVING → filter groups

SELECT → final output

🔹 ADVANCED LEVEL (Query Optimization)

SQL uses a Query Optimizer internally.

When you run:

SELECT * FROM users WHERE email = 'test@gmail.com';

✅ SQL Engine actions:

Check if email has an index

Choose:

Index Scan (fast)

OR Full Table Scan (slow)

Fetch matching rows

Return results

🔹 EXPLAIN (How SQL Shows Execution Plan)

To see how SQL runs your query:

EXPLAIN SELECT * FROM users WHERE email = 'abc@gmail.com';


Output shows:

index used or not

number of rows scanned

cost

🔹 COMMON INTERNAL OPERATIONS
Component	Meaning
Table Scan	Read full table
Index Scan	Read index only
Nested Loop	Compare row by row
Hash Join	Use memory for fast join
Sort	ORDER BY process
Temp Table	GROUP BY / DISTINCT
🔹 PERFORMANCE TIPS

✅ Always use:

Index on WHERE columns

Avoid SELECT *

Use LIMIT

Prefer WHERE over HAVING

Avoid functions on indexed columns

🔹 REAL-LIFE ANALOGY

Think SQL like making tea:

FROM = get ingredients
WHERE = remove bad leaves
GROUP BY = separate cups
HAVING = remove weak tea
SELECT = pour tea
ORDER BY = arrange cups
LIMIT = serve few cups

🔹 BONUS: Subqueries Execution
SELECT name
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

✅ First runs:

Inner query → get average
Outer query → compare salaries

## 🏆 Congratulations!

You now have a comprehensive SQL interview preparation guide covering everything from basics to advanced concepts. Practice these queries regularly, understand the concepts behind them, and you'll be well-prepared for any SQL interview.

### Next Steps:
1. Complete the 4-day practice plan
2. Solve queries on platforms like LeetCode, HackerRank, SQLZoo
3. Work with real databases to understand performance
4. Practice explaining your solutions verbally
5. Review this guide before interviews

**Good luck with your SQL interviews! 🚀**
