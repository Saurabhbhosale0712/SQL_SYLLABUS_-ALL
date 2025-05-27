# Intermediate Level SQL
----

> * Aggregate Functions
> * GROUP BY & HAVING
> * Joins (INNER, LEFT, RIGHT, FULL, SELF)
> * Subqueries
> * Set Operations (`UNION`, `INTERSECT`, `EXCEPT`)

---
---

## 📊 DEMO TABLES

### 📄 Table: `employees`

| emp\_id | name    | dept\_id | salary |
| ------- | ------- | -------- | ------ |
| 1       | Alice   | 101      | 50000  |
| 2       | Bob     | 102      | 60000  |
| 3       | Charlie | 101      | 55000  |
| 4       | David   | 103      | 70000  |
| 5       | Eva     | NULL     | 40000  |

---

### 📄 Table: `departments`

| dept\_id | dept\_name |
| -------- | ---------- |
| 101      | HR         |
| 102      | IT         |
| 103      | Marketing  |
| 104      | Finance    |

---
🔴🔴🔴🔴🔴🔴
---

## 🧱 1. AGGREGATE FUNCTIONS

### 📌 Use:

To perform calculations on a group of values.

### 🔍 Query:

```sql
SELECT COUNT(*) AS total_employees,
       SUM(salary) AS total_salary,
       AVG(salary) AS average_salary,
       MIN(salary) AS min_salary,
       MAX(salary) AS max_salary
FROM employees;
```

### ✅ Output:

| total\_employees | total\_salary | average\_salary | min\_salary | max\_salary |
| ---------------- | ------------- | --------------- | ----------- | ----------- |
| 5                | 275000        | 55000           | 40000       | 70000       |


---
🔴🔴🔴🔴🔴🔴
---

## 🧱 2. GROUP BY & HAVING

### 📌 Use:

To group rows and apply aggregate functions per group.

### 🔍 Query:

```sql
SELECT dept_id, COUNT(*) AS num_employees, AVG(salary) AS avg_salary
FROM employees
GROUP BY dept_id
HAVING AVG(salary) > 50000;
```

### ✅ Output:

| dept\_id | num\_employees | avg\_salary |
| -------- | -------------- | ----------- |
| 101      | 2              | 52500       |
| 102      | 1              | 60000       |
| 103      | 1              | 70000       |


---
🔴🔴🔴🔴🔴🔴
---

## 🧱 3. JOINS

---

### ✅ INNER JOIN

```sql
SELECT e.name, d.dept_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;
```

| name    | dept\_name |
| ------- | ---------- |
| Alice   | HR         |
| Bob     | IT         |
| Charlie | HR         |
| David   | Marketing  |

---

### ✅ LEFT JOIN

```sql
SELECT e.name, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id;
```

| name    | dept\_name |
| ------- | ---------- |
| Alice   | HR         |
| Bob     | IT         |
| Charlie | HR         |
| David   | Marketing  |
| Eva     | NULL       |

---

### ✅ RIGHT JOIN

```sql
SELECT e.name, d.dept_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.dept_id;
```

| name    | dept\_name |
| ------- | ---------- |
| Alice   | HR         |
| Bob     | IT         |
| Charlie | HR         |
| David   | Marketing  |
| NULL    | Finance    |

---

### ✅ FULL JOIN *(If supported)*

```sql
SELECT e.name, d.dept_name
FROM employees e
FULL JOIN departments d ON e.dept_id = d.dept_id;
```

| name    | dept\_name |
| ------- | ---------- |
| Alice   | HR         |
| Bob     | IT         |
| Charlie | HR         |
| David   | Marketing  |
| Eva     | NULL       |
| NULL    | Finance    |

---

### ✅ SELF JOIN

```sql
SELECT e1.name AS employee, e2.name AS manager
FROM employees e1
JOIN employees e2 ON e1.emp_id != e2.emp_id AND e1.dept_id = e2.dept_id;
```

| employee | manager |
| -------- | ------- |
| Alice    | Charlie |
| Charlie  | Alice   |


---
🔴🔴🔴🔴🔴🔴
---

## 🧱 4. SUBQUERIES

---

### ✅ Subquery in WHERE

```sql
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

| name  | salary |
| ----- | ------ |
| Bob   | 60000  |
| David | 70000  |

---

### ✅ Subquery in SELECT

```sql
SELECT name,
       (SELECT dept_name FROM departments d WHERE d.dept_id = e.dept_id) AS department
FROM employees e;
```

| name    | department |
| ------- | ---------- |
| Alice   | HR         |
| Bob     | IT         |
| Charlie | HR         |
| David   | Marketing  |
| Eva     | NULL       |


---
🔴🔴🔴🔴🔴🔴
---

## 🧱 5. SET OPERATIONS

---

### ✅ UNION

```sql
SELECT name FROM employees
UNION
SELECT dept_name FROM departments;
```

| name      |
| --------- |
| Alice     |
| Bob       |
| Charlie   |
| David     |
| Eva       |
| HR        |
| IT        |
| Marketing |
| Finance   |

---

### ✅ INTERSECT

```sql
SELECT name FROM employees
INTERSECT
SELECT dept_name FROM departments;
```

| name                                                        |
| ----------------------------------------------------------- |
| (No result, since names and department names are different) |

---

### ✅ EXCEPT

```sql
SELECT name FROM employees
EXCEPT
SELECT dept_name FROM departments;
```

| name    |
| ------- |
| Alice   |
| Bob     |
| Charlie |
| David   |
| Eva     |

---

## ✅ Summary Table

| Feature             | Covered |
| ------------------- | ------- |
| Aggregate Functions | ✅       |
| GROUP BY & HAVING   | ✅       |
| Joins (All types)   | ✅       |
| Subqueries          | ✅       |
| Set Operations      | ✅       |

---
🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴🔴
