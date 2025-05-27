# Beginner Level SQL >> explanations, SQL queries, and expected outputs.

---

## ✅ Sample Tables

### 🗂️ **Table: `students`**

| id | name    | age | gender |
| -- | ------- | --- | ------ |
| 1  | Alice   | 21  | F      |
| 2  | Bob     | 22  | M      |
| 3  | Charlie | 20  | M      |
| 4  | Daisy   | 23  | F      |
| 5  | Edward  | 22  | M      |

-------
--------

## 🧾 Beginner Level SQL Topics

---

### **1. SELECT & FROM**

**Use:** Retrieve columns from a table.

```sql
SELECT name, age FROM students;
```

✅ **Output:**

| name    | age |
| ------- | --- |
| Alice   | 21  |
| Bob     | 22  |
| Charlie | 20  |
| Daisy   | 23  |
| Edward  | 22  |

---

### **2. WHERE Clause**

**Use:** Filter records based on conditions.

```sql
SELECT * FROM students WHERE gender = 'F';
```

✅ **Output:**

| id | name  | age | gender |
| -- | ----- | --- | ------ |
| 1  | Alice | 21  | F      |
| 4  | Daisy | 23  | F      |

---

### **3. DISTINCT**

**Use:** Return unique values.

```sql
SELECT DISTINCT age FROM students;
```

✅ **Output:**

| age |
| --- |
| 21  |
| 22  |
| 20  |
| 23  |

---

### **4. ORDER BY**

**Use:** Sort results by column.

```sql
SELECT * FROM students ORDER BY age ASC;
```

✅ **Output:**

| id | name    | age | gender |
| -- | ------- | --- | ------ |
| 3  | Charlie | 20  | M      |
| 1  | Alice   | 21  | F      |
| 2  | Bob     | 22  | M      |
| 5  | Edward  | 22  | M      |
| 4  | Daisy   | 23  | F      |

---

### **5. LIMIT**

**Use:** Return limited number of rows (works in MySQL/PostgreSQL).

```sql
SELECT * FROM students LIMIT 3;
```

✅ **Output:**

| id | name    | age | gender |
| -- | ------- | --- | ------ |
| 1  | Alice   | 21  | F      |
| 2  | Bob     | 22  | M      |
| 3  | Charlie | 20  | M      |

---

### **6. Comparison Operators**

#### **a. Greater than**

```sql
SELECT name, age FROM students WHERE age > 21;
```

✅ **Output:**

| name   | age |
| ------ | --- |
| Bob    | 22  |
| Daisy  | 23  |
| Edward | 22  |

#### **b. IN**

```sql
SELECT * FROM students WHERE age IN (20, 23);
```

✅ **Output:**

| id | name    | age | gender |
| -- | ------- | --- | ------ |
| 3  | Charlie | 20  | M      |
| 4  | Daisy   | 23  | F      |

#### **c. LIKE**

```sql
SELECT * FROM students WHERE name LIKE 'A%';
```

✅ **Output:**

| id | name  | age | gender |
| -- | ----- | --- | ------ |
| 1  | Alice | 21  | F      |

#### **d. IS NULL / IS NOT NULL**

Assume we add one record:

```sql
INSERT INTO students VALUES (6, NULL, 24, 'F');
```

```sql
SELECT * FROM students WHERE name IS NULL;
```

✅ **Output:**

| id | name | age | gender |
| -- | ---- | --- | ------ |
| 6  | NULL | 24  | F      |

---

### **7. Aliases**

**Use:** Rename column or table in output.

```sql
SELECT name AS student_name, age AS student_age FROM students;
```

✅ **Output:**

| student\_name | student\_age |
| ------------- | ------------ |
| Alice         | 21           |
| Bob           | 22           |
| Charlie       | 20           |
| Daisy         | 23           |
| Edward        | 22           |
| NULL          | 24           |

---

## 📝 Summary

| Topic          | SQL Keyword(s)                    | Use                           |
| -------------- | --------------------------------- | ----------------------------- |
| Select Data    | `SELECT`, `FROM`                  | Retrieve data                 |
| Filter Rows    | `WHERE`                           | Apply conditions              |
| Unique Values  | `DISTINCT`                        | Get unique column values      |
| Sorting        | `ORDER BY`                        | Sort data by one/more columns |
| Limit Results  | `LIMIT`                           | Show limited number of rows   |
| Conditions     | `>`, `<`, `IN`, `LIKE`, `IS NULL` | Apply specific filters        |
| Rename Columns | `AS`                              | Make output readable          |

---

