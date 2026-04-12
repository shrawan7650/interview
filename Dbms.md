# 🚀 DBMS Mastery Guide: Zero to Advanced + Placement Ready

> **By:** Senior DBMS Expert & Backend Engineer  
> **Goal:** Build strong DBMS concepts → Design real-world databases → Crack top company interviews  
> **Style:** Simple English + Hinglish + Real-world examples (WhatsApp, Zomato, Instagram, etc.)

---

## 📋 Table of Contents

1. [Module 1: Database Basics](#module-1-database-basics)
2. [Module 2: ER Model & Design](#module-2-er-model--design)
3. [Module 3: Keys in DBMS](#module-3-keys-in-dbms)
4. [Module 4: Normalization](#module-4-normalization)
5. [Module 5: SQL Integration](#module-5-sql-integration)
6. [Module 6: Transactions & ACID](#module-6-transactions--acid)
7. [Module 7: Concurrency Control](#module-7-concurrency-control)
8. [Module 8: Locking & Deadlocks](#module-8-locking--deadlocks)
9. [Module 9: Indexing](#module-9-indexing)
10. [Module 10: Query Optimization](#module-10-query-optimization)
11. [Module 11: Views, Stored Procedures & Triggers](#module-11-views-stored-procedures--triggers)
12. [Module 12: DB Architecture](#module-12-db-architecture)
13. [Module 13: NoSQL Basics](#module-13-nosql-basics)
14. [Module 14: Backend Connection](#module-14-backend-connection)
15. [Final Preparation & Mock Interview](#final-preparation--mock-interview)

---

## Module 1: Database Basics

### 🔹 What is a Database?

**Simple explanation:**  
Socho tumhare phone mein WhatsApp hai. Tumhare saare messages, contacts, media — sab kuch kahin store hota hai. Woh "kahin" hi ek **database** hai.

> **Database = Organized collection of data, stored so it can be easily accessed, managed, and updated.**

**Real-life examples:**
- **Zomato** → stores restaurant data, user orders, reviews
- **Instagram** → stores posts, likes, followers, DMs
- **Gmail** → stores emails, contacts, labels
- **Bank** → stores account numbers, transactions, balances

---

### 🔹 What is DBMS?

**DBMS = Database Management System**

It's the **software** that manages the database.

Think of it like this:
- **Database** = A huge library with millions of books
- **DBMS** = The librarian who organizes, finds, and manages those books

**Examples of DBMS software:**
- MySQL, PostgreSQL (most popular)
- Oracle, Microsoft SQL Server
- MongoDB (NoSQL)
- SQLite (used in Android apps)

**Without DBMS — problems:**
- No security (anyone can access data)
- No backup
- Data duplication everywhere
- No way to handle multiple users at once
- Hard to find specific data

**With DBMS — solutions:**
- Controlled access (login required)
- Automatic backup
- No duplicate data
- Handles 1000s of users simultaneously
- Fast data retrieval with queries

---

### 🔹 DBMS vs RDBMS

| Feature | DBMS | RDBMS |
|---------|------|-------|
| Full Form | Database Management System | Relational DBMS |
| Data Storage | Files, hierarchical | Tables (rows & columns) |
| Relationship | Not supported | Supported via foreign keys |
| Examples | XML DB, file systems | MySQL, PostgreSQL, Oracle |
| Data Integrity | Weak | Strong |
| ACID Properties | Not always | Always |
| SQL Support | Varies | Yes |

**Simple yaad karne ka trick:**  
RDBMS = DBMS + **Relationships between tables + SQL + ACID**

---

### 🔹 Types of Databases

```
Databases
├── Relational (SQL)       → MySQL, PostgreSQL, Oracle
├── NoSQL
│   ├── Document           → MongoDB (JSON-like)
│   ├── Key-Value          → Redis (cache)
│   ├── Column-family      → Cassandra (analytics)
│   └── Graph              → Neo4j (social networks)
├── NewSQL                 → Google Spanner, CockroachDB
├── In-Memory              → Redis, Memcached
├── Time-Series            → InfluxDB (IoT, metrics)
└── Search                 → Elasticsearch
```

**When to use which?**
- **Relational** → structured data, banking, e-commerce
- **MongoDB** → flexible data, content management, catalogs
- **Redis** → caching, sessions, leaderboards
- **Cassandra** → write-heavy, distributed, IoT
- **Neo4j** → friend recommendations, fraud detection

---

### ⭐ Module 1 — Interview Questions

**Basic:**
1. What is a database? Why do we need it?
2. Difference between DBMS and RDBMS?
3. What are the advantages of DBMS over file systems?
4. Name 5 popular databases and their use cases.

**Intermediate:**
5. What is data redundancy? How does DBMS solve it?
6. What are the different types of databases? When would you choose NoSQL over SQL?

**Tricky / Scenario-based:**
7. Zomato ke database mein daily 10 million orders store ho rahe hain. Aap kaunsa database choose karoge aur kyun?
8. Why does Instagram use both SQL and NoSQL databases together?
9. A file system can also store data. Why is DBMS better?

**Your Answers:**
- Q7 → Combination: PostgreSQL (for structured data: users, restaurants) + Cassandra (for high-write order data)
- Q8 → SQL for structured (user profiles, relationships) + NoSQL for unstructured (posts, stories, flexible schema)
- Q9 → File system has no: concurrency control, query language, security, backup, integrity constraints

---

### 📝 Module 1 — Revision Notes

```
DATABASE → Organized data storage
DBMS → Software to manage DB (MySQL, PostgreSQL, MongoDB)
RDBMS → DBMS + Tables + Relationships + ACID + SQL

Types: Relational | Document | Key-Value | Column | Graph | In-Memory
Choose SQL for: structured, relational data
Choose NoSQL for: flexible schema, scale, speed
```

---

## Module 2: ER Model & Design

### 🔹 What is ER Model?

**ER = Entity-Relationship**

ER Model is a **blueprint** of your database — like an architect draws a map before building a house.

> Before writing any SQL, you first design your ER Diagram (ERD).

---

### 🔹 Components of ER Model

#### 1. Entity
An **Entity** is a real-world object about which we store data.

```
Examples:
- Student (has name, roll number, age)
- Product (has id, name, price)
- Order (has order_id, date, amount)
- Employee (has emp_id, name, salary)
```

**Types of Entity:**
- **Strong Entity** → Has its own primary key (e.g., Student)
- **Weak Entity** → Depends on another entity (e.g., OrderItem depends on Order)

#### 2. Attributes
Properties of an entity.

```
Student Entity:
├── roll_no        (Simple Attribute)
├── name           (Simple Attribute)
├── address        (Composite: house_no + street + city)
├── phone_numbers  (Multivalued: can have multiple)
├── age            (Derived: calculated from DOB)
└── DOB            (Stored Attribute)
```

**Types of Attributes:**
| Type | Description | Example |
|------|-------------|---------|
| Simple | Cannot be divided | roll_no, price |
| Composite | Can be divided | address = city + state + zip |
| Multivalued | Multiple values | phone_numbers, skills |
| Derived | Calculated from other | age (from DOB) |
| Key | Uniquely identifies entity | student_id |

#### 3. Relationships
How entities are connected.

```
Student  ——— enrolls_in ———  Course
User     ——— places ———      Order
Doctor   ——— treats ———      Patient
```

**Relationship Types:**
- **Unary (Recursive):** Employee manages Employee
- **Binary:** Student enrolls in Course (most common)
- **Ternary:** Student, Course, Teacher (3-way)

---

### 🔹 Cardinality (MOST IMPORTANT)

Cardinality = How many instances of one entity relate to another.

#### One-to-One (1:1)
```
Person ————————— Passport
(One person has exactly one passport)

Aadhar Card ←→ Person
```

#### One-to-Many (1:M)
```
Customer ————————< Orders
(One customer can have MANY orders)
(But each order belongs to ONE customer)

Teacher ————————< Students
Department ————————< Employees
```

#### Many-to-Many (M:N)
```
Students >————————< Courses
(One student can enroll in MANY courses)
(One course can have MANY students)

Movies >————————< Actors
Products >————————< Orders
```

---

### 🔹 ER Diagram — Visual Notation

```
Rectangle    = Entity          [Student]
Ellipse      = Attribute       (name)
Diamond      = Relationship    <enrolls>
Double lines = Weak Entity     [[OrderItem]]
Underline    = Key attribute   (student_id)
Dashed oval  = Derived attr    (age)
Double oval  = Multivalued     ((phone))
```

**Example ER Diagram — Zomato (Text Format):**

```
[User] ——(places)—— [Order] ——(contains)—— [OrderItem] ——(refers_to)—— [MenuItem]
  |                    |                                                      |
(user_id)          (order_id)                                           (item_id)
(name)             (date)                                               (name)
(email)            (status)                                             (price)
(phone)            (total_amt)

[Order] ——(from)—— [Restaurant]
                       |
                   (restaurant_id)
                   (name)
                   (location)
                   (rating)
```

---

### 🔹 ER to Table Conversion (VERY IMPORTANT)

**Rules:**

**Rule 1:** Each strong entity → One table (with its attributes as columns)

**Rule 2:** Each weak entity → Table with its attributes + primary key of parent

**Rule 3:** 1:1 Relationship → Add foreign key to either table (prefer the table with optional participation)

**Rule 4:** 1:M Relationship → Add foreign key to the "many" side

**Rule 5:** M:N Relationship → Create a new **junction/bridge table** with both primary keys

---

**Example: Instagram (Simplified)**

**Entities:** User, Post, Comment, Hashtag

**Relationships:**
- User —posts→ Post (1:M)
- User —likes→ Post (M:N)
- Post —has→ Comment (1:M)
- Post —tagged_with→ Hashtag (M:N)

**Tables Generated:**

```sql
-- Rule 1: Strong Entities become tables
User(user_id PK, username, email, bio, created_at)
Post(post_id PK, caption, image_url, created_at)
Comment(comment_id PK, text, created_at)
Hashtag(hashtag_id PK, tag_name)

-- Rule 4: 1:M → FK on "many" side
Post(post_id PK, user_id FK, caption, ...)  -- user posts post
Comment(comment_id PK, user_id FK, post_id FK, text, ...)

-- Rule 5: M:N → Junction table
Post_Likes(user_id FK, post_id FK, liked_at)    -- user likes post
Post_Hashtags(post_id FK, hashtag_id FK)         -- post has hashtag
```

---

### ⭐ Module 2 — Interview Questions

**Basic:**
1. What is an ER diagram? Why is it used?
2. What are entities, attributes, and relationships?
3. What is cardinality? Explain 1:1, 1:M, M:N with examples.
4. What is the difference between strong and weak entities?

**Intermediate:**
5. How do you convert an M:N relationship to tables?
6. What is a composite attribute? Give an example.
7. What is a derived attribute? Is it stored in the database?

**Tricky:**
8. Design an ER diagram for WhatsApp.
9. If a student can enroll in multiple courses and a course can have multiple students, how many tables will you create?
10. When would you use a ternary relationship?

**Scenario-based:**
11. Design the database schema for a hospital management system.
12. Design ER model for Netflix (users, movies, subscriptions, watchlist).

---

### 📝 Module 2 — Revision Notes

```
ER Model = Blueprint of database design
Entity = Real-world object → becomes a TABLE
Attribute = Property of entity → becomes a COLUMN
Relationship = Connection between entities

Cardinality:
  1:1  → FK on either side
  1:M  → FK on "many" side  
  M:N  → Create junction/bridge table

Weak Entity = No own PK, depends on strong entity
Derived Attr = Calculated, not stored (e.g., age)
Multivalued Attr = Separate table needed
```

---

## Module 3: Keys in DBMS

### 🔹 Why Keys?

Database mein lakho rows hote hain. Keys help us:
- **Uniquely identify** each row
- **Link tables** together
- **Maintain data integrity**

---

### 🔹 Types of Keys

#### 1. Super Key
A set of one or more attributes that can **uniquely identify** a row.

```
Student Table: (student_id, name, email, phone)

Super Keys:
- {student_id}               ✓
- {email}                    ✓
- {phone}                    ✓
- {student_id, name}         ✓ (redundant but still a super key)
- {student_id, email, phone} ✓ (redundant)
```

> Super Key = Any combination that gives uniqueness (may have extra attributes)

#### 2. Candidate Key
A **minimal** Super Key — no extra attributes, can't remove any column.

```
From above Super Keys, minimal ones are:
- {student_id}   ✓ Candidate Key
- {email}        ✓ Candidate Key  
- {phone}        ✓ Candidate Key

{student_id, name} is NOT a candidate key (student_id alone is enough)
```

> Candidate Key = Minimal Super Key (no unnecessary columns)

#### 3. Primary Key
One Candidate Key chosen to **officially** identify rows. There can be **only ONE** per table.

```sql
CREATE TABLE Student (
    student_id INT PRIMARY KEY,  -- chosen PK
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,   -- email is still unique but not PK
    phone VARCHAR(15) UNIQUE
);
```

**Properties:**
- NOT NULL (cannot be empty)
- UNIQUE (no duplicates)
- Only ONE per table
- Immutable (should not change)

#### 4. Alternate Key
Candidate Keys that were **NOT chosen** as Primary Key.

```
If student_id is PK, then:
- email is an Alternate Key
- phone is an Alternate Key
```

#### 5. Foreign Key
An attribute in one table that **refers to the Primary Key** of another table.

```sql
-- Orders table references Users table
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    user_id INT,                          -- Foreign Key
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);
```

**Purpose:** Maintains **Referential Integrity** — you can't add an order for a user that doesn't exist.

#### 6. Composite Key
A Primary Key made of **two or more columns** together.

```sql
-- Neither student_id nor course_id alone is unique
-- Together they uniquely identify an enrollment
CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    enrolled_date DATE,
    PRIMARY KEY (student_id, course_id)  -- Composite Key
);
```

#### 7. Surrogate Key
An **artificial key** added by the system, not from the data itself.

```sql
CREATE TABLE Product (
    product_id INT AUTO_INCREMENT PRIMARY KEY,  -- Surrogate Key
    product_name VARCHAR(200),
    barcode VARCHAR(50) UNIQUE                   -- Natural Key
);
```

> Surrogate = system-generated (auto-increment IDs, UUIDs)  
> Natural Key = from real data (barcode, SSN, email)

---

### 🔹 Key Relationships Summary

```
All possible unique combinations
        ↓
    SUPER KEYS
        ↓ (remove redundancy)
  CANDIDATE KEYS
      /       \
PRIMARY KEY  ALTERNATE KEYS
(chosen one) (not chosen)

Tables linked by:
FOREIGN KEY → references PRIMARY KEY of another table
```

---

### 🔹 Real-World Example — Amazon

```
Users(user_id PK, name, email UK, phone UK, address)
                              ↑              ↑
                     Alternate Keys   Alternate Keys

Products(product_id PK, name, barcode UK, price, stock)

Orders(order_id PK, user_id FK→Users, order_date, total)
                        ↑
                   Foreign Key

OrderItems(order_id FK, product_id FK, quantity, price)
           ←————————— Composite PK ————————————→
```

---

### ⭐ Module 3 — Interview Questions

**Basic:**
1. What is a primary key? Properties of primary key?
2. What is the difference between primary key and unique key?
3. What is a foreign key? What integrity does it maintain?
4. Can a table have multiple primary keys?

**Intermediate:**
5. What is the difference between a super key and a candidate key?
6. When would you use a composite key?
7. Can a foreign key be NULL? Can it be a duplicate?
8. What is a surrogate key? When would you use it over a natural key?

**Tricky:**
9. Can a table have no primary key? Is it good practice?
10. Can a foreign key reference a non-primary key column?
11. What happens if you try to delete a parent row that has child rows (foreign key constraint)?

**Answers to Tricky:**
- Q9 → Yes, technically possible but BAD practice. No way to uniquely identify rows.
- Q10 → Yes, FK can reference UNIQUE key too, not just PK.
- Q11 → Depends on ON DELETE behavior: RESTRICT (error), CASCADE (delete children too), SET NULL, SET DEFAULT.

---

### 📝 Module 3 — Revision Notes

```
Super Key    → Any unique combination (may have extra cols)
Candidate Key → Minimal super key
Primary Key  → ONE chosen candidate key (NOT NULL + UNIQUE)
Alternate Key → Other candidate keys not chosen as PK
Foreign Key  → Links to PK of another table (referential integrity)
Composite Key → PK made of 2+ columns
Surrogate Key → System-generated (AUTO_INCREMENT, UUID)

Primary Key vs Unique Key:
  PK → NOT NULL + UNIQUE + only ONE per table
  UK → allows ONE NULL + UNIQUE + multiple allowed
```

---

## Module 4: Normalization

### 🔹 What is Normalization?

**Problem:** Without normalization, databases have **anomalies** (problems).

**Normalization** = Process of organizing tables to **reduce redundancy** and **improve data integrity**.

---

### 🔹 Problems Without Normalization (Anomalies)

**Bad Table Example:**

| order_id | customer_name | customer_email | product_id | product_name | product_price | quantity |
|----------|--------------|----------------|------------|--------------|---------------|----------|
| 1 | Rahul | rahul@gmail.com | P01 | iPhone | 80000 | 1 |
| 2 | Priya | priya@gmail.com | P01 | iPhone | 80000 | 2 |
| 3 | Rahul | rahul@gmail.com | P02 | iPad | 60000 | 1 |

**Problems:**
1. **Insertion Anomaly:** Can't add a product without an order
2. **Update Anomaly:** If iPhone price changes, update EVERY row (miss one = inconsistent)
3. **Deletion Anomaly:** Delete order 2 → lose all info about Priya

---

### 🔹 Functional Dependency

**FD: X → Y** means "X determines Y" (if we know X, we can find Y)

```
student_id → name          (knowing student_id gives us name)
student_id → email         (knowing student_id gives us email)
course_id  → course_name   (knowing course_id gives us course name)
{student_id, course_id} → grade  (need both to find grade)
```

**Types of FD:**
- **Full FD:** Y depends on ENTIRE composite key (not just part of it)
- **Partial FD:** Y depends on PART of composite key
- **Transitive FD:** X→Y and Y→Z, so X→Z (indirectly)

---

### 🔹 1NF — First Normal Form

**Rule:** Every column must have **atomic (indivisible) values**. No repeating groups.

**❌ Not in 1NF:**

| student_id | name | courses |
|------------|------|---------|
| 1 | Rahul | Math, Physics, Chemistry |
| 2 | Priya | Math, Biology |

**Problem:** `courses` has multiple values in one cell.

**✅ In 1NF:**

| student_id | name | course |
|------------|------|--------|
| 1 | Rahul | Math |
| 1 | Rahul | Physics |
| 1 | Rahul | Chemistry |
| 2 | Priya | Math |
| 2 | Priya | Biology |

**Rules for 1NF:**
- Each cell has single value (atomic)
- Each column has unique name
- Order of rows doesn't matter
- No repeating groups

---

### 🔹 2NF — Second Normal Form

**Rule:** Must be in 1NF + **No Partial Dependencies** (non-key attribute must depend on ENTIRE primary key)

> 2NF is only relevant when you have a COMPOSITE primary key.

**❌ Not in 2NF:**

| student_id | course_id | student_name | course_name | grade |
|------------|-----------|--------------|-------------|-------|
| 1 | C01 | Rahul | Math | A |
| 1 | C02 | Rahul | Physics | B |
| 2 | C01 | Priya | Math | A+ |

PK = {student_id, course_id}

**Partial Dependencies:**
- student_id → student_name (depends on PART of PK)
- course_id → course_name (depends on PART of PK)
- {student_id, course_id} → grade (depends on FULL PK ✓)

**✅ In 2NF (split into 3 tables):**

```
Student(student_id PK, student_name)
Course(course_id PK, course_name)
Enrollment(student_id FK, course_id FK, grade) ← Composite PK
```

---

### 🔹 3NF — Third Normal Form

**Rule:** Must be in 2NF + **No Transitive Dependencies**

> Non-key attribute should NOT depend on another non-key attribute.

**❌ Not in 3NF:**

| emp_id | emp_name | dept_id | dept_name | dept_location |
|--------|----------|---------|-----------|---------------|
| 1 | Rahul | D01 | IT | Bangalore |
| 2 | Priya | D01 | IT | Bangalore |
| 3 | Amit | D02 | HR | Mumbai |

PK = emp_id

**Transitive Dependency:**
- emp_id → dept_id → dept_name (dept_name depends on dept_id, not emp_id directly)
- emp_id → dept_id → dept_location

**✅ In 3NF:**

```
Employee(emp_id PK, emp_name, dept_id FK)
Department(dept_id PK, dept_name, dept_location)
```

---

### 🔹 BCNF — Boyce-Codd Normal Form

**Rule:** Must be in 3NF + **For every FD X→Y, X must be a Super Key**

> BCNF is stricter than 3NF. It handles cases where multiple overlapping candidate keys exist.

**❌ Not in BCNF (but IS in 3NF):**

| student_id | subject | teacher |
|------------|---------|---------|
| 1 | Math | Mr. Sharma |
| 1 | Physics | Ms. Gupta |
| 2 | Math | Mr. Sharma |

**Assume:** Each teacher teaches only one subject.

FD: teacher → subject (teacher is NOT a super key, but determines subject)

**✅ In BCNF:**

```
Teacher_Subject(teacher PK, subject)
Student_Teacher(student_id FK, teacher FK)
```

---

### 🔹 Normalization Summary

```
1NF → Atomic values, no repeating groups
  ↓
2NF → Remove Partial Dependencies (non-key depends on full PK)
  ↓
3NF → Remove Transitive Dependencies (non-key depends on non-key)
  ↓
BCNF → Every determinant must be a Super Key

Most real-world databases aim for 3NF or BCNF.
Sometimes we DENORMALIZE for performance.
```

---

### 🔹 Denormalization

Sometimes we intentionally introduce redundancy for **performance**.

**Example:** Instead of JOINing 5 tables every time, store some data redundantly.

```
Normalized:  SELECT o.*, u.name, u.email FROM orders o JOIN users u ON o.user_id = u.id
Denormalized: Orders table already has user_name and user_email columns → no JOIN needed
```

**When to denormalize:**
- Read-heavy systems (reports, analytics)
- Performance is critical
- Data doesn't change often

---

### ⭐ Module 4 — Interview Questions

**Basic:**
1. What is normalization? Why is it needed?
2. What are the different normal forms?
3. What are insertion, update, and deletion anomalies?
4. What is a functional dependency?

**Intermediate:**
5. Explain 2NF with an example.
6. What is transitive dependency? How does 3NF eliminate it?
7. What is the difference between 3NF and BCNF?
8. What is denormalization? When would you use it?

**Tricky:**
9. Is it always good to normalize completely? What are the tradeoffs?
10. Can a table be in 3NF but not in BCNF? Give an example.
11. Zomato ke restaurant reviews table ko normalize karo.

---

### 📝 Module 4 — Revision Notes

```
Normalization = Reduce redundancy + fix anomalies

1NF: Atomic values only
2NF: 1NF + No partial dependency (full PK dependency)
3NF: 2NF + No transitive dependency (X→Y→Z: remove Y→Z)
BCNF: 3NF + Every determinant is a super key

Anomalies (without normalization):
  Insert: Can't add data without other data
  Update: Change one place, breaks many
  Delete: Delete one thing, lose other

Denormalize when: Read performance > write correctness
```

---

## Module 5: SQL Integration

### 🔹 How DBMS Concepts Connect to SQL

Every DBMS concept has a SQL implementation.

---

### 🔹 DDL — Data Definition Language

```sql
-- Create Table (implements Entity)
CREATE TABLE Users (
    user_id   INT          PRIMARY KEY,     -- Primary Key
    username  VARCHAR(50)  NOT NULL UNIQUE, -- Candidate Key
    email     VARCHAR(100) NOT NULL UNIQUE, -- Alternate Key
    password  VARCHAR(255) NOT NULL,
    created_at TIMESTAMP   DEFAULT NOW()
);

-- Create Table with Foreign Key (implements Relationship)
CREATE TABLE Posts (
    post_id    INT          PRIMARY KEY,
    user_id    INT          NOT NULL,
    caption    TEXT,
    created_at TIMESTAMP    DEFAULT NOW(),
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
        ON DELETE CASCADE    -- if user deleted, delete posts too
        ON UPDATE CASCADE
);

-- Alter Table
ALTER TABLE Users ADD COLUMN phone VARCHAR(15);
ALTER TABLE Users DROP COLUMN phone;
ALTER TABLE Users MODIFY COLUMN username VARCHAR(100);

-- Delete Table
DROP TABLE Posts;
TRUNCATE TABLE Posts;  -- delete data but keep structure
```

---

### 🔹 DML — Data Manipulation Language

```sql
-- INSERT
INSERT INTO Users (user_id, username, email, password)
VALUES (1, 'rahul_dev', 'rahul@gmail.com', 'hashed_pw');

-- SELECT (most important)
SELECT u.username, p.caption, p.created_at
FROM Users u
JOIN Posts p ON u.user_id = p.user_id
WHERE u.user_id = 1
ORDER BY p.created_at DESC
LIMIT 10;

-- UPDATE
UPDATE Users
SET email = 'new@gmail.com'
WHERE user_id = 1;

-- DELETE
DELETE FROM Posts WHERE post_id = 5;
```

---

### 🔹 Joins — Bringing Related Tables Together

```sql
-- INNER JOIN: only matching rows from both tables
SELECT u.name, o.order_id, o.total
FROM Users u
INNER JOIN Orders o ON u.user_id = o.user_id;

-- LEFT JOIN: all from left, matching from right (NULL if no match)
SELECT u.name, o.order_id
FROM Users u
LEFT JOIN Orders o ON u.user_id = o.user_id;
-- Shows users even if they have no orders

-- RIGHT JOIN: all from right, matching from left
SELECT u.name, o.order_id
FROM Users u
RIGHT JOIN Orders o ON u.user_id = o.user_id;

-- FULL OUTER JOIN: all rows from both
SELECT u.name, o.order_id
FROM Users u
FULL OUTER JOIN Orders o ON u.user_id = o.user_id;

-- SELF JOIN: table joins with itself
SELECT e.name AS employee, m.name AS manager
FROM Employees e
LEFT JOIN Employees m ON e.manager_id = m.emp_id;
```

---

### 🔹 Aggregation & Grouping

```sql
-- GROUP BY with aggregates
SELECT 
    restaurant_id,
    COUNT(*) AS total_orders,
    SUM(amount) AS total_revenue,
    AVG(amount) AS avg_order_value,
    MAX(amount) AS highest_order,
    MIN(amount) AS lowest_order
FROM Orders
WHERE status = 'delivered'
GROUP BY restaurant_id
HAVING total_orders > 100     -- filter AFTER grouping (WHERE is BEFORE)
ORDER BY total_revenue DESC;
```

---

### 🔹 Subqueries & CTEs

```sql
-- Subquery: find users who placed at least one order
SELECT name FROM Users
WHERE user_id IN (SELECT DISTINCT user_id FROM Orders);

-- CTE (Common Table Expression) - cleaner
WITH active_users AS (
    SELECT DISTINCT user_id FROM Orders
    WHERE created_at > NOW() - INTERVAL 30 DAY
)
SELECT u.name, u.email
FROM Users u
JOIN active_users au ON u.user_id = au.user_id;

-- Window Functions (Advanced)
SELECT 
    user_id,
    order_id,
    amount,
    RANK() OVER (PARTITION BY user_id ORDER BY amount DESC) AS order_rank,
    SUM(amount) OVER (PARTITION BY user_id) AS user_total
FROM Orders;
```

---

### ⭐ Module 5 — Interview Questions

**Basic:**
1. Difference between WHERE and HAVING?
2. What is the difference between DELETE, TRUNCATE, and DROP?
3. Explain different types of JOINs.
4. What is a subquery?

**Intermediate:**
5. What is a CTE? How is it different from a subquery?
6. Write a query to find the second highest salary.
7. What are window functions? Give an example.
8. Difference between UNION and UNION ALL?

**Tricky SQL:**
9. Find duplicate emails in a Users table.
10. Find employees who earn more than their manager.
11. Find the nth highest salary without using LIMIT.

**SQL Answers:**

```sql
-- Q6: Second highest salary
SELECT MAX(salary) FROM Employees
WHERE salary < (SELECT MAX(salary) FROM Employees);

-- Q9: Find duplicates
SELECT email, COUNT(*) FROM Users
GROUP BY email HAVING COUNT(*) > 1;

-- Q10: Employee earns more than manager
SELECT e.name AS employee, m.name AS manager
FROM Employees e JOIN Employees m ON e.manager_id = m.emp_id
WHERE e.salary > m.salary;

-- Q11: Nth highest salary (N=3)
SELECT DISTINCT salary FROM Employees
ORDER BY salary DESC LIMIT 1 OFFSET 2; -- offset = N-1
```

---

### 📝 Module 5 — Revision Notes

```
DDL: CREATE, ALTER, DROP, TRUNCATE
DML: SELECT, INSERT, UPDATE, DELETE
DCL: GRANT, REVOKE
TCL: COMMIT, ROLLBACK, SAVEPOINT

JOIN types:
  INNER → matching rows only
  LEFT  → all left + matching right (NULL for non-match)
  RIGHT → all right + matching left
  FULL  → all rows from both

WHERE → filter rows BEFORE grouping
HAVING → filter groups AFTER GROUP BY
DELETE → delete rows (can rollback)
TRUNCATE → delete all rows fast (cannot rollback in most DBs)
DROP → delete entire table structure
```

---

## Module 6: Transactions & ACID

### 🔹 What is a Transaction?

**Real-life example:** You transfer ₹5000 from your account to your friend's account.

**Steps:**
1. Debit ₹5000 from YOUR account
2. Credit ₹5000 to FRIEND's account

**What if power cuts after step 1 but before step 2?** ₹5000 is gone from your account but friend didn't receive it! 💀

**Transaction** = A group of operations that must be completed **ALL or NONE**.

```sql
START TRANSACTION;

UPDATE Accounts SET balance = balance - 5000 WHERE user_id = 1; -- debit
UPDATE Accounts SET balance = balance + 5000 WHERE user_id = 2; -- credit

-- If everything OK:
COMMIT;

-- If any error:
ROLLBACK;  -- undo everything
```

---

### 🔹 ACID Properties

ACID = 4 guarantees a reliable transaction must have.

#### A — Atomicity
**"All or Nothing"**

Either ALL operations in a transaction succeed, or NONE happen.

```
Transfer ₹5000:
  Step 1: Debit ✓
  Step 2: Credit ✗ (network failure)
  Result: ROLLBACK → debit also undone. Back to start.
```

#### C — Consistency
**Database must go from one valid state to another valid state.**

```
Before transfer: You have ₹10,000, Friend has ₹2,000. Total = ₹12,000
After transfer:  You have ₹5,000, Friend has ₹7,000. Total = ₹12,000

Total money in system = same. Consistency maintained ✓
```

#### I — Isolation
**Concurrent transactions should NOT interfere with each other.**

```
Transaction A: Reads balance (₹10,000), will deduct ₹3,000
Transaction B: Reads balance (₹10,000), will deduct ₹8,000

Without isolation: Both succeed → balance = -₹1,000 (impossible!) 
With isolation: B waits for A. A deducts → ₹7,000. B checks ₹7,000 → insufficient funds.
```

#### D — Durability
**Once committed, changes are permanent** — even if system crashes.

```
Committed your order on Zomato → even if server crashes, your order is saved.
Data written to disk/logs permanently.
```

---

### 🔹 Transaction States

```
                 BEGIN
                   ↓
            [ACTIVE] ← executing operations
                   ↓
        ┌──────────┴──────────┐
        ↓                     ↓
  [PARTIALLY           [FAILED]
   COMMITTED]               ↓
        ↓              [ABORTED/ROLLED BACK]
  [COMMITTED]         (back to original state)
```

---

### 🔹 Transaction Control Statements

```sql
START TRANSACTION;    -- begin transaction
SAVEPOINT sp1;        -- create checkpoint
-- ... some operations ...
ROLLBACK TO sp1;      -- undo back to checkpoint only
-- ... more operations ...
COMMIT;               -- make all changes permanent

-- Example with savepoint:
START TRANSACTION;
  INSERT INTO Orders VALUES (...);
  SAVEPOINT after_order;
  
  INSERT INTO Payments VALUES (...);
  -- Payment fails!
  ROLLBACK TO after_order;  -- undo payment only, keep order
  
COMMIT;
```

---

### ⭐ Module 6 — Interview Questions

**Basic:**
1. What is a transaction? Give a real-world example.
2. Explain ACID properties.
3. What is the difference between COMMIT and ROLLBACK?
4. What is atomicity?

**Intermediate:**
5. Why is isolation needed? What happens without it?
6. What is the difference between consistency and integrity?
7. What is a SAVEPOINT? When would you use it?
8. Can a committed transaction be rolled back?

**Tricky:**
9. ACID mein "C" matlab Consistency hai — but consistency kaun ensure karta hai?
10. What happens to ACID in a distributed database (microservices)?
11. Is every SQL statement a transaction?

**Answers:**
- Q9 → C (Consistency) is ensured by the APPLICATION logic + database constraints (NOT by DBMS alone)
- Q10 → ACID is hard in distributed systems. Use BASE (Basically Available, Soft state, Eventually consistent) or 2-Phase Commit
- Q11 → Yes! Every SQL statement is an implicit transaction (auto-commit mode)

---

### 📝 Module 6 — Revision Notes

```
Transaction = Group of operations (ALL or NONE)

ACID:
  A = Atomicity     → All or Nothing
  C = Consistency   → Valid state → Valid state
  I = Isolation     → Concurrent transactions don't interfere
  D = Durability    → Committed = Permanent

States: Active → Partially Committed → Committed
                              ↓
                           Failed → Aborted

Commands: START TRANSACTION, COMMIT, ROLLBACK, SAVEPOINT
```

---

## Module 7: Concurrency Control

### 🔹 Why Concurrency Control?

Multiple users accessing database simultaneously (like 10,000 people booking movie tickets at once). Without control → **chaos**.

---

### 🔹 Problems Without Concurrency Control

#### 1. Dirty Read
Reading **uncommitted** (dirty) data from another transaction.

```
T1: UPDATE balance = 5000  (not committed yet)
T2: READ balance → gets 5000 (dirty read!)
T1: ROLLBACK → balance goes back to original
T2: worked with wrong data! 💀
```

#### 2. Non-Repeatable Read
Same query gives **different results** within same transaction because another transaction modified data.

```
T1: READ balance → 10000
T2: UPDATE balance = 7000, COMMIT
T1: READ balance again → 7000 (different! 😱)
```

#### 3. Phantom Read
Same query gives **different rows** because another transaction inserted/deleted rows.

```
T1: SELECT * FROM Orders WHERE amount > 1000 → 5 rows
T2: INSERT new order with amount 1500, COMMIT
T1: Same SELECT → 6 rows (phantom row appeared! 👻)
```

#### 4. Lost Update
Two transactions read same value, update it, and one update is lost.

```
T1: Read qty = 10, sets qty = 10 - 2 = 8
T2: Read qty = 10, sets qty = 10 - 3 = 7
T1 COMMIT: qty = 8
T2 COMMIT: qty = 7  (T1's update LOST! actual should be 5) 💀
```

---

### 🔹 Isolation Levels

SQL standard defines 4 isolation levels (tradeoff: isolation vs performance):

| Isolation Level | Dirty Read | Non-Repeatable | Phantom |
|----------------|------------|----------------|---------|
| READ UNCOMMITTED | ✓ Possible | ✓ Possible | ✓ Possible |
| READ COMMITTED | ✗ Prevented | ✓ Possible | ✓ Possible |
| REPEATABLE READ | ✗ Prevented | ✗ Prevented | ✓ Possible |
| SERIALIZABLE | ✗ Prevented | ✗ Prevented | ✗ Prevented |

**Setting isolation level in MySQL:**
```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
START TRANSACTION;
-- ... operations ...
COMMIT;
```

**Which to use?**
- **Banking:** SERIALIZABLE (strongest, slowest)
- **E-commerce orders:** REPEATABLE READ (MySQL default)
- **Analytics/Reports:** READ COMMITTED (PostgreSQL default)
- **Cache updates:** READ UNCOMMITTED (fastest, weakest)

---

### 🔹 Concurrency Control Techniques

#### 1. Lock-Based Protocols
- **Shared Lock (S-Lock):** Multiple reads allowed simultaneously
- **Exclusive Lock (X-Lock):** Only one write, no other reads/writes

#### 2. Timestamp-Based Protocols
Each transaction gets a timestamp. Older timestamp = higher priority.

#### 3. MVCC — Multi-Version Concurrency Control
- Used by PostgreSQL, MySQL InnoDB
- Readers don't block writers, writers don't block readers
- Each transaction sees a **snapshot** of the DB at start time
- Different versions of same row exist simultaneously

```
PostgreSQL MVCC:
Transaction T1 sees data as of time T1_start
Transaction T2 sees data as of time T2_start
Both can run concurrently without locking each other!
```

---

### ⭐ Module 7 — Interview Questions

**Basic:**
1. What is concurrency? Why is it needed?
2. What is a dirty read? Give an example.
3. What are the 4 isolation levels?

**Intermediate:**
4. Difference between dirty read, non-repeatable read, and phantom read?
5. What isolation level prevents phantom reads?
6. What is MVCC? How does PostgreSQL use it?
7. Which isolation level does MySQL use by default?

**Tricky:**
8. Why is SERIALIZABLE rarely used in production?
9. Can two transactions both hold shared locks on same data?
10. How does Instagram handle millions of concurrent likes without conflicts?

**Answers:**
- Q8 → SERIALIZABLE = least concurrency = performance bottleneck. Use only when absolute consistency needed.
- Q9 → YES! Multiple S-locks can coexist. X-lock is exclusive only.
- Q10 → Counter increment with atomic operations + Redis for real-time count + eventual consistency for display

---

### 📝 Module 7 — Revision Notes

```
Concurrency Problems:
  Dirty Read        → reading uncommitted data
  Non-Repeatable    → same query, different results
  Phantom Read      → same query, different rows
  Lost Update       → one transaction's update overwrites another

Isolation Levels (strictest → most lenient):
  SERIALIZABLE      → prevents all problems (slow)
  REPEATABLE READ   → prevents dirty + non-repeatable (MySQL default)
  READ COMMITTED    → prevents dirty read only (PostgreSQL default)
  READ UNCOMMITTED  → prevents nothing (fastest)

MVCC = Readers and writers don't block each other (snapshot-based)
```

---

## Module 8: Locking & Deadlocks

### 🔹 Types of Locks

| Lock Type | Read | Write | Other reads | Other writes |
|-----------|------|-------|-------------|--------------|
| Shared (S) | ✓ | ✗ | ✓ | ✗ |
| Exclusive (X) | ✓ | ✓ | ✗ | ✗ |
| Intent Shared (IS) | ✓ | ✗ | ✓ | ✓ |
| Intent Exclusive (IX) | ✓ | ✓ | ✓ | ✗ |

**Lock Compatibility Matrix:**
```
        S     X
S       ✓     ✗
X       ✗     ✗
```

---

### 🔹 Two-Phase Locking (2PL)

A protocol to ensure serializability.

```
Phase 1 — GROWING: acquire locks, release NONE
Phase 2 — SHRINKING: release locks, acquire NONE

|—growing—|—shrinking—|
 Lock      Unlock
 Lock      Unlock
 Lock      Unlock
```

---

### 🔹 Deadlock

**Deadlock = Two or more transactions wait for each other forever.**

```
T1 holds Lock(A), waiting for Lock(B)
T2 holds Lock(B), waiting for Lock(A)
↑_____________________↑
   Circular wait! Neither can proceed.
```

**Real example:**

```
T1: UPDATE Accounts WHERE id=1 (locks row 1)
    UPDATE Accounts WHERE id=2 (waiting for T2 to release row 2)

T2: UPDATE Accounts WHERE id=2 (locks row 2)
    UPDATE Accounts WHERE id=1 (waiting for T1 to release row 1)

DEADLOCK! ☠️
```

---

### 🔹 Deadlock Handling

#### Prevention
- **Order resources:** Always lock in same order (T1 and T2 must both lock row 1 first, then row 2)
- **Wound-Wait:** Older transaction wounds (kills) younger
- **Wait-Die:** Older waits, younger dies (restarts)

#### Detection
- **Wait-for graph:** If cycle exists → deadlock
- DBMS periodically checks for cycles, kills one transaction

#### Recovery
- **Victim selection:** Kill the transaction that's easiest to restart (least work done)
- **Rollback:** Undo killed transaction

---

### 🔹 Deadlock in MySQL/PostgreSQL

```sql
-- MySQL automatically detects deadlocks
-- Kills the transaction with less work done
-- You'll get: ERROR 1213: Deadlock found when trying to get lock

-- Your application should handle it:
try {
    begin_transaction();
    // ... operations ...
    commit();
} catch (DeadlockException e) {
    rollback();
    retry();  // retry the transaction
}
```

---

### ⭐ Module 8 — Interview Questions

**Basic:**
1. What is a deadlock? Give an example.
2. What is the difference between shared and exclusive locks?
3. How can deadlocks be prevented?

**Intermediate:**
4. What is Two-Phase Locking? What does it guarantee?
5. How does a DBMS detect deadlocks?
6. What is a wait-for graph?

**Tricky:**
7. Can deadlock occur with a single transaction?
8. What is livelock? How is it different from deadlock?
9. How does MySQL handle deadlocks in production?

**Answers:**
- Q7 → No! Deadlock requires minimum 2 transactions in circular wait.
- Q8 → Livelock = both transactions keep retrying/backing off without making progress. Deadlock = both stuck, not retrying. Solution for livelock: random backoff time.

---

### 📝 Module 8 — Revision Notes

```
Locks:
  Shared (S) → read only, multiple allowed
  Exclusive (X) → read+write, no others

Deadlock = Circular wait between transactions
  Prevention: consistent lock ordering, timeout
  Detection: wait-for graph (cycle = deadlock)
  Recovery: kill one transaction (victim), rollback

2PL:
  Growing phase → acquire locks
  Shrinking phase → release locks
  Guarantees serializability
```

---

## Module 9: Indexing

### 🔹 What is an Index?

**Without index:** "Find all orders from user_id = 5" → scan EVERY row (full table scan).

**With index:** Jump directly to user_id = 5 rows. Like a book's index!

> Index = Special data structure that speeds up data retrieval

**Analogy:**  
Phone book mein directly page pe jaate ho by name (index). Bina index ke har page check karna padta.

---

### 🔹 How Index Works Internally

#### B-Tree Index (Most Common)

```
Balanced Tree structure:

                [50]
               /    \
           [25]      [75]
          /    \    /    \
       [10]  [30] [60]  [90]
```

- **B-Tree** (Binary): each node has 2 children
- **B+Tree** (most DBs use this): data stored only at leaf nodes, internal nodes for navigation

```
B+Tree for user_id index:

Internal:  [10 | 20 | 30]
Leaf:  [1,2,3,4,5,6,7,8,9,10] → [11..20] → [21..30] → ...
         ↓                         ↓            ↓
      Row pointers            Row pointers   Row pointers
```

**Properties:**
- Search: O(log n)
- Supports range queries (BETWEEN, >, <)
- Supports ORDER BY efficiently

#### Hash Index

```
hash(user_id) → bucket_location

hash(1)  → Bucket 3  → [row pointer for user_id=1]
hash(5)  → Bucket 7  → [row pointer for user_id=5]
hash(99) → Bucket 2  → [row pointer for user_id=99]
```

**Properties:**
- Search: O(1) average
- Only equality queries (=), NO range queries
- Used in: hash joins, in-memory tables

---

### 🔹 Clustered vs Non-Clustered Index

#### Clustered Index
- **Data is physically stored** in the order of the index
- Only **ONE** per table (data can only be sorted one way)
- Primary Key is usually clustered by default

```
Table data on disk (sorted by user_id = clustered index on user_id):
[user_id=1 data] [user_id=2 data] [user_id=3 data] ...
```

#### Non-Clustered Index
- Separate structure that **points to** data
- Can have **MANY** per table
- Contains index column value + pointer to actual row

```
Non-clustered index on email:

email                    → row_pointer
abc@gmail.com           → [page 3, slot 5]
rahul@gmail.com         → [page 7, slot 2]
zara@gmail.com          → [page 1, slot 8]
```

---

### 🔹 Types of Indexes

```sql
-- Single Column Index
CREATE INDEX idx_user_email ON Users(email);

-- Composite (Multi-column) Index
CREATE INDEX idx_order_user_date ON Orders(user_id, created_date);
-- Useful for queries: WHERE user_id=X AND created_date > Y

-- Unique Index (data must be unique)
CREATE UNIQUE INDEX idx_username ON Users(username);

-- Full-text Index (for text search)
CREATE FULLTEXT INDEX idx_post_caption ON Posts(caption);
-- Used for: SELECT * FROM Posts WHERE MATCH(caption) AGAINST('beach')

-- Partial/Filtered Index (PostgreSQL)
CREATE INDEX idx_active_users ON Users(email) WHERE is_active = TRUE;
```

---

### 🔹 When to Use (and NOT Use) Indexes

**USE Index when:**
- Column is frequently used in WHERE, JOIN, ORDER BY, GROUP BY
- Column has high cardinality (many distinct values)
- Table is large (100K+ rows)

**AVOID Index when:**
- Table is small
- Column is rarely queried
- Column has low cardinality (e.g., gender: only M/F)
- Write-heavy tables (indexes slow down INSERT/UPDATE/DELETE)

---

### 🔹 Real-World Indexing Strategy

**Instagram Posts Table:**
```sql
CREATE TABLE Posts (
    post_id    BIGINT PRIMARY KEY,       -- clustered index auto
    user_id    BIGINT NOT NULL,
    caption    TEXT,
    location   VARCHAR(200),
    created_at TIMESTAMP,
    like_count INT DEFAULT 0
);

-- User's posts (feed query)
CREATE INDEX idx_posts_user_created ON Posts(user_id, created_at DESC);

-- Location search
CREATE INDEX idx_posts_location ON Posts(location);

-- Full-text caption search
CREATE FULLTEXT INDEX idx_posts_caption ON Posts(caption);
```

---

### ⭐ Module 9 — Interview Questions

**Basic:**
1. What is an index? Why is it used?
2. What is the difference between clustered and non-clustered index?
3. How many clustered indexes can a table have?

**Intermediate:**
4. How does a B-Tree index work?
5. When should you NOT use an index?
6. What is a composite index? What is the "left prefix rule"?
7. What is the difference between B-Tree and Hash index?

**Tricky:**
8. Does index always improve query performance?
9. You have a query: `SELECT * FROM Orders WHERE status='delivered'`. Should you index `status`?
10. What is index cardinality? Why does it matter?
11. What is a covering index?

**Answers:**
- Q8 → NO! Index has overhead for INSERT/UPDATE/DELETE. For low-cardinality columns, full scan may be faster.
- Q9 → Low cardinality (few values: pending/delivered/cancelled). Index may not help much; full scan might be faster.
- Q11 → Covering index: query can be answered entirely from the index without accessing the actual table rows.

**Left Prefix Rule:**
```
Composite index on (user_id, created_at, status)

Queries that USE this index:
  WHERE user_id = 1                           ✓
  WHERE user_id = 1 AND created_at > X        ✓
  WHERE user_id = 1 AND created_at > X AND status = 'active' ✓

Queries that DON'T use this index:
  WHERE created_at > X                        ✗ (skipped user_id)
  WHERE status = 'active'                     ✗ (skipped both)
```

---

### 📝 Module 9 — Revision Notes

```
Index = Data structure for fast lookups (like a book index)

B+Tree: O(log n), supports =, <, >, BETWEEN, ORDER BY
Hash:   O(1) average, only = queries

Clustered: data physically sorted → 1 per table (usually PK)
Non-clustered: separate pointer structure → many per table

Use index: high cardinality, frequent WHERE/JOIN/ORDER BY
Avoid: low cardinality, write-heavy, small tables

Left prefix rule: composite index (A,B,C) → queries must start with A
Covering index: all needed columns in the index itself (no table lookup)
```

---

## Module 10: Query Optimization

### 🔹 Why Query Optimization?

Same result, different performance:

```sql
-- Query A: Full scan of 1M rows
SELECT * FROM Orders WHERE YEAR(created_at) = 2024;

-- Query B: Uses index, 1000x faster
SELECT * FROM Orders WHERE created_at >= '2024-01-01' AND created_at < '2025-01-01';
```

---

### 🔹 Query Execution Plan

**EXPLAIN** shows you how the database will execute a query.

```sql
EXPLAIN SELECT u.name, COUNT(o.order_id) as order_count
FROM Users u
JOIN Orders o ON u.user_id = o.user_id
WHERE u.created_at > '2024-01-01'
GROUP BY u.user_id;
```

**Output columns to look for:**
- `type`: access method (system > const > ref > range > index > ALL)
  - `ALL` = full table scan = BAD 🔴
  - `ref` or `range` = using index = GOOD 🟢
- `key`: which index is used
- `rows`: estimated rows to scan
- `Extra`: Using filesort, Using temporary = BAD

---

### 🔹 Query Optimizer

**Cost-Based Optimizer (CBO):** DBMS estimates the cost of different execution plans and picks cheapest.

```
Query: SELECT * FROM Orders JOIN Users WHERE city='Mumbai'

Plan A: Scan Orders → join Users (cost: 50000)
Plan B: Scan Users WHERE city='Mumbai' → join Orders (cost: 500)

Optimizer picks Plan B ✓
```

**Optimizer uses:**
- Table statistics (row count, column distributions)
- Index availability
- Join algorithms (Hash Join, Nested Loop, Sort-Merge)

---

### 🔹 Query Optimization Tips

```sql
-- ❌ BAD: Function on indexed column (index not used)
SELECT * FROM Orders WHERE YEAR(created_at) = 2024;
-- ✅ GOOD: Sargable query
SELECT * FROM Orders WHERE created_at BETWEEN '2024-01-01' AND '2024-12-31';

-- ❌ BAD: SELECT * (fetches unnecessary columns)
SELECT * FROM Users;
-- ✅ GOOD: Select only needed columns
SELECT user_id, name, email FROM Users;

-- ❌ BAD: N+1 problem
for each user:
    SELECT orders FROM Orders WHERE user_id = X
-- ✅ GOOD: Single JOIN
SELECT u.name, o.order_id FROM Users u JOIN Orders o ON u.user_id = o.user_id;

-- ❌ BAD: LIKE with leading wildcard (can't use index)
WHERE name LIKE '%rahul%';
-- ✅ BETTER: Leading string (can use index)
WHERE name LIKE 'rahul%';

-- ❌ BAD: OR can prevent index use
WHERE user_id = 1 OR user_id = 2;
-- ✅ GOOD: IN clause
WHERE user_id IN (1, 2);
```

---

### ⭐ Module 10 — Interview Questions

1. What is a query execution plan? How do you view it?
2. What is a full table scan? When does it happen?
3. What is "sargable" query? Give an example.
4. What is the N+1 query problem? How do you fix it?
5. What is query optimizer? Cost-based vs rule-based?

---

### 📝 Module 10 — Revision Notes

```
EXPLAIN → see query execution plan
type: ALL(bad) → range/ref(good) → const(best)

Optimizations:
  - Avoid functions on indexed columns
  - Use sargable predicates (=, >, <, BETWEEN, LIKE 'x%')
  - Select only needed columns (no SELECT *)
  - Fix N+1 with JOINs or batch queries
  - Use covering indexes
  - Avoid leading wildcard in LIKE
```

---

## Module 11: Views, Stored Procedures & Triggers

### 🔹 Views

A **View** is a virtual table based on a SELECT query. It doesn't store data itself.

```sql
-- Create a view
CREATE VIEW user_order_summary AS
SELECT 
    u.user_id, u.name, u.email,
    COUNT(o.order_id) AS total_orders,
    SUM(o.amount) AS total_spent
FROM Users u
LEFT JOIN Orders o ON u.user_id = o.user_id
GROUP BY u.user_id;

-- Use the view like a table
SELECT * FROM user_order_summary WHERE total_spent > 10000;

-- Update view (if updatable)
UPDATE user_order_summary SET name = 'Rahul Sharma' WHERE user_id = 1;
```

**Benefits:**
- Simplify complex queries
- Security (hide sensitive columns)
- Data abstraction

**Materialized View:**
- Actually stores the query result
- Much faster for complex aggregations
- Must be refreshed periodically
- Used in: data warehouses, reporting

---

### 🔹 Stored Procedures

A **Stored Procedure** is a precompiled set of SQL statements stored in the database.

```sql
-- Create stored procedure
DELIMITER //
CREATE PROCEDURE transfer_money(
    IN from_account INT,
    IN to_account INT,
    IN amount DECIMAL(10,2)
)
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Transfer failed';
    END;
    
    START TRANSACTION;
        UPDATE Accounts SET balance = balance - amount WHERE id = from_account;
        UPDATE Accounts SET balance = balance + amount WHERE id = to_account;
    COMMIT;
END //
DELIMITER ;

-- Call stored procedure
CALL transfer_money(1, 2, 5000.00);
```

**Benefits:**
- Reduced network traffic (send one call, not 10 queries)
- Reusable business logic
- Better security (users call SP, don't access tables directly)
- Performance (precompiled)

---

### 🔹 Triggers

A **Trigger** automatically executes when a specific event occurs.

```sql
-- Trigger: log every order deletion
CREATE TRIGGER after_order_delete
AFTER DELETE ON Orders
FOR EACH ROW
BEGIN
    INSERT INTO Order_Audit_Log (order_id, deleted_at, deleted_by)
    VALUES (OLD.order_id, NOW(), CURRENT_USER());
END;

-- Trigger: update product stock when order placed
CREATE TRIGGER after_orderitem_insert
AFTER INSERT ON OrderItems
FOR EACH ROW
BEGIN
    UPDATE Products 
    SET stock = stock - NEW.quantity 
    WHERE product_id = NEW.product_id;
END;
```

**Types of Triggers:**
- BEFORE / AFTER
- INSERT / UPDATE / DELETE

**When to use triggers:**
- Audit logging
- Automatic timestamps
- Data validation
- Cascade updates not covered by FK

**When NOT to use:**
- Complex business logic (hard to debug)
- Performance-critical paths (hidden overhead)

---

### ⭐ Module 11 — Interview Questions

1. What is a view? How is it different from a table?
2. What is a materialized view? When would you use it?
3. What are stored procedures? Advantages over application-level queries?
4. What is a trigger? Give a real-world use case.
5. Can you UPDATE data through a view?
6. What is the difference between BEFORE and AFTER triggers?

---

### 📝 Module 11 — Revision Notes

```
View: Virtual table (no data stored), based on SELECT query
  Updatable: if based on single table, no aggregations
  Materialized: stores result physically, needs refresh

Stored Procedure: Precompiled SQL stored in DB
  Benefits: reusable, less network calls, security, performance

Trigger: Auto-execute on INSERT/UPDATE/DELETE
  BEFORE: validate/modify before operation
  AFTER: log/cascade after operation
  Use for: audit logs, auto-updates, validations
```

---

## Module 12: DB Architecture

### 🔹 3-Schema Architecture (ANSI/SPARC)

Three levels of data abstraction:

```
                USER LEVEL
                ↓
    ┌───────────────────────────┐
    │   EXTERNAL SCHEMA         │  ← What user sees (views, APIs)
    │   (multiple user views)   │  ← Different users see different data
    └───────────┬───────────────┘
                │ logical mapping
    ┌───────────┴───────────────┐
    │   CONCEPTUAL SCHEMA       │  ← Logical structure of entire DB
    │   (all tables, keys, FKs) │  ← ER model → tables
    └───────────┬───────────────┘
                │ physical mapping
    ┌───────────┴───────────────┐
    │   INTERNAL SCHEMA         │  ← Physical storage details
    │   (files, indexes, pages) │  ← How data is actually stored on disk
    └───────────────────────────┘
```

**Benefits:**
- **Data Independence:** Change internal storage → doesn't affect users
- **Security:** Users only see their relevant data
- **Abstraction:** Hide complexity

---

### 🔹 OLTP vs OLAP

| Feature | OLTP | OLAP |
|---------|------|------|
| Full Name | Online Transaction Processing | Online Analytical Processing |
| Purpose | Day-to-day operations | Business intelligence, analytics |
| Operations | INSERT, UPDATE, DELETE | Complex SELECT queries |
| Data Size | Small transactions | Huge datasets |
| Response Time | Milliseconds | Seconds to minutes |
| Schema | Normalized (3NF) | Denormalized (Star/Snowflake) |
| Users | Thousands concurrent | Few analysts |
| Examples | Order booking, banking | Sales reports, dashboards |
| Database | MySQL, PostgreSQL | BigQuery, Redshift, Snowflake |

**Real Example:**
- **Zomato OLTP:** Placing an order, updating order status, adding a review
- **Zomato OLAP:** "Which cities had highest orders last quarter?" "What's the trend of cancellations?"

---

### 🔹 Data Warehouse Architecture

```
[OLTP DBs] → ETL Process → [Data Warehouse] → [OLAP Queries] → [Dashboards]

ETL = Extract, Transform, Load

Star Schema (Data Warehouse):
                [Date Dimension]
                      |
[Customer Dim] — [Fact Table] — [Product Dim]
                      |
               [Location Dim]

Fact Table: contains metrics (sales_amount, quantity)
Dimension Tables: descriptive data (who, what, when, where)
```

---

### ⭐ Module 12 — Interview Questions

1. Explain the 3-schema architecture.
2. What is data independence? Types?
3. Difference between OLTP and OLAP?
4. What is a data warehouse? How is it different from a regular database?
5. What is ETL?
6. What is a star schema vs snowflake schema?

---

### 📝 Module 12 — Revision Notes

```
3-Schema:
  External → user views
  Conceptual → logical structure (tables, keys)
  Internal → physical storage

Data Independence:
  Logical → change conceptual without changing external
  Physical → change internal without changing conceptual

OLTP: operational (fast, normalized, many users)
OLAP: analytical (complex queries, denormalized, few users)

Data Warehouse: centralized store for analytics
ETL: Extract → Transform → Load into warehouse
Star Schema: fact table surrounded by dimension tables
```

---

## Module 13: NoSQL Basics

### 🔹 SQL vs NoSQL

| Feature | SQL (Relational) | NoSQL |
|---------|-----------------|-------|
| Schema | Fixed (rigid) | Flexible (dynamic) |
| Data Model | Tables | Document, K-V, Column, Graph |
| Scalability | Vertical (bigger server) | Horizontal (more servers) |
| ACID | Full | Eventual consistency (BASE) |
| Joins | Yes | No (embed or reference) |
| Examples | MySQL, PostgreSQL | MongoDB, Redis, Cassandra |
| Best for | Structured data, transactions | Unstructured, high scale |

**BASE (NoSQL alternative to ACID):**
- **B**asically Available
- **S**oft state
- **E**ventually Consistent

---

### 🔹 MongoDB Basics

MongoDB stores data as **BSON documents** (like JSON).

```javascript
// SQL Row → MongoDB Document
// SQL Table → MongoDB Collection

// Insert a document
db.users.insertOne({
  _id: ObjectId("..."),  // Primary key (auto-generated)
  username: "rahul_dev",
  email: "rahul@gmail.com",
  posts: [               // Embedded array (no JOIN needed!)
    { post_id: 1, caption: "My first post", likes: 150 },
    { post_id: 2, caption: "Sunset photo", likes: 300 }
  ],
  address: {             // Embedded document (composite attribute)
    city: "Bangalore",
    state: "Karnataka"
  },
  created_at: new Date()
});

// Find documents
db.users.find({ "address.city": "Bangalore" });
db.users.find({ "posts.likes": { $gt: 100 } });

// Aggregation (like GROUP BY)
db.orders.aggregate([
  { $match: { status: "delivered" } },
  { $group: { _id: "$restaurant_id", total: { $sum: "$amount" } } },
  { $sort: { total: -1 } }
]);
```

---

### 🔹 When to Choose SQL vs NoSQL

**Choose SQL (PostgreSQL/MySQL):**
- Banking, payments, financial data
- Data has clear relationships
- ACID required
- Complex JOIN queries
- Data structure is well-defined

**Choose MongoDB:**
- Product catalogs (attributes vary by category)
- Content management, blogs
- User profiles with flexible fields
- Rapid prototyping

**Choose Redis:**
- Session storage
- Caching
- Real-time leaderboards
- Rate limiting

**Choose Cassandra:**
- Time-series data (IoT, logs, metrics)
- Write-heavy workloads
- Need geographic distribution
- Order tracking, event logs

---

### ⭐ Module 13 — Interview Questions

1. When would you choose NoSQL over SQL?
2. What are the types of NoSQL databases?
3. What is the CAP theorem?
4. What is eventual consistency?
5. How does MongoDB handle relationships (no JOINs)?
6. What is the difference between embedding and referencing in MongoDB?
7. Why is Redis so fast?

**CAP Theorem:**
```
CAP = Consistency + Availability + Partition Tolerance
You can only guarantee 2 out of 3:

CA systems: MySQL (no partition tolerance)
CP systems: MongoDB, HBase (consistent, not always available)
AP systems: Cassandra, DynamoDB (available, eventually consistent)
```

---

### 📝 Module 13 — Revision Notes

```
NoSQL types:
  Document: MongoDB (JSON-like, flexible schema)
  Key-Value: Redis (ultra-fast, in-memory)
  Column: Cassandra (write-heavy, distributed)
  Graph: Neo4j (relationships, social networks)

SQL → ACID (strong consistency)
NoSQL → BASE (eventual consistency)

CAP: pick 2 of 3
  C = Consistency
  A = Availability  
  P = Partition Tolerance

MongoDB: embedded docs for 1:M, references for M:N
Redis: in-memory → O(1) operations → super fast
```

---

## Module 14: Backend Connection

### 🔹 How DBMS is Used in Real Backend

**Architecture:**
```
Client (React/Mobile App)
        ↓ HTTP Request
    [Backend API Server]  ← Node.js/Python/Java/Go
        ↓ SQL Query
    [Database]  ← PostgreSQL/MySQL
        ↓ Result
    [Backend API Server]
        ↓ JSON Response
Client (React/Mobile App)
```

---

### 🔹 Connection Pooling (CRITICAL for Production)

```javascript
// BAD: New connection for each request (slow, resource waste)
async function getUser(id) {
    const conn = await mysql.createConnection(config);  // expensive!
    const [rows] = await conn.execute('SELECT * FROM users WHERE id = ?', [id]);
    await conn.end();
    return rows;
}

// GOOD: Connection Pool (reuse connections)
const pool = mysql.createPool({
    host: 'localhost',
    user: 'root',
    database: 'myapp',
    connectionLimit: 10  // max 10 connections
});

async function getUser(id) {
    const [rows] = await pool.execute('SELECT * FROM users WHERE id = ?', [id]);
    return rows[0];
}
```

---

### 🔹 ORM vs Raw SQL

**ORM (Object-Relational Mapper):** Write code, it generates SQL.

```javascript
// Sequelize ORM (Node.js)
const user = await User.findOne({
    where: { email: 'rahul@gmail.com' },
    include: [{ model: Order, as: 'orders' }]
});
// Generates: SELECT users.*, orders.* FROM users LEFT JOIN orders ON ...

// Raw SQL equivalent
const [rows] = await pool.execute(`
    SELECT u.*, o.order_id, o.amount 
    FROM users u 
    LEFT JOIN orders o ON u.user_id = o.user_id 
    WHERE u.email = ?
`, ['rahul@gmail.com']);
```

**ORM pros:** Faster development, prevents SQL injection, database-agnostic  
**ORM cons:** Performance (generates suboptimal SQL), limited control  
**Raw SQL pros:** Full control, performance optimization  
**Raw SQL cons:** More code, SQL injection risk if not careful

---

### 🔹 Preventing SQL Injection

```javascript
// ❌ DANGEROUS: SQL Injection vulnerable
const userId = req.params.id;  // attacker sends: "1 OR 1=1"
const query = `SELECT * FROM users WHERE id = ${userId}`;
// Becomes: SELECT * FROM users WHERE id = 1 OR 1=1 → returns ALL users!

// ✅ SAFE: Parameterized Query
const [rows] = await pool.execute(
    'SELECT * FROM users WHERE id = ?',
    [userId]  // sanitized automatically
);
```

---

### 🔹 Database Optimization in Real Apps

```
1. Connection Pooling → reuse connections
2. Indexing → speed up frequent queries
3. Caching (Redis) → cache frequently read data
4. Read Replicas → separate read/write servers
5. Sharding → split data across multiple DBs
6. Pagination → don't return all rows at once

-- Pagination example
SELECT * FROM posts ORDER BY created_at DESC LIMIT 20 OFFSET 40;
-- Or better (cursor-based):
SELECT * FROM posts WHERE post_id < last_cursor ORDER BY post_id DESC LIMIT 20;
```

---

### 🔹 Caching with Redis

```javascript
const redis = require('redis');
const client = redis.createClient();

async function getUserProfile(userId) {
    // Check cache first
    const cached = await client.get(`user:${userId}`);
    if (cached) return JSON.parse(cached);  // cache hit!
    
    // Cache miss → query DB
    const [rows] = await pool.execute('SELECT * FROM users WHERE id = ?', [userId]);
    const user = rows[0];
    
    // Store in cache for 1 hour
    await client.setEx(`user:${userId}`, 3600, JSON.stringify(user));
    return user;
}
```

---

### ⭐ Module 14 — Interview Questions

1. What is connection pooling? Why is it important?
2. What is SQL injection? How do you prevent it?
3. What is an ORM? Advantages and disadvantages?
4. How would you handle 1 million concurrent users on your database?
5. What is read replica? When do you use it?
6. How does caching improve database performance?
7. What is database sharding?

---

### 📝 Module 14 — Revision Notes

```
Backend → DB connection flow:
  App → Connection Pool → DB (reuse connections)

SQL Injection: use parameterized queries always!

ORM: Code → SQL (convenience) vs Raw SQL (performance)

Scaling DB:
  Vertical: bigger server (limited)
  Horizontal: read replicas, sharding

Caching: Redis in front of DB (read from cache first)
Cache invalidation: update cache when data changes

Sharding: split data by userId%10 → 10 DB servers
Read Replica: write → master, read → replica
```

---

## Final Preparation & Mock Interview

### 🎯 Top 50 DBMS Interview Questions (Company Level)

#### Database Basics (5 Qs)
1. What is the difference between DBMS and RDBMS?
2. What are the types of databases? When would you choose NoSQL?
3. What is data redundancy and data integrity?
4. Explain the concept of schema.
5. What is metadata?

#### Keys & Constraints (8 Qs)
6. Explain all types of keys with examples.
7. Can a table have multiple unique constraints?
8. Can a foreign key be NULL? Can it reference a non-PK column?
9. What is referential integrity? ON DELETE CASCADE vs RESTRICT?
10. What is the difference between primary key and composite key?
11. Can two tables have a foreign key referencing each other?
12. What is a natural key vs surrogate key?
13. What happens when you insert a duplicate primary key?

#### Normalization (8 Qs)
14. Explain 1NF, 2NF, 3NF with examples.
15. What is BCNF? How is it different from 3NF?
16. What is denormalization? When is it used?
17. What is functional dependency?
18. What is partial dependency?
19. What is transitive dependency?
20. Normalize this table: [give scenario]
21. Is it always good to normalize to BCNF?

#### SQL (10 Qs)
22. Difference between WHERE and HAVING?
23. Explain all types of JOINs.
24. What is a subquery? Correlated vs non-correlated?
25. What are window functions? Give example.
26. Write query for Nth highest salary.
27. Find duplicate records in a table.
28. What is UNION vs UNION ALL?
29. What is a CTE? Advantages over subquery?
30. Difference between DELETE, TRUNCATE, DROP?
31. What is a self JOIN? Give example.

#### Transactions & Concurrency (8 Qs)
32. Explain ACID properties with examples.
33. What is a dirty read? How to prevent it?
34. What are the 4 isolation levels? Tradeoffs?
35. What is a deadlock? How to detect and resolve?
36. What is Two-Phase Locking?
37. What is MVCC? Which databases use it?
38. What is a phantom read?
39. Can you have ACID in a distributed database?

#### Indexing (6 Qs)
40. How does B-Tree index work?
41. Clustered vs non-clustered index?
42. What is the left prefix rule for composite indexes?
43. When should you NOT create an index?
44. What is a covering index?
45. How does EXPLAIN work? What to look for?

#### Architecture & Advanced (5 Qs)
46. OLTP vs OLAP?
47. What is a stored procedure vs function?
48. What is a trigger? When would you use it?
49. What is the CAP theorem?
50. How would you design the database for Twitter?

---

### 🏗️ System Design DB Questions (Product Companies)

**Design the database for:**

1. **WhatsApp**
   - Users, Messages, Groups, Group Members
   - How to store media? (S3 + URL in DB)
   - Read receipt at scale?

2. **Zomato/Swiggy**
   - Users, Restaurants, Menus, Orders, Delivery, Reviews
   - Real-time order tracking?
   - Search by location?

3. **Netflix**
   - Users, Movies, Episodes, Subscriptions, Watchlist, Ratings
   - Recommendation data?
   - Handle different subscription tiers?

4. **Amazon**
   - Users, Products, Inventory, Orders, Payments, Reviews, Sellers
   - Prevent overselling (concurrency!)?
   - Search at scale?

---

### ⚡ Rapid Revision Checklist

```
□ Database basics + DBMS vs RDBMS
□ ER Model components (entity, attribute, relationship)
□ Cardinality (1:1, 1:M, M:N) and table conversion
□ All key types (super, candidate, primary, foreign, composite)
□ Normalization (1NF, 2NF, 3NF, BCNF) with examples
□ SQL: SELECT, JOIN types, GROUP BY, HAVING, Subqueries
□ ACID properties with real examples
□ Isolation levels and concurrency problems
□ Deadlock: detection, prevention, recovery
□ Indexing: B-Tree, Hash, Clustered, Non-clustered
□ EXPLAIN command and query optimization
□ Views, Stored Procedures, Triggers
□ OLTP vs OLAP
□ NoSQL types + CAP theorem
□ Connection pooling + caching basics
```

---

### ❌ Common Mistakes to Avoid

1. **Confusing Super Key and Candidate Key** → Super Key has extra cols, Candidate is minimal
2. **Saying table can have multiple primary keys** → Only ONE primary key (but can have multiple unique keys)
3. **Mixing up 2NF and 3NF** → 2NF removes partial dependency, 3NF removes transitive
4. **Not knowing BCNF vs 3NF difference** → BCNF: every determinant must be super key
5. **Forgetting that TRUNCATE can't be rolled back** (in most databases)
6. **Not knowing that FK can be NULL** → unless explicitly set NOT NULL
7. **Confusing dirty read with non-repeatable read** → dirty = uncommitted data; non-repeatable = committed but changed between reads
8. **Saying index always improves performance** → Not for write-heavy, low-cardinality columns
9. **Not knowing left prefix rule** for composite indexes
10. **Confusing HAVING and WHERE** → WHERE = before GROUP, HAVING = after GROUP

---

### 🎤 Mock Interview (Practice These Out Loud)

**Round 1 — Conceptual:**
- "Explain ACID properties with a real-world example."
- "What is normalization? Walk me through 1NF to BCNF."
- "What is the difference between clustered and non-clustered index?"

**Round 2 — SQL:**
- "Write a query to find all users who placed more than 3 orders in the last 30 days."
- "Find the top 3 most ordered products from the Orders table."
- "Write a query to find employees who earn more than the average salary of their department."

**Round 3 — Design:**
- "Design the database for a food delivery app like Swiggy."
- "How would you handle 10 million concurrent users on your database?"
- "User ne ek product 2 different devices se same time pe order kar diya, stock sirf 1 hai. Kaise handle karoge?"

---

## 🎯 Final Tips

> **Consistency is key.** Roz ek topic revise karo.

**Study Plan:**
- Week 1: Basics + ER Model + Keys + Normalization
- Week 2: SQL (practice daily on LeetCode SQL problems)
- Week 3: Transactions + Concurrency + Indexing
- Week 4: Advanced + NoSQL + System Design + Mock Interviews

**Practice Resources:**
- LeetCode SQL Problems (Easy → Hard)
- HackerRank SQL
- SQLZoo.net (interactive)
- Mode Analytics SQL Tutorial

**Always remember in interviews:**
1. Think out loud
2. Ask clarifying questions
3. Start with simple solution, then optimize
4. Use real-world examples in your answers
5. If you don't know → say "I'm not 100% sure, but I think..." (don't bluff)

---

*📌 This guide covers everything from 0 to interview-ready. Revise each module, practice SQL daily, and you'll be placement-ready!*

*🚀 All the best — Tu kar sakta hai!*
