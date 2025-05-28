# **Advance_level_SQL**
➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖
# 11. Advanced Filtering and Expressions

 - CASE Statements
 - COALESCE
 - NULLIF
 - IF and IFNULL

---

### 🧾 **Sample Table: `Employees`**

| EmpID | Name    | Department | Salary | Bonus | ManagerID |
| ----- | ------- | ---------- | ------ | ----- | --------- |
| 1     | Alice   | HR         | 50000  | 5000  | NULL      |
| 2     | Bob     | Finance    | 60000  | NULL  | 1         |
| 3     | Charlie | IT         | 70000  | 7000  | 1         |
| 4     | David   | HR         | NULL   | 4000  | 2         |
| 5     | Eva     | IT         | 75000  | NULL  | 3         |

---

## 1️⃣ **CASE Statement**

### 📌 Query:

```sql
SELECT Name, Department, 
  CASE 
    WHEN Salary >= 70000 THEN 'High'
    WHEN Salary >= 50000 THEN 'Medium'
    ELSE 'Low'
  END AS Salary_Level
FROM Employees;
```

### 📤 Output:

| Name    | Department | Salary\_Level |
| ------- | ---------- | ------------- |
| Alice   | HR         | Medium        |
| Bob     | Finance    | Medium        |
| Charlie | IT         | High          |
| David   | HR         | Low           |
| Eva     | IT         | High          |

---

## 2️⃣ **COALESCE**

> `COALESCE(value1, value2, ...)` returns the **first non-NULL** value.

### 📌 Query:

```sql
SELECT Name, COALESCE(Bonus, 0) AS FinalBonus
FROM Employees;
```

### 📤 Output:

| Name    | FinalBonus |
| ------- | ---------- |
| Alice   | 5000       |
| Bob     | 0          |
| Charlie | 7000       |
| David   | 4000       |
| Eva     | 0          |

---

## 3️⃣ **NULLIF**

> `NULLIF(value1, value2)` returns **NULL** if the two values are equal, otherwise returns `value1`.

### 📌 Query:

```sql
SELECT Name, Salary, NULLIF(Salary, 50000) AS NullIfSalary
FROM Employees;
```

### 📤 Output:

| Name    | Salary | NullIfSalary |
| ------- | ------ | ------------ |
| Alice   | 50000  | NULL         |
| Bob     | 60000  | 60000        |
| Charlie | 70000  | 70000        |
| David   | NULL   | NULL         |
| Eva     | 75000  | 75000        |

---

## 4️⃣ **IF and IFNULL**

> * `IF(condition, true_value, false_value)`
> * `IFNULL(value, replacement)` returns `replacement` if `value` is NULL.

### 📌 Query with `IF`:

```sql
SELECT Name, Salary,
  IF(Salary > 60000, 'Eligible', 'Not Eligible') AS BonusStatus
FROM Employees;
```

### 📤 Output:

| Name    | Salary | BonusStatus  |
| ------- | ------ | ------------ |
| Alice   | 50000  | Not Eligible |
| Bob     | 60000  | Not Eligible |
| Charlie | 70000  | Eligible     |
| David   | NULL   | Not Eligible |
| Eva     | 75000  | Eligible     |

---

### 📌 Query with `IFNULL`:

```sql
SELECT Name, IFNULL(Bonus, 1000) AS AdjustedBonus
FROM Employees;
```

### 📤 Output:

| Name    | AdjustedBonus |
| ------- | ------------- |
| Alice   | 5000          |
| Bob     | 1000          |
| Charlie | 7000          |
| David   | 4000          |
| Eva     | 1000          |


---
➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖
---

# 12. Window Functions (Analytic Functions)
----

## 1️⃣ **ROW\_NUMBER()**

> Assigns a unique sequential number to rows within a partition.

### 📌 Query:

```sql
SELECT Name, Department, Salary,
  ROW_NUMBER() OVER (PARTITION BY Department ORDER BY Salary DESC) AS RowNum
FROM Employees;
```

### 📤 Output:

| Name    | Department | Salary | RowNum |
| ------- | ---------- | ------ | ------ |
| Alice   | HR         | 50000  | 1      |
| David   | HR         | NULL   | 2      |
| Bob     | Finance    | 60000  | 1      |
| Eva     | IT         | 75000  | 1      |
| Charlie | IT         | 70000  | 2      |

---

## 2️⃣ **RANK() and DENSE\_RANK()**

> `RANK()` leaves gaps for ties.
> `DENSE_RANK()` does **not** leave gaps.

### 📌 Query:

```sql
SELECT Name, Department, Salary,
  RANK() OVER (ORDER BY Salary DESC) AS SalaryRank,
  DENSE_RANK() OVER (ORDER BY Salary DESC) AS DenseSalaryRank
FROM Employees;
```

### 📤 Output:

| Name    | Department | Salary | SalaryRank | DenseSalaryRank |
| ------- | ---------- | ------ | ---------- | --------------- |
| Eva     | IT         | 75000  | 1          | 1               |
| Charlie | IT         | 70000  | 2          | 2               |
| Bob     | Finance    | 60000  | 3          | 3               |
| Alice   | HR         | 50000  | 4          | 4               |
| David   | HR         | NULL   | 5          | 5               |

---

## 3️⃣ **NTILE(n)**

> Divides ordered rows into `n` buckets of roughly equal size.

### 📌 Query (into 2 buckets):

```sql
SELECT Name, Department, Salary,
  NTILE(2) OVER (ORDER BY Salary DESC) AS SalaryTile
FROM Employees;
```

### 📤 Output:

| Name    | Department | Salary | SalaryTile |
| ------- | ---------- | ------ | ---------- |
| Eva     | IT         | 75000  | 1          |
| Charlie | IT         | 70000  | 1          |
| Bob     | Finance    | 60000  | 1          |
| Alice   | HR         | 50000  | 2          |
| David   | HR         | NULL   | 2          |

---

## 4️⃣ **LEAD() / LAG()**

> Accesses the next/previous row value.

### 📌 Query:

```sql
SELECT Name, Salary,
  LAG(Salary) OVER (ORDER BY Salary) AS PrevSalary,
  LEAD(Salary) OVER (ORDER BY Salary) AS NextSalary
FROM Employees;
```

### 📤 Output:

| Name    | Salary | PrevSalary | NextSalary |
| ------- | ------ | ---------- | ---------- |
| David   | NULL   | NULL       | Alice      |
| Alice   | 50000  | NULL       | Bob        |
| Bob     | 60000  | 50000      | Charlie    |
| Charlie | 70000  | 60000      | Eva        |
| Eva     | 75000  | 70000      | NULL       |

> NULL for David is due to missing salary.

---

## 5️⃣ **FIRST\_VALUE() / LAST\_VALUE()**

> Returns the first or last value in the window frame.

### 📌 Query:

```sql
SELECT Name, Department, Salary,
  FIRST_VALUE(Salary) OVER (PARTITION BY Department ORDER BY Salary DESC) AS FirstSalary,
  LAST_VALUE(Salary) OVER (PARTITION BY Department ORDER BY Salary DESC 
                           ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS LastSalary
FROM Employees;
```

### 📤 Output:

| Name    | Department | Salary | FirstSalary | LastSalary |
| ------- | ---------- | ------ | ----------- | ---------- |
| Alice   | HR         | 50000  | 50000       | NULL       |
| David   | HR         | NULL   | 50000       | NULL       |
| Bob     | Finance    | 60000  | 60000       | 60000      |
| Eva     | IT         | 75000  | 75000       | 70000      |
| Charlie | IT         | 70000  | 75000       | 70000      |

---

## 6️⃣ **PARTITION BY and ORDER BY in Window Functions**

These two clauses are key for organizing how the window functions operate:

* `PARTITION BY` divides the data into groups (like GROUP BY, but without collapsing rows).
* `ORDER BY` defines the order within each group.

You've already seen both used above in `ROW_NUMBER()`, `RANK()`, `FIRST_VALUE()`, etc.

