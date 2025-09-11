# **Chapter 1: CRUD in SQL**

## **1. Introduction**

CRUD stands for **Create, Read, Update, and Delete**, which are the four basic operations used to manage data in a database. In SQL (Structured Query Language), these operations allow you to interact with tables to insert, retrieve, modify, and remove data.

---

## **2. CRUD Operations Overview**

### **2.1 Create (INSERT)**

Used to add new records into a table.

```sql
INSERT INTO students (name, age, major)
VALUES ('John Doe', 20, 'Computer Science');
```

**Explanation:**

* `INSERT INTO` specifies the table.
* `(name, age, major)` are the column names.
* `VALUES` provides the data for each column.

---

### **2.2 Read (SELECT)**

Used to retrieve data from a table.

```sql
SELECT name, major
FROM students
WHERE age > 18;
```

**Explanation:**

* `SELECT` defines which columns to retrieve.
* `FROM` specifies the table.
* `WHERE` filters the data based on conditions.

---

### **2.3 Update (UPDATE)**

Used to modify existing records.

```sql
UPDATE students
SET major = 'Data Science'
WHERE name = 'John Doe';
```

**Explanation:**

* `UPDATE` specifies the table to modify.
* `SET` defines new values for the columns.
* `WHERE` ensures only the matching rows are updated.

---

### **2.4 Delete (DELETE)**

Used to remove records from a table.

```sql
DELETE FROM students
WHERE name = 'John Doe';
```

**Explanation:**

* `DELETE FROM` specifies the table.
* `WHERE` ensures only the targeted rows are deleted.

---

## **3. Best Practices**

1. Always use a `WHERE` clause when updating or deleting data to avoid affecting all rows.
2. Backup the database before performing mass updates or deletions.
3. Use transactions for critical data operations.
4. Test queries on sample data before running them in production.