---
➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖
---

# 13. Common Table Expressions (CTEs)

---

## 🔸 **13.1 USING WITH Clause (Non-Recursive CTE)**

> A CTE is a temporary result set used within a `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement.

---

### ✅ Example: Get average salary per department using CTE and then list employees with salary above department average.

### 📌 Query:

```sql
WITH DeptAvg AS (
  SELECT Department, AVG(Salary) AS AvgSalary
  FROM Employees
  GROUP BY Department
)

SELECT e.Name, e.Department, e.Salary, d.AvgSalary
FROM Employees e
JOIN DeptAvg d ON e.Department = d.Department
WHERE e.Salary > d.AvgSalary;
```

### 📤 Output:

| Name | Department | Salary | AvgSalary |
| ---- | ---------- | ------ | --------- |
| Eva  | IT         | 75000  | 72500     |

---

## 🔸 **13.2 Recursive CTEs**

> Recursive CTEs are used to work with **hierarchical data** like **employee-manager relationships**.

---

### ✅ Example: Find the **hierarchy** under ManagerID = 1 (Alice).

### 📌 Query:

```sql
WITH RECURSIVE EmployeeHierarchy AS (
  -- Anchor member
  SELECT EmpID, Name, ManagerID, 1 AS Level
  FROM Employees
  WHERE ManagerID IS NULL

  UNION ALL

  -- Recursive member
  SELECT e.EmpID, e.Name, e.ManagerID, h.Level + 1
  FROM Employees e
  JOIN EmployeeHierarchy h ON e.ManagerID = h.EmpID
)

SELECT * FROM EmployeeHierarchy;
```

### 📤 Output:

| EmpID | Name    | ManagerID | Level |
| ----- | ------- | --------- | ----- |
| 1     | Alice   | NULL      | 1     |
| 2     | Bob     | 1         | 2     |
| 3     | Charlie | 1         | 2     |
| 4     | David   | 2         | 3     |
| 5     | Eva     | 3         | 3     |

> This gives you a clear **org chart-style** hierarchy.

---

## ✅ Summary of Key Terms:

| Concept           | Description                                                                |
| ----------------- | -------------------------------------------------------------------------- |
| **CTE**           | Temporary result set defined using `WITH`. Helps organize complex queries. |
| **Recursive CTE** | CTE that calls itself to work on hierarchical or tree-structured data.     |
| **WITH Clause**   | Starts the definition of a CTE. Can be used multiple times in a query.     |

---
➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖
-----

# 14. Data Types and Constraints
---

## ✅ **A. Data Types**

| Data Type    | Description            | Example            |
| ------------ | ---------------------- | ------------------ |
| `INT`        | Whole numbers          | `EmpID INT`        |
| `FLOAT`      | Decimal numbers        | `Salary FLOAT`     |
| `VARCHAR(n)` | Variable-length string | `Name VARCHAR(50)` |
| `DATE`       | Calendar date          | `HireDate DATE`    |

---

### ✅ **B. Constraints**

Constraints are rules applied to table columns to ensure **data integrity**.

| Constraint    | Description                                       |
| ------------- | ------------------------------------------------- |
| `PRIMARY KEY` | Uniquely identifies each record (cannot be NULL). |
| `FOREIGN KEY` | Links to a `PRIMARY KEY` in another table.        |
| `NOT NULL`    | Ensures a column cannot store NULL values.        |
| `UNIQUE`      | Ensures all values in a column are different.     |
| `DEFAULT`     | Assigns a default value if none is provided.      |
| `CHECK`       | Validates values against a condition.             |

---

## 🧾 Sample Table Creation Example:

```sql
CREATE TABLE Employees (
  EmpID INT PRIMARY KEY,
  Name VARCHAR(50) NOT NULL,
  Department VARCHAR(50),
  Salary FLOAT CHECK (Salary >= 0),
  Bonus FLOAT DEFAULT 0,
  HireDate DATE NOT NULL
);
```

> 🔹 This table:

* Ensures `EmpID` is unique and cannot be NULL.
* `Name` and `HireDate` must have values.
* `Bonus` will be 0 if not entered.
* `Salary` must be non-negative.

---

## 🧾 Second Table for FOREIGN KEY Constraint:

```sql
CREATE TABLE Departments (
  DeptID INT PRIMARY KEY,
  DeptName VARCHAR(50) UNIQUE
);

ALTER TABLE Employees
ADD DeptID INT,
ADD CONSTRAINT FK_Dept FOREIGN KEY (DeptID) REFERENCES Departments(DeptID);
```

---

## 🧪 INSERT Example:

```sql
INSERT INTO Departments (DeptID, DeptName)
VALUES (1, 'HR'), (2, 'Finance'), (3, 'IT');

INSERT INTO Employees (EmpID, Name, Department, Salary, Bonus, HireDate, DeptID)
VALUES (101, 'Alice', 'HR', 50000, 5000, '2022-01-01', 1);
```

---

## 🔎 Summary Table of Concepts:

| SQL Element   | Example                   | Meaning                  |
| ------------- | ------------------------- | ------------------------ |
| `INT`         | `EmpID INT`               | Integer ID               |
| `VARCHAR(50)` | `Name VARCHAR(50)`        | Up to 50 characters      |
| `PRIMARY KEY` | `EmpID INT PRIMARY KEY`   | Unique identifier        |
| `FOREIGN KEY` | `FOREIGN KEY (DeptID)`    | Relates to another table |
| `NOT NULL`    | `Name VARCHAR NOT NULL`   | Cannot be empty          |
| `UNIQUE`      | `DeptName VARCHAR UNIQUE` | No duplicates            |
| `DEFAULT`     | `Bonus FLOAT DEFAULT 0`   | Sets value if none given |
| `CHECK`       | `CHECK (Salary >= 0)`     | Ensures valid range      |

---
➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖
----

# 15. DDL (Data Definition Language)
---

## ✅ 1. `CREATE` – Creates tables, views, or indexes

### 📌 Example: Create a table

```sql
CREATE TABLE Employees (
  EmpID INT PRIMARY KEY,
  Name VARCHAR(50),
  Salary FLOAT,
  Department VARCHAR(30)
);
```

### 📌 Example: Create a view

```sql
CREATE VIEW HighEarners AS
SELECT Name, Salary FROM Employees
WHERE Salary > 60000;
```

### 📌 Example: Create an index

```sql
CREATE INDEX idx_department ON Employees(Department);
```

---

## ✅ 2. `ALTER` – Modifies an existing table or view

### 📌 Add a new column

```sql
ALTER TABLE Employees
ADD HireDate DATE;
```

### 📌 Modify column data type

```sql
ALTER TABLE Employees
MODIFY Salary DECIMAL(10, 2);
```

### 📌 Rename column (syntax may vary by RDBMS)

```sql
ALTER TABLE Employees
RENAME COLUMN Name TO FullName;
```

---

## ✅ 3. `DROP` – Deletes tables, views, or indexes (permanently)

### 📌 Drop a table

```sql
DROP TABLE Employees;
```

### 📌 Drop a view

```sql
DROP VIEW HighEarners;
```

### 📌 Drop an index

```sql
DROP INDEX idx_department ON Employees;
```

> ⚠️ **Note**: `DROP` permanently deletes the object and its data.

---

## ✅ 4. `TRUNCATE` – Removes **all records** from a table but **keeps the structure**

### 📌 Example:

```sql
TRUNCATE TABLE Employees;
```

> 🔸 Faster than `DELETE` because it doesn’t log individual row deletions.
> 🔸 Cannot be rolled back in some databases.

---

## 📌 Summary Table of DDL Commands

| Command    | Purpose                               | Reversible?   |
| ---------- | ------------------------------------- | ------------- |
| `CREATE`   | Define new table, view, or index      | ❌ No rollback |
| `ALTER`    | Change table/view structure           | ❌ Depends     |
| `DROP`     | Delete structure and data permanently | ❌ No rollback |
| `TRUNCATE` | Remove all data (keep structure)      | ❌ No rollback |

---
➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖
----

# 16. DML (Data Manipulation Language)
---

## ✅ 1. `INSERT` — Add new rows to a table

```sql
INSERT INTO Employees (EmpID, Name, Salary, Department)
VALUES (101, 'Alice', 50000, 'HR'),
       (102, 'Bob', 60000, 'Finance'),
       (103, 'Charlie', 70000, 'IT');
```

**Output:**
Rows with EmpID 101, 102, and 103 added.

---

## ✅ 2. `UPDATE` — Modify existing rows

Example: Give a 10% salary raise to IT department employees:

```sql
UPDATE Employees
SET Salary = Salary * 1.10
WHERE Department = 'IT';
```

**Output:**
Employee Charlie’s salary updated from 70000 to 77000.

---

## ✅ 3. `DELETE` — Remove rows from a table

Example: Delete employees in Finance department:

```sql
DELETE FROM Employees
WHERE Department = 'Finance';
```

**Output:**
Rows with Department 'Finance' deleted.

---

## ✅ 4. `MERGE` — Insert or update data conditionally (also called UPSERT)

> Note: Syntax varies by RDBMS (SQL Server, Oracle, etc.).

Example: Merge data from `NewEmployees` table into `Employees` — update if exists, else insert.

```sql
MERGE INTO Employees AS E
USING NewEmployees AS N
ON E.EmpID = N.EmpID
WHEN MATCHED THEN
  UPDATE SET E.Name = N.Name, E.Salary = N.Salary, E.Department = N.Department
WHEN NOT MATCHED THEN
  INSERT (EmpID, Name, Salary, Department)
  VALUES (N.EmpID, N.Name, N.Salary, N.Department);
```

---

## Summary Table

| Command  | Purpose                        |
| -------- | ------------------------------ |
| `INSERT` | Add new records                |
| `UPDATE` | Modify existing records        |
| `DELETE` | Remove records                 |
| `MERGE`  | Insert or update conditionally |

---
➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖➖
----

# 17. DCL & TCL
----

Used to control access permissions and security on database objects.

| Command  | Purpose                             |
| -------- | ----------------------------------- |
| `GRANT`  | Give users privileges to do actions |
| `REVOKE` | Remove privileges from users        |

---

## ✅ Example: `GRANT` and `REVOKE`

```sql
-- Grant SELECT and INSERT permission on Employees table to user 'john'
GRANT SELECT, INSERT ON Employees TO john;

-- Revoke INSERT permission from user 'john'
REVOKE INSERT ON Employees FROM john;
```

**Effect:**

* John can now only SELECT data, but not INSERT after revoke.

---

## **TCL (Transaction Control Language)**

Manages transactions to ensure data integrity.

| Command     | Purpose                                      |
| ----------- | -------------------------------------------- |
| `COMMIT`    | Save all changes made during the transaction |
| `ROLLBACK`  | Undo changes made in the current transaction |
| `SAVEPOINT` | Set a point in a transaction to rollback to  |

---

## ✅ Example: Using `COMMIT`, `ROLLBACK`, and `SAVEPOINT`

```sql
BEGIN TRANSACTION;

UPDATE Employees SET Salary = Salary * 1.1 WHERE Department = 'IT';

SAVEPOINT before_bonus;

UPDATE Employees SET Bonus = 5000 WHERE Department = 'IT';

-- Something wrong happened, rollback bonus update but keep salary update
ROLLBACK TO SAVEPOINT before_bonus;

COMMIT; -- Save salary update permanently
```

---

## Summary Table

| Command     | Category | Description                           |
| ----------- | -------- | ------------------------------------- |
| `GRANT`     | DCL      | Give privileges                       |
| `REVOKE`    | DCL      | Remove privileges                     |
| `COMMIT`    | TCL      | Save transaction changes              |
| `ROLLBACK`  | TCL      | Undo transaction changes              |
| `SAVEPOINT` | TCL      | Partial rollback point in transaction |

---
